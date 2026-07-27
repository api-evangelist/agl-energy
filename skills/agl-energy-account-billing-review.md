---
name: Review a consenting consumer's AGL accounts, bills and concessions
description: >-
  Walk a consented AGL customer's energy accounts through balance, invoices,
  billing transactions, payment schedule and concessions — the flow behind bill
  analysis, hardship detection, concession-eligibility checks and switching
  advice. Requires CDR accreditation and a live consumer consent.
api: openapi/agl-energy-cds-energy-openapi.json
base_url: null
auth: adr-consent
scopes:
  - common:customer.basic:read
  - energy:accounts.basic:read
  - energy:accounts.detail:read
  - energy:accounts.paymentschedule:read
  - energy:accounts.concessions:read
  - energy:billing:read
operations:
  - getCustomer
  - listEnergyAccounts
  - getEnergyAccountDetail
  - getEnergyAccountBalance
  - listEnergyAccountBalancesBulk
  - getEnergyAccountInvoices
  - listEnergyAccountInvoicesBulk
  - getBillingForEnergyAccount
  - listEnergyAccountBillingBulk
  - getEnergyAccountPaymentSchedule
  - getEnergyAccountConcessions
generated: '2026-07-27'
method: generated
---

# Review a consenting consumer's AGL accounts and bills

Same gate as the usage skill: Accredited Data Recipient (or CDR representative /
sponsored party), CDR Register certificates, mutual TLS, FAPI 1.0 Advanced, and a
live consumer consent obtained through AGL's own flow. The base URI comes from
the CDR Register — do not hard-code it, and do not point at
`https://public.cdr.agl.com.au`, which 404s for `/cds-au/v1/energy/*`.

Ask the consumer only for the scopes you will actually use. Each one below is a
separate consent the consumer can decline, and `energy:billing:read` is a single
scope covering balances, invoices and transactions.

## Steps

1. **Identify the consumer (optional).** `getCustomer` —
   `GET /common/customer` on the *Common* API, `x-v: 1`, scope
   `common:customer.basic:read`. `customerUType` discriminates `person` from
   `organisation`. Use `getCustomerDetail` (`x-v: 2`,
   `common:customer.detail:read`) only if you genuinely need contact details.

2. **List the accounts.** `listEnergyAccounts` — `GET /energy/accounts`,
   `x-v: 2`, scope `energy:accounts.basic:read`. Query parameter `open-status`
   (`ALL`, `OPEN`, `CLOSED`, default `ALL`) — pass `OPEN` unless you are doing
   historical analysis. Each record gives `accountId`, `accountNumber`,
   `displayName`, `openStatus`, `creationDate` and a `plans[]` overview.

3. **Get the account detail.** `getEnergyAccountDetail` —
   `GET /energy/accounts/{accountId}`, **`x-v: 4`**, scope
   `energy:accounts.detail:read`. This is the join point of the whole model: its
   `plans[]` carry `planOverview`/`planDetail` and `servicePointIds[]`, which is
   how you get from a billing account to the physical connections used by
   `skills/agl-energy-consumer-usage-and-der.md`.

4. **Balance.** `getEnergyAccountBalance` —
   `GET /energy/accounts/{accountId}/balance`, `x-v: 1`, scope
   `energy:billing:read`. For every account at once use
   `listEnergyAccountBalancesBulk` (`GET /energy/accounts/balances`), or
   `listEnergyAccountBalancesSpecificAccounts`
   (`POST /energy/accounts/balances` with a `RequestAccountIdListV1` body — a
   read, despite the POST).

5. **Invoices.** `getEnergyAccountInvoices` —
   `GET /energy/accounts/{accountId}/invoices`, `x-v: 1`, scope
   `energy:billing:read`. Filters `oldest-date` / `newest-date` are on the
   *issue* date. Each `EnergyInvoice` gives `invoiceNumber`, `issueDate`,
   `dueDate`, `period`, `invoiceAmount`, `gstAmount`, `payOnTimeDiscount`,
   `balanceAtIssue`, `paymentStatus`, `servicePoints[]`, and `electricity` /
   `gas` / `accountCharges` breakdowns. Bulk and id-list variants exist
   (`listEnergyAccountInvoicesBulk`, `listEnergyInvoicesForSpecificAccounts`).

6. **Billing transactions.** `getBillingForEnergyAccount` —
   `GET /energy/accounts/{accountId}/billing`, **`x-v: 3`**, scope
   `energy:billing:read`. `EnergyBillingTransactionV3.transactionUType`
   discriminates `usage`, `demand`, `onceOff`, `otherCharges` and `payment`;
   read the matching branch, never all of them. Bulk and id-list variants:
   `listEnergyAccountBillingBulk`, `listEnergyAccountBillingForSpecificAccounts`.

7. **Payment schedule.** `getEnergyAccountPaymentSchedule` —
   `GET /energy/accounts/{accountId}/payment-schedule`, `x-v: 1`, scope
   `energy:accounts.paymentschedule:read`. `paymentScheduleUType` selects
   `cardDebit`, `directDebit`, `digitalWallet` or `manualPayment`. This is the
   strongest hardship signal in the dataset when read alongside `balance`.

8. **Concessions.** `getEnergyAccountConcessions` —
   `GET /energy/accounts/{accountId}/concessions`, `x-v: 1`, scope
   `energy:accounts.concessions:read`. `EnergyConcession` gives `type`,
   `displayName`, `startDate`/`endDate`, `discountFrequency`, `amount`,
   `percentage`, `appliedTo` and `additionalInfoUri`. Absence of a concession on
   an account whose profile suggests eligibility is the actionable finding.

9. **Compare against the market.** Take the plan from step 3 and diff it against
   the anonymous plan catalogue —
   `skills/agl-energy-compare-retail-plans.md`. That half needs no consent, so do
   the modelling there and keep the consented data set minimal.

## Conventions that will bite you

- **`x-v` varies sharply here:** accounts list `2`, account detail `4`, billing
  `3`, everything else `1`. Getting it wrong is HTTP 406
  `urn:au-cds:error:cds-all:Header/UnsupportedVersion` — the most common failure
  against any CDR data holder.
- **`accountId` is opaque and consent-scoped.** It is deliberately not
  `accountNumber`, and it is not stable across consent arrangements.
- **404 vs 422 for the same condition.** A bad `accountId` in the URI is HTTP 404
  `urn:au-cds:error:cds-energy:Authorisation/InvalidEnergyAccount` (permanent) or
  `.../UnavailableEnergyAccount` (retryable); the identical pair returns as HTTP
  422 when the id came from a POST body. Retry only on `Unavailable*`.
- **Consent revocation is a 403,** `urn:au-cds:error:cds-all:Authorisation/RevokedConsent`.
  Stop and honour your CDR deletion/de-identification obligations; do not retry.
- **Pagination:** `page-size` max 1000, `page` past the end returns 422
  `urn:au-cds:error:cds-all:Field/InvalidPage`. Follow `links.next`.
- Amounts are strings. Parse as decimal, not float, and read `gstAmount`
  separately rather than deriving it.
- Every operation here is a read: no idempotency key, no webhooks, no
  rate-limit headers to back off against.

## Related

- `scopes/agl-energy-scopes.yml`, `authentication/agl-energy-authentication.yml`
- `errors/agl-energy-problem-types.yml`, `conventions/agl-energy-conventions.yml`
- `data-model/agl-energy-data-model.yml`

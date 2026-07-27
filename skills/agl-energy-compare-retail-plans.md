---
name: Compare AGL retail energy plans and tariffs
description: >-
  Pull AGL's full retail plan catalogue as Consumer Data Right Product Reference
  Data and drill into the tariff structure of any plan — rates, supply charges,
  solar feed-in, discounts, fees and eligibility. Completely anonymous: no key,
  no accreditation, no consumer consent.
api: openapi/agl-energy-cds-energy-openapi.json
base_url: https://cdr.energymadeeasy.gov.au/agl/cds-au/v1
auth: none
operations:
  - listEnergyPlans
  - getEnergyPlanDetail
generated: '2026-07-27'
method: generated
---

# Compare AGL retail energy plans

This is the only genuinely open dataset carrying AGL's name — 1,343 plans as of
2026-07-27. The critical thing to get right before you write a line of code:
**AGL does not serve this**. In CDR banking every ADI serves its own Product
Reference Data from its own domain. In CDR energy the Australian Energy Regulator
hosts every retailer's PRD centrally, under a per-brand path. AGL's own CDR base
URI returns HTTP 404 for `/cds-au/v1/energy/plans`.

## Before you start

- Base URL: `https://cdr.energymadeeasy.gov.au/agl/cds-au/v1` (AER-operated)
- No credential. CORS is open (`Access-Control-Allow-Origin: *`), so this is callable straight from a browser.
- The `x-v` version differs per endpoint. Get it wrong and you get a 406.

## Steps

1. **Call `listEnergyPlans`** — `GET /energy/plans` with header `x-v: 1`.
   Query parameters the spec declares: `type` (`STANDING`, `MARKET`,
   `REGULATED`, `ALL`), `fuelType` (`ELECTRICITY`, `GAS`, `DUAL`, `ALL`),
   `effective` (`CURRENT`, `FUTURE`, `ALL`), `updated-since`, `brand`, plus
   `page` and `page-size`.
   Read `meta.totalRecords` and `meta.totalPages` first — 1,343 records at the
   default page size of 25 is 54 pages. Set `page-size` up to the CDS maximum of
   `1000` to cut round trips.

2. **Page through.** Follow `links.next` until it is absent. Do not compute your
   own page numbers past the end: requesting a page beyond the last one returns
   **HTTP 422** `urn:au-cds:error:cds-all:Field/InvalidPage`, and a `page-size`
   above 1000 returns **HTTP 400** `urn:au-cds:error:cds-all:Field/InvalidPageSize`.

3. **Filter down.** Each `EnergyPlan` carries `planId`, `displayName`, `type`,
   `fuelType`, `customerType` (`RESIDENTIAL` / `BUSINESS`), `brand`, `brandName`,
   `effectiveFrom`/`effectiveTo`, and a `geography` block with `includedPostcodes`
   / `excludedPostcodes` / `distributors`. Narrow on geography first — a plan
   that is not offered in your consumer's distribution zone is noise.

4. **Call `getEnergyPlanDetail`** — `GET /energy/plans/{planId}` with header
   **`x-v: 3`** (not 1). A verified example id is `AGL1067320MRE2@EME`.
   The response adds `electricityContract` and/or `gasContract`, plus
   `meteringCharges`.

5. **Read the tariff structure** inside the contract object:
   - `pricingModel` — one of `SINGLE_RATE`, `SINGLE_RATE_CONT_LOAD`,
     `TIME_OF_USE`, `TIME_OF_USE_CONT_LOAD`, `FLEXIBLE`, `FLEXIBLE_CONT_LOAD`,
     `QUOTA`. Gas contracts must be `SINGLE_RATE`.
   - `tariffPeriod[]` — `dailySupplyChargeType`, `dailySupplyCharge`,
     `bandedDailySupplyCharges[]`, then `rateBlockUType` selecting
     `singleRate` (with `generalUnitPrice`, `rates[]`, `period`) or
     `timeOfUseRates[]` (with `rates[]`, `timeOfUse[]`, `type`), plus
     `demandCharges[]`.
   - `solarFeedInTariff[]` — `scheme`, `payerType`, then `tariffUType`
     selecting `singleTariff.rates[]` or `timeVaryingTariffs[]`. This is where
     a DER-owning household's economics live.
   - `discounts[]` — `type` is `CONDITIONAL` / `GUARANTEED` / `OTHER`, and
     `methodUType` selects `percentOfBill`, `percentOfUse`, `fixedAmount` or
     `percentOverThreshold`. Also `incentives[]`, `fees[]`, `eligibility[]`,
     `greenPowerCharges[]`, `intrinsicGreenPower`, `controlledLoad[]`.

6. **Normalise before comparing.** Rate amounts are strings, not numbers. Always
   read the discriminator (`rateBlockUType`, `tariffUType`, `methodUType`) before
   reaching into a branch, honour the contract-level `timeZone` and `isFixed`,
   and compare like-for-like `tariffPeriod` windows only. Rate arrays are ordered
   by usage volume — a block/step tariff, not a single price.

## Conventions that will bite you

- **The x-v differs per endpoint**: list is `1`, detail is `3`. This is the single
  most common integration failure against any CDR data holder.
- `planId` is not opaque here — it is public and stable enough to cache, but
  respect `lastUpdated` and re-pull rather than treating a cached plan as current.
- Everything is a read. There is no idempotency key because there is nothing to
  make idempotent, and there are no rate-limit headers to back off against.
- The AER host is AWS API Gateway behind CloudFront. A 5xx here is an AER
  incident, not an AGL one — check `https://cdr.energymadeeasy.gov.au/agl/cds-au/v1/discovery/status`,
  which is a *different* status feed from AGL's own.

## Related

- `data-model/agl-energy-data-model.yml` — how `planId` links into the consumer's account graph.
- `skills/agl-energy-account-billing-review.md` — matching a real consumer's account to a plan requires consent.

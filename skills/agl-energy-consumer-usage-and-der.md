---
name: Read a consenting consumer's AGL electricity usage and DER
description: >-
  Walk a consented AGL customer's service points, meter reads and distributed
  energy resource register — the flow behind any solar/battery analytics, VPP
  onboarding or switching recommendation. Requires CDR accreditation, mutual TLS
  and a live consumer consent; there is no anonymous path and no sandbox.
api: openapi/agl-energy-cds-energy-openapi.json
base_url: null
auth: adr-consent
scopes:
  - energy:electricity.servicepoints.basic:read
  - energy:electricity.servicepoints.detail:read
  - energy:electricity.usage:read
  - energy:electricity.der:read
operations:
  - listElectricityServicePoints
  - getElectricityServicePointDetail
  - getElectricityServicePointUsage
  - listElectricityUsageBulk
  - listElectricityUsageForServicePoints
  - getElectricityDERForServicePoint
  - listElectricityDERBulk
  - listElectricityDERForSpecificServicePoints
generated: '2026-07-27'
method: generated
---

# Read a consenting consumer's AGL usage and DER

## You cannot start this skill without accreditation

There is no key to request and no sandbox to try. To reach any of these
operations you must be an Accredited Data Recipient under the Consumer Data
Right, or operate as a CDR representative or sponsored/affiliate party beneath
one. That means ACCC accreditation, CDR Rules Schedule 2 information-security
controls, CDR Register-issued transport and signing certificates, a registered
software product, and then that individual consumer's authorisation through AGL's
own consent flow.

**The base URI is not published.** AGL's consumer-data base URI is distributed to
accredited participants through the CDR Register — resolve it from the Register,
never hard-code it. `https://public.cdr.agl.com.au` is the *public* base URI and
returns HTTP 404 for `/cds-au/v1/energy/*`.

Auth is FAPI 1.0 Advanced: mutual TLS, `private_key_jwt` client authentication,
Pushed Authorization Requests, mTLS-bound access tokens. See
`authentication/agl-energy-authentication.yml`.

## Steps

1. **Pre-flight.** Run `skills/agl-energy-check-cdr-availability.md` first. If
   AGL's consent flow is in a scheduled window, a new consent will fail.

2. **Call `listElectricityServicePoints`** — `GET /energy/electricity/servicepoints`,
   header `x-v: 2`, scope `energy:electricity.servicepoints.basic:read`.
   Returns `EnergyServicePointV2` records: `servicePointId` (tokenised — this is
   the id every downstream call takes), `nationalMeteringId` (the real NMI),
   `servicePointClassification`, `servicePointStatus`, `jurisdictionCode`,
   `isGenerator`, `validFromDate`, `lastUpdateDateTime`, `consumerProfile`.
   `isGenerator: true` is your first signal that DER is worth pulling.

3. **Call `getElectricityServicePointDetail`** —
   `GET /energy/electricity/servicepoints/{servicePointId}`, header `x-v: 2`,
   scope `energy:electricity.servicepoints.detail:read`.
   Adds `meters[]` (with `registers[]`), `distributionLossFactor`, `location`,
   `relatedParticipants[]` (distributor and financially responsible market
   participant) and `specifications`. You need `registers[].registerSuffix` to
   interpret the reads in the next step.

4. **Pull the reads.** Three shapes, pick one:
   - `getElectricityServicePointUsage` — `GET /energy/electricity/servicepoints/{servicePointId}/usage`, `x-v: 1`, one service point.
   - `listElectricityUsageBulk` — `GET /energy/electricity/servicepoints/usage`, `x-v: 1`, every service point under the consent.
   - `listElectricityUsageForServicePoints` — `POST /energy/electricity/servicepoints/usage`, `x-v: 1`, body `RequestServicePointIdListV1` with an explicit id list. **This POST is a read, not a write** — it exists only because an id list can outgrow a query string.

   Query parameters on all three: `interval-reads` (`NONE`, `MIN_30`, `FULL` —
   default `NONE`, so you get accumulation reads unless you ask for interval
   data), `oldest-date`, `newest-date`, `page`, `page-size`.

5. **Interpret each `EnergyUsageRead`.** `readUType` is the discriminator: read
   `basicRead` for an accumulation meter, `intervalRead` for interval data.
   Fields: `servicePointId`, `registerId`, `registerSuffix`, `meterId`,
   `controlledLoad`, `readStartDate`, `readEndDate`, `unitOfMeasure`.
   `intervalRead.readIntervalLength` tells you the interval size; join
   `registerSuffix` back to step 3 to know whether a series is consumption,
   controlled load or export.

6. **Pull the DER register.** `getElectricityDERForServicePoint`
   (`GET /energy/electricity/servicepoints/{servicePointId}/der`, `x-v: 1`),
   `listElectricityDERBulk`, or `listElectricityDERForSpecificServicePoints`
   (POST id list). Scope `energy:electricity.der:read`.
   `EnergyDerRecord` gives `approvedCapacity`, `availablePhasesCount`,
   `installedPhasesCount`, `islandableInstallation`,
   `hasCentralProtectionControl`, `protectionMode` and `acConnections[]`. Each AC
   connection carries `equipmentType`, `manufacturerName`, `inverterSeries`,
   `inverterModelNumber`, `inverterDeviceCapacity`, `commissioningDate`,
   `status`, and a nested `derDevices[]` array — inverter and battery detail, not
   just a solar yes/no.

7. **Reconcile export against feed-in.** Join the export register series from
   step 5 to the plan's `solarFeedInTariff` (see
   `skills/agl-energy-compare-retail-plans.md`, anonymous) to model what the
   consumer is actually earning.

## Conventions that will bite you

- **`x-v` per endpoint, not per API.** Service points are `2`, usage and DER are
  `1`. A wrong version is HTTP 406 `urn:au-cds:error:cds-all:Header/UnsupportedVersion`.
- **`servicePointId` is consent-scoped and opaque.** It is not the NMI and it is
  not stable across consent arrangements. Never use it as a durable primary key
  in your own store — key on the consent arrangement plus the id.
- **AEMO is in the request path.** NMI standing data and metering data come from
  AEMO as secondary data holder. When a failure originates there, the CDS error
  object carries `isSecondaryDataHolderError: true` — surface that to the user
  rather than blaming AGL.
- **Errors are CDR URNs, not RFC 9457.** A bad `servicePointId` in the URI is
  HTTP 404 `urn:au-cds:error:cds-energy:Authorisation/InvalidServicePoint`
  (permanent) or `.../UnavailableServicePoint` (retryable). The same pair appears
  as HTTP 422 when the id came from a POST body. Retry only on the
  `Unavailable*` variants.
- **Pagination:** `page-size` maximum is 1000 (400 above it); paging past the end
  is 422 `urn:au-cds:error:cds-all:Field/InvalidPage`. Follow `links.next`.
- **Consent can vanish mid-run.** HTTP 403
  `urn:au-cds:error:cds-all:Authorisation/RevokedConsent` means stop, delete per
  your CDR obligations, and do not retry.
- No idempotency key exists and none is needed — every operation here is a read.

## Related

- `scopes/agl-energy-scopes.yml`, `authentication/agl-energy-authentication.yml`
- `conventions/agl-energy-conventions.yml`, `errors/agl-energy-problem-types.yml`
- `data-model/agl-energy-data-model.yml`

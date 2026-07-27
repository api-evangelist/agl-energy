---
name: Check AGL CDR availability before a data request
description: >-
  Read AGL's live Consumer Data Right status and scheduled outage windows before
  attempting any consumer data sharing, so a request is not fired into a
  maintenance window. Fully anonymous — no accreditation, no key, no consent.
api: openapi/agl-energy-cds-common-openapi.json
base_url: https://public.cdr.agl.com.au/cds-au/v1
auth: none
operations:
  - getStatus
  - getOutages
generated: '2026-07-27'
method: generated
---

# Check AGL CDR availability

AGL has no status page. What it has instead is the Consumer Data Standards
operations pair, served anonymously from its CDR public base URI. This is the
only AGL-operated API surface any client can call without accreditation, and it
is the correct pre-flight for anything else in this repo.

## Before you start

- Base URL: `https://public.cdr.agl.com.au/cds-au/v1`
- No credential of any kind. The `x-v` header is mandatory but is not a credential.
- Optional but recommended: send `x-fapi-interaction-id` as an RFC 4122 UUID. AGL echoes it back, and it is your correlation id for support.

## Steps

1. **Call `getStatus`** — `GET /discovery/status` with header `x-v: 1`.
   Read `data.status`. It is one of `OK`, `PARTIAL_FAILURE`, `UNAVAILABLE`,
   `SCHEDULED_OUTAGE`. `data.explanation` carries the human reason and
   `data.updateTime` tells you how fresh the assessment is.
   If the status is anything other than `OK`, do not start a consent flow.

2. **Call `getOutages`** — `GET /discovery/outages` with header `x-v: 1`.
   You get `data.outages[]`, each with `outageTime` (ISO 8601 UTC), `duration`
   (an ISO 8601 duration such as `PT8H`), `isPartial`, and `explanation`.
   AGL's windows read *"AGL CDR Consent flow will be unavailable. Customer will
   be unable to register and create new arrangements for CDR."* — so an outage
   here blocks **new consent**, not necessarily existing data calls. Read the
   explanation text rather than assuming.

3. **Decide.** Compare `outageTime + duration` against the moment you intend to
   send the consumer into AGL's consent flow. If it lands inside a window, defer
   and tell the consumer why. AGL's observed pattern is a weekly Friday-evening
   AEST window of about eight hours.

## Conventions that will bite you

- Omit `x-v` and you get **HTTP 400** `urn:au-cds:error:cds-all:Header/InvalidVersion`.
- Send a version AGL does not serve and you get **HTTP 406**
  `urn:au-cds:error:cds-all:Header/UnsupportedVersion`. Both of these endpoints
  are at version `1` today.
- Errors are **not** RFC 9457. The body is `{"errors":[{"code","title","detail"}]}`.
  See `errors/agl-energy-problem-types.yml`.
- Success bodies are always `{"data":…,"links":…,"meta":…}`.
- The host root (`https://public.cdr.agl.com.au/`) returns nginx **404**. Only
  `/cds-au/v1/discovery/*` resolves. That is correct, not an outage.

## What this skill cannot do

Nothing here reads a consumer's data. `/common/customer` on this base URI returns
404 by design — customer endpoints are consumer-authorised and live on a base URI
distributed through the CDR Register. See
`skills/agl-energy-consumer-usage-and-der.md`.

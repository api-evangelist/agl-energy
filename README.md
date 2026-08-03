# AGL Energy (agl-energy)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AGL Energy Limited (ASX:AGL) is Australia's oldest listed company — founded in Sydney in 1837 as the Australian Gas Light Company — and one of the country's largest integrated energy businesses, retailing electricity, gas, broadband and mobile to roughly four million customer accounts while owning the nation's largest electricity generation portfolio. Its API posture is entirely a product of regulation: AGL publishes no public developer portal and no first-party OpenAPI, but it is a designated Consumer Data Right energy data holder and that mandate is genuinely, verifiably live.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/agl-energy/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/agl-energy/refs/heads/main/apis.yml)

## Tags

- Energy
- Australia
- Utilities
- Electricity
- Gas
- Energy Retailer
- Consumer Data Right
- CDR
- Smart Metering
- Solar
- DER
- Renewables
- Energy Markets

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## Mandate Posture

| Question | Answer |
| --- | --- |
| Mandate regime | Consumer Data Right, extended from banking to energy (ACCC / Data Standards Body) |
| Mandate status | **Live and implemented** — verified by anonymous HTTP calls, not by a compliance page |
| Data standard | CDR Consumer Data Standards — Energy API v1.36.0 and Common API v1.36.0 |
| Consumer data API | Yes — accredited-only, consumer-authorised, mTLS, base URI unpublished |
| Open market data | No — AGL publishes no grid or market data of its own |
| Open product data | Yes, but the AER hosts it, not AGL |
| Access gate | Accredited Data Recipient status under the CDR |
| Developer portal | None public. `apideveloper.agl.com.au` resolves but returns HTTP 403. |

AGL is the control case for whether a statutory data mandate is replicable. The same Act, regulator, standards body and security profile that produced Australia's byte-for-byte fifty-bank banking contract were transplanted into energy — and on AGL the transplant took. But the mandate replicated without the market replicating: AGL ships no developer programme, no SDK, no first-party specification, and one abandoned GitHub organization. Every machine-readable thing it publishes, it publishes because it was told to.

## APIs

### AGL CDR Energy API

AGL's mandated Consumer Data Right energy data-sharing surface — service points, electricity usage, DER register detail, accounts, balances, invoices, billing, payment schedules and concessions. Consumer-authorised: only an Accredited Data Recipient holding CDR Register-issued certificates can call it, over mTLS, after the consumer completes AGL's consent flow. The base URI is distributed through the CDR Register rather than published.

- **Human URL:** [https://consumerdatastandardsaustralia.github.io/standards/](https://consumerdatastandardsaustralia.github.io/standards/)

#### Tags

- Consumer Data Right
- CDR
- Energy
- Usage
- Billing
- Accounts
- DER
- Australia

#### Properties

- [OpenAPI](openapi/agl-energy-cds-energy-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://consumerdatastandardsaustralia.github.io/standards/)
- [Documentation](https://www.cdr.gov.au/rollout/cdr-energy-sector)
- [Documentation](https://www.aemo.com.au/energy-systems/electricity/cdr-at-aemo/about-cdr)

### AGL CDR Discovery (Common) API

The unauthenticated half of AGL's CDR implementation, served from its CDR public base URI. `GET /cds-au/v1/discovery/status` returned HTTP 200 with `{"status":"OK","explanation":"All services operational"}` on 2026-07-27, and `GET /cds-au/v1/discovery/outages` returned six scheduled outage windows describing unavailability of the AGL CDR consent flow. These two endpoints are the only anonymously reachable API AGL itself operates.

- **Human URL:** [https://consumerdatastandardsaustralia.github.io/standards/](https://consumerdatastandardsaustralia.github.io/standards/)
- **Base URL:** `https://public.cdr.agl.com.au/cds-au/v1`

#### Tags

- Consumer Data Right
- CDR
- Discovery
- Status
- Outages
- Australia

#### Properties

- [OpenAPI](openapi/agl-energy-cds-common-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://consumerdatastandardsaustralia.github.io/standards/)

### AGL Energy Product Reference Data (PRD) API

AGL's retail energy plans as anonymous, machine-readable CDR Product Reference Data — 1,343 plans across 135 pages, verified live on 2026-07-27 with `x-v: 1` and `Access-Control-Allow-Origin: *`. Critically, this endpoint is operated by the Australian Energy Regulator, not by AGL. AGL's own public base URI returns HTTP 404 for `/cds-au/v1/energy/plans`. In CDR banking every bank serves its own product API; in CDR energy the regulator serves everyone's.

- **Human URL:** [https://www.aer.gov.au/energy-product-reference-data](https://www.aer.gov.au/energy-product-reference-data)
- **Base URL:** `https://cdr.energymadeeasy.gov.au/agl/cds-au/v1`

#### Tags

- Consumer Data Right
- CDR
- Product Reference Data
- Tariffs
- Plans
- Electricity
- Gas
- Australia

#### Properties

- [OpenAPI](openapi/agl-energy-cds-energy-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://www.aer.gov.au/energy-product-reference-data)
- [API Reference](https://cdr.energymadeeasy.gov.au/agl/cds-au/v1/energy/plans)

## Common Properties

- [Website](https://www.agl.com.au/)
- [Consumer Data Right Policy](https://www.agl.com.au/consumer-data-right-policy)
- [Manage Consumer Data Right](https://www.agl.com.au/help-support/account-setup-management/manage-consumer-data-right)
- [Community Forum](https://neighbourhood.agl.com.au/)
- [GitHub Organization](https://github.com/AGLEnergy)
- [LinkedIn](https://www.linkedin.com/company/agl-energy)
- [CDR Register](https://api.cdr.gov.au/cdr-register/v1/all/data-holders/brands/summary)

## Review

See [review.yml](review.yml) for the full mandate-versus-implementation record, every probed URL with its HTTP status, the consumer-data versus market-data split, the access gate, and the authentication model.

## Maintainers

- Kin Lane — kin@apievangelist.com

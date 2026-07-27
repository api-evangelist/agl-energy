# AGL Energy (agl-energy)

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

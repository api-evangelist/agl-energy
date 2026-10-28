# AGL Energy

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AGL Energy Limited (ASX:AGL) is Australia's oldest listed company — founded in Sydney in 1837 as the Australian Gas Light Company — and one of the country's largest integrated energy businesses, retailing electricity, gas, broadband and mobile to roughly four million customer accounts while owning the nation's largest electricity generation portfolio (Bayswater and Loy Yang A coal, gas peakers, hydro, wind, utility-scale solar and grid-scale batteries). It sits at both ends of the Australian value chain: generator and wholesale market participant in the NEM, and the retailer of record that holds the customer relationship, the billing account and the metering data. Its API posture is entirely a product of regulation, not of product strategy. AGL publishes no public developer portal and no self-serve API programme — apideveloper.agl.com.au resolves through Akamai but returns HTTP 403 to every anonymous client, and agl.com.au itself is bot-blocked at 403. What AGL does expose is the Consumer Data Right: it is a designated CDR energy data holder, listed on the CDR Register under brand "AGL" with public base URI https://public.cdr.agl.com.au, and that surface is real and verified — GET /cds-au/v1/discovery/status and /cds-au/v1/discovery/outages both return HTTP 200 with conformant Consumer Data Standards envelopes, and AGL's own outage notices describe scheduled downtime of the "AGL CDR Consent flow". Its energy plan Product Reference Data is genuinely open and anonymous — 1,343 plans at https://cdr.energymadeeasy.gov.au/agl/cds-au/v1/energy/plans — but that endpoint is operated centrally by the Australian Energy Regulator, not by AGL, which is the structural difference from CDR banking where every bank serves its own PRD. Consumer usage, billing, service point, DER and account data is available only to Accredited Data Recipients, over mTLS, under a consumer authorisation, with the base URI distributed through the CDR Register rather than published. AGL is therefore open on product data, closed to everyone but accredited recipients on consumer data, and silent everywhere else — it publishes no open grid or market data of its own. It is also migrating around four million customer services onto the Kaluza platform under a A$150m, 20 percent stake taken in 2024, so the retail data layer behind these mandated endpoints is being rebuilt on a third-party energy operating system.

**Areas:** Energy · Australia · Utilities · Electricity · Gas · Energy Retailer · Consumer Data Right · CDR · Smart Metering · Solar · DER · Renewables

## AGL Energy on APIs.io

Everything profiled for AGL Energy is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/agl-energy/](https://apis.io/providers/agl-energy/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [AGL Energy Data Holder Customers API](https://apis.io/apis/agl-energy/agl-energy-data-holder-customers-api/) — Data Holder Customer endpoints
- [AGL Energy Data Holder Operations API](https://apis.io/apis/agl-energy/agl-energy-data-holder-operations-api/) — Data Holder Operations endpoints
- [AGL Energy Distributed Energy Resources API](https://apis.io/apis/agl-energy/agl-energy-distributed-energy-resources-api/) — Distributed Energy Resource endpoints
- [AGL Energy Electricity Service Points API](https://apis.io/apis/agl-energy/agl-energy-electricity-service-points-api/) — Electricity Service Point endpoints
- [AGL Energy Electricity Usage API](https://apis.io/apis/agl-energy/agl-energy-electricity-usage-api/) — Electricity Usage endpoints
- [AGL Energy Account Balances API](https://apis.io/apis/agl-energy/agl-energy-energy-account-balances-api/) — Energy Account Balance endpoints
- [AGL Energy Account Billing API](https://apis.io/apis/agl-energy/agl-energy-energy-account-billing-api/) — Energy Account Billing endpoints
- [AGL Energy Accounts API](https://apis.io/apis/agl-energy/agl-energy-energy-accounts-api/) — Energy Account endpoints
- [AGL Energy Plans API](https://apis.io/apis/agl-energy/agl-energy-energy-plans-api/) — Energy Plan endpoints

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [agl-energy](https://apis.io/servers/agl-energy/agl-energy/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Agl Energy Agentic Access](https://apis.io/agentic-access/agl-energy/agl-energy-agentic-access/)

#### JSON Schema (19)

The data models the API reads and writes.

- [EnergyAccountDetailResponseV4](https://apis.io/schemas/agl-energy/agl-energy-energy-account-detail-response-v4/)
- [EnergyAccountListResponseV2](https://apis.io/schemas/agl-energy/agl-energy-energy-account-list-response-v2/)
- [EnergyBalanceListResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-balance-list-response/)
- [EnergyBalanceResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-balance-response/)
- [EnergyBillingListResponseV3](https://apis.io/schemas/agl-energy/agl-energy-energy-billing-list-response-v3/)
- [EnergyConcessionsResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-concessions-response/)
- [EnergyDerDetailResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-der-detail-response/)
- [EnergyDerListResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-der-list-response/)
- [EnergyInvoiceListResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-invoice-list-response/)
- [EnergyPaymentScheduleResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-payment-schedule-response/)
- [EnergyPlanListResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-plan-list-response/)
- [EnergyPlanResponseV3](https://apis.io/schemas/agl-energy/agl-energy-energy-plan-response-v3/)
- [EnergyServicePointDetailResponseV2](https://apis.io/schemas/agl-energy/agl-energy-energy-service-point-detail-response-v2/)
- [EnergyServicePointListResponseV2](https://apis.io/schemas/agl-energy/agl-energy-energy-service-point-list-response-v2/)
- [EnergyUsageListResponse](https://apis.io/schemas/agl-energy/agl-energy-energy-usage-list-response/)
- [ResponseCommonCustomer](https://apis.io/schemas/agl-energy/agl-energy-response-common-customer/)
- [ResponseCommonCustomerDetailV2](https://apis.io/schemas/agl-energy/agl-energy-response-common-customer-detail-v2/)
- [ResponseCommonDiscoveryStatus](https://apis.io/schemas/agl-energy/agl-energy-response-common-discovery-status/)
- [ResponseDiscoveryOutagesList](https://apis.io/schemas/agl-energy/agl-energy-response-discovery-outages-list/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Agl Energy Context](https://apis.io/jsonld/agl-energy/agl-energy-context/)

#### Collections (2)

Postman and Open Collection request collections.

- [CDR Common API](https://apis.io/collections/agl-energy/open-agl-energy-cds-common/)
- [CDR Energy API](https://apis.io/collections/agl-energy/open-agl-energy-cds-energy/)

#### Overlays (9)

OpenAPI overlays, including conversational phrasing for each operation.

- [Agl Energy Data Holder Customers Overlay](https://apis.io/overlays/agl-energy/agl-energy-data-holder-customers-overlay/)
- [Agl Energy Data Holder Operations Overlay](https://apis.io/overlays/agl-energy/agl-energy-data-holder-operations-overlay/)
- [Agl Energy Distributed Energy Resources Overlay](https://apis.io/overlays/agl-energy/agl-energy-distributed-energy-resources-overlay/)
- [Agl Energy Electricity Service Points Overlay](https://apis.io/overlays/agl-energy/agl-energy-electricity-service-points-overlay/)
- [Agl Energy Electricity Usage Overlay](https://apis.io/overlays/agl-energy/agl-energy-electricity-usage-overlay/)
- [Agl Energy Energy Account Balances Overlay](https://apis.io/overlays/agl-energy/agl-energy-energy-account-balances-overlay/)
- [Agl Energy Energy Account Billing Overlay](https://apis.io/overlays/agl-energy/agl-energy-energy-account-billing-overlay/)
- [Agl Energy Energy Accounts Overlay](https://apis.io/overlays/agl-energy/agl-energy-energy-accounts-overlay/)
- [Agl Energy Energy Plans Overlay](https://apis.io/overlays/agl-energy/agl-energy-energy-plans-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [AGL Energy API Rules](https://apis.io/rules/agl-energy/agl-energy-rules/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Agl Energy Authentication](https://apis.io/security/agl-energy/agl-energy-authentication/)
- [Agl Energy Domain Security](https://apis.io/security/agl-energy/agl-energy-domain-security/)
- [Agl Energy Vulnerability Disclosure](https://apis.io/security/agl-energy/agl-energy-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Agl Energy Scopes](https://apis.io/scopes/agl-energy/agl-energy-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Agl Energy Plans Pricing](https://apis.io/plans/agl-energy/agl-energy-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Agl Energy Rate Limits](https://apis.io/rate-limits/agl-energy/agl-energy-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.agl.com.au/)
- [Documentation](https://www.agl.com.au/consumer-data-right-policy)
- [API reference](https://consumerdatastandardsaustralia.github.io/standards/#energy-apis)
- [Support](https://www.agl.com.au/help-support/account-setup-management/manage-consumer-data-right)
- [Security](https://www.agl.com.au/terms-conditions/responsible-disclosure-policy)
- [Terms of service](https://www.agl.com.au/terms-conditions/online-services)
- [Privacy policy](https://www.agl.com.au/privacy-policy)
- [GitHub](https://github.com/AGLEnergy)
- [Status](https://public.cdr.agl.com.au/cds-au/v1/discovery/status)

## API Evangelist rating

**Kin Score 44.8** — developing · rubric 0.24.0 · scored 2026-10-20

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 26.0 |
| Contract governance | 33.3 |
| Operational transparency | 76.3 |
| Developer ergonomics | 35.1 |
| Access clarity | 34.2 |

**Agent readiness 26.4** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 39.3** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/agl-energy/](https://apis.io/providers/agl-energy/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for AGL Energy: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/agl-energy/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

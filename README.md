# SparkyFitness

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

SparkyFitness is a self-hosted, open-source health and fitness tracker for food, exercise, water, sleep, mood, body measurements and medications, built for families and designed around an AI assistant. It ships a full Express 5 + PostgreSQL REST API (423 operations across 328 paths, described by an OpenAPI 3.0 document the server generates and serves at /api/api-docs/json) and — unusually for a project this size — a first-party Model Context Protocol server running in-process at POST /mcp, exposing 56 tools for logging and reading a user's own health data under PostgreSQL row-level security. It synchronises with Garmin, Fitbit, Withings, Strava, Polar, Oura, Hevy, Apple HealthKit and Google Health Connect, and looks food up against OpenFoodFacts, USDA, FatSecret and Nutritionix. Because it is self-hosted, there is no vendor API host, no pricing and no signup: you run the containers and the API is yours. Distributed under a source-available non-commercial licence.

**Areas:** Company · Health · Fitness · Nutrition · Self-Hosted · Open Source · Wearables · MCP · AI Assistant · Quantified Self

## SparkyFitness on APIs.io

Everything profiled for SparkyFitness is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/sparkyfitness/](https://apis.io/providers/sparkyfitness/)** — or go straight to one of the APIs or properties below.

### APIs (22)

- [SparkyFitness MCP Server](https://apis.io/apis/sparkyfitness/sparkyfitness-mcp-server/) — A first-party Model Context Protocol server running in-process inside the SparkyFitness API server, mounted at POST /mcp over a stateless streamable HTTP…
- [SparkyFitness Admin API](https://apis.io/apis/sparkyfitness/sparkyfitness-admin-api/) — The Admin API from SparkyFitness — 3 operation(s) for admin.
- [SparkyFitness AI API](https://apis.io/apis/sparkyfitness/sparkyfitness-ai-api/) — The AI API from SparkyFitness — 1 operation(s) for ai.
- [SparkyFitness AI & Insights API](https://apis.io/apis/sparkyfitness/sparkyfitness-ai-insights-api/) — AI-powered chat assistance, reports, trends, and analytical insights.
- [SparkyFitness Allergen Preferences API](https://apis.io/apis/sparkyfitness/sparkyfitness-allergen-preferences-api/) — The Allergen Preferences API from SparkyFitness — 2 operation(s) for allergen preferences.
- [SparkyFitness Authentication API](https://apis.io/apis/sparkyfitness/sparkyfitness-authentication-api/) — The Authentication API from SparkyFitness — 4 operation(s) for authentication.
- [SparkyFitness Dashboard API](https://apis.io/apis/sparkyfitness/sparkyfitness-dashboard-api/) — The Dashboard API from SparkyFitness — 3 operation(s) for dashboard.
- [SparkyFitness Dashboard Layouts API](https://apis.io/apis/sparkyfitness/sparkyfitness-dashboard-layouts-api/) — The Dashboard Layouts API from SparkyFitness — 1 operation(s) for dashboard layouts.
- [SparkyFitness Exercise Entries API](https://apis.io/apis/sparkyfitness/sparkyfitness-exercise-entries-api/) — The Exercise Entries API from SparkyFitness — 1 operation(s) for exercise entries.
- [SparkyFitness Exercise Stats API](https://apis.io/apis/sparkyfitness/sparkyfitness-exercise-stats-api/) — The Exercise Stats API from SparkyFitness — 4 operation(s) for exercise stats.
- [SparkyFitness Exercise & Workouts API](https://apis.io/apis/sparkyfitness/sparkyfitness-exercise-workouts-api/) — The Exercise & Workouts API from SparkyFitness — 23 operation(s) for exercise & workouts.
- [SparkyFitness External Integrations API](https://apis.io/apis/sparkyfitness/sparkyfitness-external-integrations-api/) — Third-party service connections (Garmin, Withings, OIDC, etc.).
- [SparkyFitness Fitness & Workouts API](https://apis.io/apis/sparkyfitness/sparkyfitness-fitness-workouts-api/) — Exercise database, workout presets, plan templates, and activity logging.
- [SparkyFitness Goals & Personalization API](https://apis.io/apis/sparkyfitness/sparkyfitness-goals-personalization-api/) — Personal goal setting, goal presets, and application preferences.
- [SparkyFitness Identity & Security API](https://apis.io/apis/sparkyfitness/sparkyfitness-identity-security-api/) — User authentication, registration, profile management, MFA, and access control.
- [SparkyFitness Medications & GLP-1 API](https://apis.io/apis/sparkyfitness/sparkyfitness-medications-glp-1-api/) — Medication cabinet, schedules, GLP-1 injections, pen/vial inventory, titration, and modeled PK/site-rotation.
- [SparkyFitness Nutrition & Meals API](https://apis.io/apis/sparkyfitness/sparkyfitness-nutrition-meals-api/) — Food database, diary logging, meal planning, and nutritional preferences.
- [SparkyFitness Sleep Science API](https://apis.io/apis/sparkyfitness/sparkyfitness-sleepscience-api/) — The SleepScience API from SparkyFitness — 7 operation(s) for sleepscience.
- [SparkyFitness Synced Data API](https://apis.io/apis/sparkyfitness/sparkyfitness-synced-data-api/) — The Synced Data API from SparkyFitness — 2 operation(s) for synced data.
- [SparkyFitness System & Admin API](https://apis.io/apis/sparkyfitness/sparkyfitness-system-admin-api/) — System configuration, administrative tasks, backups, reviews, and versioning.
- [SparkyFitness Utility API](https://apis.io/apis/sparkyfitness/sparkyfitness-utility-api/) — The Utility API from SparkyFitness — 1 operation(s) for utility.
- [SparkyFitness Wellness & Metrics API](https://apis.io/apis/sparkyfitness/sparkyfitness-wellness-metrics-api/) — Health metrics tracking (weight, measurements, sleep, mood) and fasting.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [sparkyfitness-mcp-server](https://apis.io/servers/sparkyfitness/sparkyfitness-mcp-server/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Sparkyfitness Openapi Overlay](https://apis.io/overlays/sparkyfitness/sparkyfitness-openapi-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Sparkyfitness Authentication](https://apis.io/security/sparkyfitness/sparkyfitness-authentication/)
- [Sparkyfitness Domain Security](https://apis.io/security/sparkyfitness/sparkyfitness-domain-security/)
- [Sparkyfitness Vulnerability Disclosure](https://apis.io/security/sparkyfitness/sparkyfitness-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Sparkyfitness Plans Pricing](https://apis.io/plans/sparkyfitness/sparkyfitness-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Sparkyfitness Rate Limits](https://apis.io/rate-limits/sparkyfitness/sparkyfitness-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://codewithcj.github.io/SparkyFitness/)
- [Documentation](https://codewithcj.github.io/SparkyFitness/)
- [Getting started](https://codewithcj.github.io/SparkyFitness/install/docker-compose)
- [API reference](https://codewithcj.github.io/SparkyFitness/developer/api-reference)
- [Support](https://codewithcj.github.io/SparkyFitness/help-me)
- [Changelog](https://github.com/CodeWithCJ/SparkyFitness/releases)
- [Privacy policy](https://codewithcj.github.io/SparkyFitness/privacy_policy)
- [GitHub](https://github.com/CodeWithCJ)

## API Evangelist rating

**Kin Score 32.9** — thin · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 68.3 |
| Contract quality | 15.1 |
| Contract governance | 4.5 |
| Operational transparency | 39.5 |
| Developer ergonomics | 56.5 |
| Access clarity | 10.5 |

**Agent readiness 29.2** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | templated |
| Auth Clarity | bearer (partial credit) |
| Idempotency | derived (partial credit) |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
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
| Pagination Ratio | — |
| Operation Distinctiveness | mostly (partial credit) |

**Regulatory alignment 16.4** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/sparkyfitness/](https://apis.io/providers/sparkyfitness/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for SparkyFitness: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/sparkyfitness/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

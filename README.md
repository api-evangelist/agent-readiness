# Agent Readiness (agent-readiness)

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

A topic index covering the operational practices, signals, and patterns that make an API surface safely usable by autonomous AI agents rather than only by humans. The repo catalogs the specifications, identity layers, agent skill formats, and edge-layer signals that combine into a coherent agent-readiness posture, and provides a JSON Schema, JSON-LD context, vocabulary, and example signal records that anyone can use to score their own or a third-party API.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/agent-readiness/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=topic-agent-readiness&utm_content=repo)

## Tags

- Agent Readiness, AI Agents, API Discovery, API Governance, Machine-Readable APIs, MCP, OpenAPI, AsyncAPI

## Timestamps

- **Created:** 2026-05-22
- **Modified:** 2026-05-22

## What "agent readiness" means here

Agent readiness is the degree to which an API surface can be safely driven by an autonomous AI agent — not just read by a human developer.

A human developer can paper over a lot of API friction: ambiguous error messages, inconsistent rate-limit behaviour, undocumented idempotency, missing examples, prose-only auth descriptions, HTML-only changelogs. An agent cannot. Every implicit convention the human silently absorbs is a place the agent gets stuck, retries blindly, double-charges a card, or hallucinates a payload.

Agent readiness is the discipline of removing those implicit conventions and replacing them with machine-readable signals. It is mostly not new work — it is taking the things good API teams already do (OpenAPI, idempotency keys, rate-limit headers, status pages) and making them discoverable and consistent enough that a non-human consumer can rely on them.

## The dimensions of agent readiness

This index uses a nine-dimension model. Each dimension is a concrete, observable signal that an auditor can score from a single evidence URL.

| # | Dimension | What it asks | Why it matters for agents |
|---|-----------|--------------|---------------------------|
| 1 | **Spec presence** | Does the provider publish a current, public OpenAPI (or AsyncAPI for events)? | Agents call APIs from contracts, not HTML docs. No spec = no programmatic surface. |
| 2 | **Auth model clarity** | Is the auth model documented in machine-readable form (OIDC discovery, OpenAPI security schemes)? | Agents must negotiate auth without reading prose. |
| 3 | **Idempotency** | Do mutating operations accept an idempotency key? | Agents retry. Without idempotency, retries cause duplicates — payments, messages, tickets. |
| 4 | **Error semantics** | Are errors a stable, documented envelope with stable codes? | Agents branch on errors. Free-text errors break branching. |
| 5 | **Rate-limit headers** | Does the API surface rate-limit state in response headers? | Agents need to know when to back off; "you'll get a 429 eventually" is not enough. |
| 6 | **Dry-run / simulate mode** | Do destructive operations expose a dry-run? | Agents should be able to plan before they execute. |
| 7 | **OpenAPI examples** | Does the OpenAPI include meaningful request/response examples? | Examples are the cheapest way an agent learns a payload shape. |
| 8 | **MCP server** | Does the provider ship an MCP server (first-party or first-class community)? | MCP is the most direct expression of "this API was thought about agent-first". |
| 9 | **AsyncAPI / events** | Are event/webhook surfaces described by a contract? | Agents that need to react to state changes need a typed event surface, not example payloads. |

Three additional, more forward-looking dimensions are tracked in the vocabulary and schema:

- **`/.well-known/api-catalog`** (RFC 9727) — the canonical machine entrypoint.
- **Consent signals** (AIPREF / Cloudflare Content-Signals) — explicit machine-readable AI usage preferences.
- **Web Bot Auth** (RFC 9421) — cryptographically identified agent traffic.

## Scoring

Each signal is scored 0–3:

- **0 — absent.** No public evidence of the dimension.
- **1 — partial.** Implicit, undocumented, or inconsistent across the surface.
- **2 — present.** Documented and reachable from the developer portal.
- **3 — exemplary.** Documented, machine-discoverable, and consistent across the entire API surface.

A provider's overall score is the mean of their dimension scores. The model is deliberately blunt — the point is comparability, not certification.

## Standards catalogued in `apis.yml`

The `apis.yml` indexes 15 standards, providers, and tools that contribute to agent readiness across the dimensions above:

- **Specifications:** OpenAPI, AsyncAPI, JSON Schema, APIs.json
- **Agent tooling:** Model Context Protocol (MCP), Agent Skills
- **Discovery:** `/.well-known/api-catalog` (RFC 9727)
- **Identity and consent:** OpenID Connect, HTTP Message Signatures (RFC 9421), Web Bot Auth draft, IETF AIPREF, Cloudflare Content Signals
- **Reference providers:** Stripe, GitHub, Twilio — used as worked examples of what an exemplary agent-readiness posture looks like across the model

## APIs

### Model Context Protocol (MCP)

An open JSON-RPC protocol that lets AI agents talk to tools, resources, and prompts through a uniform server surface. The most direct expression of "an API designed for an agent".

**Human URL:** [https://modelcontextprotocol.io](https://modelcontextprotocol.io)

### OpenAPI Specification

The dominant machine-readable contract for HTTP APIs. The single highest-leverage agent-readiness signal for a REST surface.

**Human URL:** [https://www.openapis.org](https://www.openapis.org)

### AsyncAPI Specification

Machine-readable contract for event-driven APIs (webhooks, message brokers, streaming).

**Human URL:** [https://www.asyncapi.com](https://www.asyncapi.com)

### JSON Schema

The vocabulary that lets an agent validate request and response bodies against typed contracts.

**Human URL:** [https://json-schema.org](https://json-schema.org)

### Agent Skills

A community schema for publishing operational instructions an agent should follow when using a site or API.

**Human URL:** [https://agentskills.io](https://agentskills.io)

### /.well-known/api-catalog (RFC 9727)

The canonical machine entrypoint for discovering an organization's APIs, formatted as an RFC 9264 linkset.

**Human URL:** [https://www.rfc-editor.org/rfc/rfc9727.html](https://www.rfc-editor.org/rfc/rfc9727.html)

#### Observed in the wild (2026-05-22 survey of 74 candidate providers)

Four providers serve a real `application/linkset+json` LinkSet at `.well-known/api-catalog`:

| Provider | Endpoint | Entries | Notable |
|---|---|---|---|
| **Cloudflare** | `developers.cloudflare.com/.well-known/api-catalog` | 1 | Properly typed with `profile="rfc9727"`. Points at the bundled `openapi.json`. |
| **Memesio** | `memesio.com/.well-known/api-catalog` | 2 | Only catalog observed that publishes **both** the REST API and the MCP server endpoint. The template other providers should copy. |
| **Merge.dev** | `docs.merge.dev/.well-known/api-catalog` | 10 | Deepest catalog — one entry per Unified API category (HRIS, ATS, Accounting, CRM, Ticketing, File Storage, etc.). |
| **Zuplo** | `zuplo.com/.well-known/api-catalog` | 1 | Only catalog explicitly declaring `application/vnd.oai.openapi+json;version=3.1` on its typed link. |

Signal records: see [`examples/agent-readiness-signal-cloudflare-well-known-catalog-example.json`](examples/agent-readiness-signal-cloudflare-well-known-catalog-example.json), [`memesio`](examples/agent-readiness-signal-memesio-well-known-catalog-example.json), [`merge`](examples/agent-readiness-signal-merge-well-known-catalog-example.json), [`zuplo`](examples/agent-readiness-signal-zuplo-well-known-catalog-example.json).

The 70 other providers probed (the recent API Evangelist pipeline batch plus the obvious candidates — Stripe, Twilio, GitHub, Salesforce, Microsoft Graph, Google, Postman, Plaid, SendGrid, Slack, Shopify, Intercom, Segment, Hookdeck, every API gateway, every iPaaS, every identity provider, every docs platform, every SDK generator, every observability vendor, every LLM provider) either returned 404 / no route, or returned 200 with HTML — a SPA catch-all fallback, not a real LinkSet. The signal is essentially uncatalogued in 2026.

### HTTP Message Signatures (RFC 9421)

A cryptographic signature scheme for HTTP messages used by the emerging web-bot-auth profile.

**Human URL:** [https://www.rfc-editor.org/rfc/rfc9421.html](https://www.rfc-editor.org/rfc/rfc9421.html)

### Web Bot Auth (draft)

IETF draft layering an "identified bot" profile on top of RFC 9421.

**Human URL:** [https://datatracker.ietf.org/doc/draft-meunier-web-bot-auth-architecture/](https://datatracker.ietf.org/doc/draft-meunier-web-bot-auth-architecture/)

### Content-Usage / AIPREF (IETF AIPREF WG)

The IETF AIPREF working group's effort to standardise machine-readable AI usage preferences.

**Human URL:** [https://datatracker.ietf.org/wg/aipref/about/](https://datatracker.ietf.org/wg/aipref/about/)

### Cloudflare Content Signals

The `Content-Signal` robots.txt directive, complementing the AIPREF drafts.

**Human URL:** [https://blog.cloudflare.com/content-signals-policy/](https://blog.cloudflare.com/content-signals-policy/)

### APIs.json

The APIs.json format describes a provider's API portfolio in one machine-readable document.

**Human URL:** [https://apisjson.org](https://apisjson.org)

### OpenID Connect

Identity layer on top of OAuth 2.0; agent-readiness depends on discoverable OIDC metadata.

**Human URL:** [https://openid.net/connect/](https://openid.net/connect/)

### Stripe API (reference provider)

Reference provider: full public OpenAPI, idempotency keys, rate-limit guidance, consistent error envelope, status page, changelog.

**Human URL:** [https://docs.stripe.com/api](https://docs.stripe.com/api)

### GitHub REST + GraphQL API (reference provider)

Reference provider: public OpenAPI, GraphQL schema, webhooks, conditional requests, explicit `X-RateLimit-*` headers, status page, first-party MCP server.

**Human URL:** [https://docs.github.com/en/rest](https://docs.github.com/en/rest)

### Twilio API (reference provider)

Reference provider: published OpenAPI repo used to generate every official SDK, idempotency on resource creation, signed webhooks.

**Human URL:** [https://www.twilio.com/docs](https://www.twilio.com/docs)

## Common Properties

- [GitHubOrganization](https://github.com/api-evangelist)
- [GitHubRepository](https://github.com/api-evangelist/agent-readiness)
- [Documentation](https://github.com/api-evangelist/agent-readiness/blob/main/README.md)

## Features

| Name | Description |
|------|-------------|
| Dimension Model | A nine-dimension scoring model covering specs, auth, idempotency, error semantics, rate-limit headers, dry-run, examples, MCP, and event contracts |
| Signal Schema | JSON Schema describing a single agent-readiness signal (provider, dimension, score, evidence URL) |
| Provider Aggregate | JSON Schema for a provider-level aggregate of signals across dimensions |
| JSON-LD Context | A JSON-LD context aligning agent-readiness signals with schema.org and common API vocabularies |
| Reference Examples | Example signal records for Stripe, Twilio, and GitHub illustrating how the model is applied in practice |
| Vocabulary | Controlled vocabulary of agent-readiness terms across operational and capability dimensions |

## Use Cases

| Name | Description |
|------|-------------|
| Provider Self-Assessment | A team auditing their own API surface against a checklist of agent-readiness dimensions |
| Consumer Pre-Integration Review | An engineering team evaluating whether a third-party API can be driven by an autonomous agent before committing to integration |
| Procurement and RFP Scoring | A buyer scoring vendor APIs on a normalized agent-readiness rubric during procurement |
| Aggregator Indexes | A directory or marketplace ranking listed APIs by agent-readiness score to help agent developers pick safe surfaces |
| Standards Coverage Map | Mapping where each standard (OpenAPI, AsyncAPI, MCP, AIPREF, RFC 9727, RFC 9421) contributes to which dimension |

## Artifacts

Machine-readable artifacts organized by format.

### JSON Schema

- [Agent Readiness Signal Schema](json-schema/agent-readiness-signal-schema.json) — a single observed signal
- [Agent Readiness Provider Schema](json-schema/agent-readiness-provider-schema.json) — provider-level aggregate

### JSON-LD

- [Agent Readiness Context](json-ld/agent-readiness-context.jsonld) — JSON-LD context aligning signal terms with schema.org

### Examples

- [Stripe — spec-presence signal](examples/agent-readiness-signal-stripe-example.json)
- [Stripe — idempotency signal](examples/agent-readiness-signal-stripe-idempotency-example.json)
- [Stripe — provider aggregate](examples/agent-readiness-provider-stripe-example.json)
- [GitHub — rate-limit-headers signal](examples/agent-readiness-signal-github-example.json)
- [GitHub — MCP server signal](examples/agent-readiness-signal-github-mcp-example.json)
- [Twilio — spec-presence signal](examples/agent-readiness-signal-twilio-example.json)

## Vocabulary

- [Agent Readiness Vocabulary](vocabulary/agent-readiness-vocabulary.yaml) — controlled vocabulary covering 12 dimensions, 4 score labels, 5 personas, and 5 domains

## Notable absences in the topic

A few patterns deliberately omitted from the index:

- **No `llms.txt`.** The `/.well-known/api-catalog` + markdown content negotiation cover the same use case using existing RFCs; adding a third overlapping discovery file dilutes the signal.
- **No bespoke "agent API" alongside a human site.** The position taken across this topic is one URL with two representations (content negotiation), not two parallel surfaces.
- **No certification scheme.** The dimensions are deliberately scored 0–3 by an observer with an evidence URL — they are not a pass/fail audit, and there is no governing body issuing a badge.

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com

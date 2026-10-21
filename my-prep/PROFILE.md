# My Profile

## One-line summary

Principal software engineer with about 15 years' experience who designs the systems that connect old and new: contract-first integrations, large live data migrations, and the order, subscription and invoicing platform of a K–12 education business. I lead a small squad and own delivery end to end.

## Role arc (2022 onward, from my own records)

| Period | Role | Scope |
|---|---|---|
| 2022 → 2023 | SDE 2 Lead | Resourcing and delivery across several learning products: a new language launch with offline distribution, a teacher dashboard, automated student transfer and class upgrade, credential revocation in a sync mechanism, a v2 CRM API going live with Swagger docs |
| 2024 → 2025 | Squad lead, internal systems | Led a squad of four engineers on CRM integrations and internal systems |
| 2025 → now | **Principal SDE**, commercial platform squad | Product clarity for two national rollout tracks (phased rollout, dependency mapping, migration scope). In my objectives document's words: *"Led service boundary decisions, contract-first integrations, and migration design patterns (ID mapping, retries, monitoring, rollback)."* |
| Before 2022 | **[needs input]** | About 10 years not covered by my records. See [INPUTS-NEEDED.md](INPUTS-NEEDED.md) |

## What I'm strongest at (with evidence grade)

| Strength | Evidence | Grade |
|---|---|---|
| **Large live data migrations** | Designed a two-way bridge between a long-lived legacy MySQL estate and new Postgres services across six product lines. Separated a one-time historical backfill from a continuous reverse sync so legacy products kept working. Seven specifications | [confirmed] |
| **Integrations with business systems** | CRM and accounting integrations (leads into CRM, invoices created from our platform, CRM → Postgres mapping and data porting), v2 API with Swagger docs | [confirmed] |
| **Commerce platform design** | Order and subscription data model, product/SKU catalogue API, a discount rules engine with a four-tier approval ladder from self-approval to C-suite, finance-auditable billing-period calculations | [confirmed] |
| **API and platform design** | About a dozen services behind an API gateway (JWT auth, rate limiting), an aggregation (BFF) layer, namespaced error codes, deliberate contract decisions | [confirmed] |
| **Access control** | Row-level scoping: leadership sees everything, others see records where they're the assigned salesperson *or* consultant. Enforced on list, KPI counts and write paths | [confirmed] |
| **Linking engineering to business metrics** | Invoicing and payment work tied to reducing days-sales-outstanding (cash collection) | [confirmed] design. Outcome **[needs input]** |
| **Delivery leadership** | Sprint records, build and release plans, monthly programme reports for seven consecutive months, written 30/90-day onboarding plans, tech lead for three business-critical modules | [confirmed] |
| **Mentoring and team building** | Grew a team, placed new graduates deliberately, coached on prioritisation, set code review and QA standards | [confirmed] |
| **Legacy modernisation** | Migrated all users of a product from an old PHP 5.3 stack to PHP 7.3, and automated content porting | [confirmed] |
| **AI decision-making** | Contributed to a speech-to-text provider evaluation: four delivery models, break-even analysis, "buy, don't build behind a thin adapter", data-protection (India DPDP Act) as a hard constraint | **[partial]** — say "contributed to" |

## Gaps to close for the roles I'm targeting

| Gap | Why it matters | Plan |
|---|---|---|
| Hands-on production LLM work (RAG, agents, evals) | Core of FDE / Applied AI loops at AI companies | Build two portfolio projects (see [topics/06-ai-llm](../topics/06-ai-llm/README.md) section J) |
| Recent DS&A practice | Coding screens at Palantir, Scale, Databricks and AI labs | 6 weeks of [topics/02-coding](../topics/02-coding/README.md), mediums only, timed |
| Public code | My commit history isn't visible outside my employer | Publish the portfolio projects on GitHub |
| External customer exposure | My stakeholders have mostly been internal (sales, finance, academic teams) | Frame internal stakeholders as customers. Gather any external vendor, partner or client stories in [INPUTS-NEEDED.md](INPUTS-NEEDED.md) |
| Measured outcomes | Strong on design, light on hard result numbers | Collect publishable numbers (percentages, before/after) in [INPUTS-NEEDED.md](INPUTS-NEEDED.md) |
| Live demos and presentations | Solutions and FDE loops include demo rounds | Record two 10-minute demos of the portfolio projects |

## Which roles fit me best

Ranked by how well my record matches the role today, and how much prep each needs:

| Rank | Role | Fit | Why | Main prep |
|---|---|---|---|---|
| 1 | [Solutions Architect](../roles/04-solutions-architect/README.md) | Strong | Integrations, migrations, API and platform design, presenting trade-offs | Cloud reference architectures, Well-Architected, a certification |
| 2 | [Implementation / Integration Engineer](../roles/08-implementation-integration-engineer/README.md) (senior or lead) | Strong | Data migration and CRM/ERP integrations are exactly my work | Customer-facing framing, statements of work and go-live stories |
| 3 | [Forward Deployed Engineer](../roles/01-forward-deployed-engineer/README.md) (senior) | Good, with gaps | Owning messy real-world integrations end to end, ambiguity, delivery | LLM projects, DS&A, decomposition practice |
| 4 | [Staff / Principal FDE, Field CTO](../roles/12-principal-fde-field-cto/README.md) | Good at level, gap in external exposure | Principal level, technical leadership, architecture decisions | External customer stories, executive presentations |
| 5 | [Technical Delivery Manager / TPM](../roles/15-technical-delivery-manager/README.md) | Good | Release plans, programme reports, multi-team migrations | Programme governance vocabulary (RAID, steering committees) |
| 6 | [Partner Solutions Engineer](../roles/16-partner-solutions-engineer/README.md) | Good | Third-party integrations and connectors | Partner and co-selling concepts |
| 7 | [Applied AI Engineer](../roles/05-applied-ai-engineer/README.md) / [Agent Engineer](../roles/06-agent-engineer/README.md) | Stretch | Strong engineering base, but limited hands-on LLM work | The LLM projects are essential first |

**Suggested strategy:** apply to ranks 1–3 now while building the LLM portfolio projects. Move into AI-company FDE and Applied AI loops once the projects are public.

## My positioning statement (for recruiter screens and LinkedIn)

> I'm a principal engineer who makes new systems work inside old businesses. I've spent the last few years designing the bridge between a long-lived legacy estate and a new service platform while both stayed live, integrating CRM and accounting systems, and building the order and billing platform that sales and finance depend on. I want to do that work directly with customers, as a forward deployed or solutions engineer, especially on AI deployments where the hard part is integrating with the real business.

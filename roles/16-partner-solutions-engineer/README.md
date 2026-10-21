# Partner Solutions Engineer

The technical owner of a company's relationships with partners (systems integrators, ISVs, cloud providers and technology partners): building and certifying integrations, enabling partner engineers, listing on cloud marketplaces, designing joint reference architectures and supporting co-sell deals.

---

## The role

**Day to day.** Partner engineers sit between the company's own product, sales and partnerships teams and the engineering teams of its partners. A typical week includes:

- Technical calls with partner engineers: integration design reviews, office hours, troubleshooting a joint customer issue.
- Building or reviewing integrations and connectors (for example a connector to a data platform, a CRM or a cloud provider's identity or storage services), plus sample code and SDK examples.
- Partner enablement: workshops, labs, certification exams and documentation that let SI and ISV engineers deliver without you.
- Cloud marketplace work: the technical side of listing the product (subscription and entitlement handling, usage metering), and supporting private offers.
- Co-sell: joining calls with a partner's sales team and your own account team, answering technical questions, running a joint proof of concept.
- Joint reference architectures, solution briefs, blog posts and conference demos with partners.
- Bringing partner and integration gaps back to product and engineering, and tracking them.

**Titles and aliases.** Partner Solutions Engineer, Partner Engineer, Partner Solutions Architect (PSA), Partner Sales Engineer, Alliances Engineer / Technical Alliance Manager, Integrations Engineer / Integration Partner Engineer, Ecosystem Engineer, Technology Partner Engineer, ISV Solutions Architect. At cloud providers the role is often a Partner Solutions Architect aligned to a set of SI or ISV partners. Some titles (Partner Manager, Alliance Manager) are mostly commercial; read the job description to see how much engineering is expected.

**Who hires (examples, titles vary).** Cloud providers with large partner programs (AWS, Microsoft, Google Cloud); data and infrastructure vendors with big partner ecosystems (Snowflake, Databricks, MongoDB, Confluent, Elastic, Datadog, HashiCorp); SaaS platforms with app marketplaces (Salesforce, HubSpot, Atlassian, Shopify, Stripe, Twilio, Zoom); and AI companies that sell through cloud marketplaces and integrate with partner platforms. Systems integrators also hire "alliance" engineers who own their side of a vendor relationship.

**How it differs from an FDE.**

| | Partner Solutions Engineer | Forward Deployed Engineer |
|---|---|---|
| Main "customer" | Partners (SIs, ISVs, cloud providers), and joint customers through them | End customers directly |
| Main output | Integrations, connectors, reference architectures, enablement content, certified partners | Production systems at one customer |
| Leverage | One-to-many: scale through partners | One-to-one or one-to-few |
| Coding | Med: connectors, sample apps, marketplace integration, demos | High: production code daily |
| Commercial link | Partner-sourced and partner-influenced revenue, marketplace transactions, co-sell wins | Deployment outcome, expansion |
| Time horizon | Long relationships (years) with each partner | Weeks to months per deployment |

**Who it suits.** Engineers who like integration work and APIs, enjoy teaching and writing, are comfortable influencing people they do not manage, and want a role that mixes engineering with business strategy. Good for people who like breadth across many companies' products.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding (connectors, sample apps, scripts) | Med |
| API and integration design (auth, webhooks, versioning, rate limits) | High |
| Cloud platform knowledge (one provider deeply) | Med to High |
| Teaching, enablement and technical writing | High |
| Partner and stakeholder management (influence without authority) | High |
| Sales / commercial awareness (co-sell, marketplace, partner programs) | Med |
| Product influence (integration roadmap) | Med |

**Career paths.** Senior or Principal Partner SA; partner engineering manager; head of alliances or ecosystem; product manager for integrations, platform or marketplace; developer relations; solutions architect or field CTO; moving to the partner side (SI practice lead).

---

## Typical interview loop

Loops vary by company and level. The rounds below are typical or reported, not guaranteed.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Partner or integration experience, why partners rather than direct customers, travel | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Hiring manager | Partner programs you have worked with, how you scale through others, influence without authority | This file + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |
| Technical: integration and APIs | OAuth, webhooks, versioning, rate limits, idempotency, connector design | This file + [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) |
| Architecture whiteboard | Joint reference architecture with a cloud provider or partner product; security and data flow | This file + [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Practical coding or take-home (common for integration-heavy roles) | Build a small connector or sync job with pagination, retries and idempotency | This file + [02-coding](../../topics/02-coding/ANSWERS.md) |
| Enablement or presentation round | Teach a partner's engineers a product or architecture in 20–30 minutes; handle questions | This file (Partner enablement) + [05-learning-round](../../topics/05-learning-round/ANSWERS.md) |
| Partner role-play | Channel conflict, a failing SI, a roadmap request, a joint customer escalation | This file + [09-customer-scenarios](../../topics/09-customer-scenarios/ANSWERS.md) |
| Behavioral | Influence, collaboration across companies, saying no, scaling yourself | [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |

---

## Common rounds — use the shared sections

- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): highest priority. APIs and integration, authentication and authorization, cloud basics.
- [03 System design](../../topics/03-system-design/ANSWERS.md): the method for any reference architecture or connector framework whiteboard.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): difficult situations, communication and business acumen apply directly to partners.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): especially conflict and collaboration, and communication.
- [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md): triaging integration failures across two companies' systems.
- [02 Coding](../../topics/02-coding/ANSWERS.md): practical coding against APIs for take-homes.
- [05 Learning round](../../topics/05-learning-round/ANSWERS.md): the teaching skills behind an enablement round.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md): for AI companies whose partners build on their models.
- Related roles: [04 Solutions Architect](../04-solutions-architect/README.md), [08 Implementation / Integration Engineer](../08-implementation-integration-engineer/README.md), [10 Developer Relations](../10-developer-relations/README.md).

---

## Role-specific questions

### Role and partner ecosystem
- [ ] How is a partner solutions engineer different from a solutions engineer and a forward deployed engineer?
- [ ] Explain the difference between SIs, ISVs, resellers, cloud marketplaces and technology partners, and how you would work with each.
- [ ] How do you decide which partners to invest engineering time in?
- [ ] How do you measure the success of a partner engineering program?

### Building integrations and connectors
- [ ] Design a connector framework that lets partners build integrations to our platform.
- [ ] Walk through building an integration between our product and Salesforce. What are the hard parts?
- [ ] How do you handle authentication for partner integrations (OAuth 2.0, API keys, service accounts)?
- [ ] How do you handle API versioning and breaking changes when hundreds of partner integrations depend on your API?
- [ ] Design a webhook system that partners can rely on.
- [ ] A partner's integration is causing excessive load and rate-limit errors on your API. What do you do?
- [ ] Take-home: build a minimal connector that syncs records between two APIs with pagination, retries and idempotency. How do you approach it?

### Cloud marketplaces
- [ ] How do cloud marketplaces work, and why do customers buy through them?
- [ ] Walk through the technical work to list a SaaS product on AWS Marketplace.
- [ ] What is a private offer, and how does marketplace purchasing interact with a customer's committed cloud spend?
- [ ] How do you meter usage-based billing for a marketplace listing, and what goes wrong?

### Partner enablement and certification
- [ ] Design a partner enablement program for a new product launch.
- [ ] How would you design a technical certification program for SI partners?
- [ ] An SI partner delivered a poor implementation for a joint customer. How do you handle it?
- [ ] How do you write documentation and sample code that partners can build on without your help?
- [ ] Enablement exercise: teach a partner's engineers our product in 20 minutes. How do you structure it?

### Co-selling and alliances
- [ ] What is co-selling, and what is the partner engineer's role in a co-sell deal?
- [ ] How do you handle channel conflict between a partner and your direct sales team?
- [ ] A strategic partner asks for a roadmap commitment to win a deal. How do you respond?
- [ ] What goes into the technical part of a joint business plan with a partner?
- [ ] How do you work with a cloud provider's partner team to get co-sell support?

### Joint reference architectures and ecosystem design
- [ ] Whiteboard: design a joint reference architecture for our product running on a cloud provider for a regulated customer.
- [ ] How do you validate and maintain a joint reference architecture so it does not go stale?
- [ ] How do you scope and build a joint solution or accelerator with an SI?
- [ ] Design the review and certification process for third-party integrations listed in our app directory.

### Joint customer issues and feedback
- [ ] A joint customer's integration broke after the partner updated their product. Walk through how you triage it.
- [ ] How do you handle security and data-sharing reviews when integrating with a partner?
- [ ] How do you bring partner feedback to your product team and get integration features prioritised?

### Behavioral (role-specific)
- [ ] Tell me about a time you influenced an external team you had no authority over.
- [ ] Tell me about a time you built something that others reused at scale.
- [ ] Tell me about a time you balanced the interests of two parties who wanted different things.
- [ ] Tell me about a time you said no to a partner or customer.

---

## Model answers

### Role and partner ecosystem

#### How is a partner solutions engineer different from a solutions engineer and a forward deployed engineer?

- **Solutions engineer (SE):** pre-sales, paired with an account executive, wins the technical decision for one customer deal through discovery, demos and proofs of concept.
- **Forward deployed engineer (FDE):** builds production software with or for one customer to deliver an outcome.
- **Partner solutions engineer (PSE):** works through other companies. The "customers" are partners: systems integrators who implement the product, ISVs who integrate with it, and cloud providers who resell or co-sell it. The job is leverage: one good connector, reference architecture or certified SI partner can reach hundreds of customers you will never meet.

The day-to-day difference is that a PSE builds integrations and enablement assets rather than one-off demos, measures success through partner-sourced and partner-influenced revenue, certified practitioners and integration adoption, and relationships last years rather than one sales cycle. A PSE still joins deals, but usually as the person who connects your account team, the partner's team and the joint customer.

One line: the SE wins a deal, the FDE delivers a deployment, the PSE builds the channels through which many deals and deployments happen.

#### Explain the difference between SIs, ISVs, resellers, cloud marketplaces and technology partners, and how you would work with each.

- **Systems integrators (SIs) and consultancies** (global firms such as Accenture or Deloitte, plus regional and boutique firms): implement your product for customers. Work with them on enablement, certification, delivery quality, reusable accelerators and joint pursuits. Their incentive is billable services, so show them a repeatable practice.
- **ISVs and technology partners:** other software vendors whose products integrate with yours (a data platform, an identity provider, a monitoring tool). Work on integration design, co-engineering, certification of the integration, joint reference architectures and joint marketing. The incentive is a better product for shared customers.
- **Resellers and distributors:** resell licences, sometimes with first-line support. Work on technical sales enablement and deal support; the engineering depth is lower.
- **Cloud marketplaces** (AWS Marketplace, Microsoft commercial marketplace, Google Cloud Marketplace): a procurement channel. Work on the technical listing (fulfilment and metering APIs), private offers and the provider's co-sell programs.
- **Cloud providers as partners:** they are also co-sell partners and sometimes competitors. Work on co-sell readiness, joint architectures and field relationships.

The key point in an interview: each partner type has a different incentive, and your programs should be built around their incentive, not yours.

#### How do you decide which partners to invest engineering time in?

Partner engineering time is scarce, so I treat it like a portfolio:

1. **Start from customer demand:** which partner products appear most often in our customers' stacks and in lost-deal notes? Which SIs show up in our biggest accounts?
2. **Score partners on fit and reach:** shared target customers, the partner's market reach, technical fit of the integration, their willingness to commit their own engineers and sales, and strategic value (for example a cloud provider's co-sell program).
3. **Tier them:** a small number of strategic partners get dedicated engineering, joint roadmaps and co-built integrations; a middle tier gets standard programs (certification, office hours); the long tail gets self-service (docs, SDKs, a partner portal).
4. **Require mutual investment:** a partner who will not assign engineers or sales sponsorship rarely produces results.
5. **Review quarterly** against outcomes: integration adoption, partner-sourced pipeline, certified practitioners, delivery quality. Move partners between tiers based on evidence.

I would also say no to partners who want effort without commitment, politely, and point them to self-service resources.

#### How do you measure the success of a partner engineering program?

I separate outcome metrics from activity metrics.

**Outcomes (what the business cares about):**
- Partner-sourced pipeline and revenue (deals the partner brought) and partner-influenced revenue (deals where a partner helped).
- Marketplace transaction volume and how much of it counted toward customers' cloud commitments.
- Adoption of each integration: active installs, API calls through the connector, retention of customers who use it versus those who do not.
- Delivery quality for SI-led implementations: time to go-live, customer satisfaction, escalations.

**Leading indicators:**
- Certified practitioners per partner and how many are actually staffed on projects.
- Time for a new partner to build and certify an integration.
- Support tickets per integration, and partner self-service rate (questions answered by docs rather than by me).

**Activity metrics** (workshops run, assets published) are useful only as inputs. I would agree two or three headline metrics with the partnerships leader, review them monthly per tier, and be honest that attribution is messy; partner influence is real even when it is hard to prove.

### Building integrations and connectors

#### Design a connector framework that lets partners build integrations to our platform.

**Clarify:** direction (data into us, out of us, or both), volume, who builds (our team, partners, or both), and where connectors run (our cloud or the customer's environment).

**Design:**
- **A stable connector contract:** a small interface each connector implements, such as `spec()` (config schema), `check()` (validate credentials), `discover()` (available objects and schemas), `read(state)` (incremental reads returning records plus a cursor) and `write(records)`. Open-source connector frameworks follow a similar pattern.
- **An SDK** that handles the shared hard parts: auth helpers (OAuth flows, token refresh), pagination, retries with backoff, rate-limit handling, state and checkpoints, logging and metrics.
- **A runtime** that schedules connectors, isolates them (one container per sync), stores secrets in a secrets manager, and records run history.
- **Testing and certification:** a test harness with recorded API responses, a conformance test suite every connector must pass, and a review before listing.
- **Versioning:** connectors are versioned and pinned per customer; upgrades are opt-in or staged.
- **Observability:** per-connector error rates, latency, rows synced and an alert path to the owning partner.

**Trade-offs:** a strict contract makes certification easy but limits unusual sources; allow an escape hatch, but certify only the standard path.

#### Walk through building an integration between our product and Salesforce. What are the hard parts?

**Setup:** create a Salesforce connected app (or the newer external client app), use OAuth 2.0 (authorisation code flow for user-installed integrations, or JWT bearer flow for server-to-server), request minimal scopes, and store refresh tokens securely.

**Data movement:**
- Use the REST API for small reads and writes, Bulk API 2.0 for large volumes, and Change Data Capture or Platform Events (or polling on `SystemModstamp`) for near-real-time changes.
- Map objects and fields, including custom objects and fields, which differ in every org.

**Hard parts:**
- **Customisation:** every customer's org has different fields, validation rules, triggers and required fields. Make mapping configurable and discover the schema with describe calls.
- **API limits:** orgs have a daily API request allocation shared with every other integration. Batch, use bulk APIs and back off.
- **Identity matching and duplicates:** decide match keys (external ID fields) and conflict rules for two-way sync.
- **Permissions:** the integration user's profile and field-level security silently hide fields.
- **Sandbox versus production,** deployment and testing across both.
- **Distribution:** if you package it for AppExchange, you go through Salesforce's security review.

I would state these up front so the customer knows what to expect.

#### How do you handle authentication for partner integrations (OAuth 2.0, API keys, service accounts)?

Choose by who is acting and on whose behalf:

- **OAuth 2.0 authorisation code flow (with PKCE for public clients):** when a partner app acts on behalf of a customer's users or account. The customer consents to specific scopes, tokens are revocable, and the partner never sees the customer's password. Best default for marketplace and third-party apps.
- **OAuth 2.0 client credentials:** server-to-server access where the partner acts as itself, not as a user.
- **API keys:** simple, fine for low-risk or internal use, but hard to scope and rotate, and easily leaked. If offered, support scoping, expiry, multiple active keys for rotation and secret scanning.
- **Cloud service identities:** for integrations between cloud resources, prefer workload identity federation or cross-account roles (for example an IAM role assumed with an external ID) over long-lived keys.
- **Mutual TLS or signed requests** for high-security partner links.

Across all of them: least-privilege scopes, short-lived access tokens with refresh, an audit log of which partner did what, a way for customers to see and revoke connected apps, and webhook signatures in the other direction.

#### How do you handle API versioning and breaking changes when hundreds of partner integrations depend on your API?

**Avoid breaking changes:** make additive changes (new optional fields, new endpoints), tell partners to ignore unknown fields, and never change the meaning of an existing field.

**When a break is unavoidable:**
- **Version explicitly:** a version in the URL or a date-based version header pinned per API key or app (Stripe's dated versions are a well-known example). Old versions keep working until a published sunset date.
- **Publish a deprecation policy:** for example at least 12 months' notice for breaking changes, with `Deprecation` and `Sunset` response headers and changelog entries.
- **Measure usage:** know which partners call the deprecated version or field, how often, and on behalf of how many customers.
- **Migrate actively:** migration guides with before-and-after code, SDK releases, direct outreach to the top partners by traffic, and office hours.
- **Brownouts:** before the final shutdown, briefly disable the old version at announced times so silent dependants find out.
- **Contract tests:** run partner-style contract tests in CI so accidental breaks are caught before release.

The PSE's role is the human part: tracking each important partner to completion, not just announcing.

#### Design a webhook system that partners can rely on.

**Delivery:**
- Events are written to a durable store (an outbox) in the same transaction as the state change, then delivered by workers from a queue.
- At-least-once delivery with retries using exponential backoff and jitter over a long window (for example up to a few days), then a dead-letter state and an alert to the partner.
- Each event has a unique ID so receivers can deduplicate, a type, a created timestamp and an API version.
- Ordering is not guaranteed; document it, and include enough data (or a resource version) for receivers to ignore stale events.

**Security:** sign payloads with a per-endpoint secret (HMAC with a timestamp to prevent replay), require HTTPS, support secret rotation with two valid secrets.

**Partner experience:** a dashboard showing delivery attempts and responses, manual replay, a test-event button, the ability to subscribe to specific event types, and an events API so partners can reconcile missed events.

**Protecting both sides:** per-endpoint concurrency limits, automatic disabling of endpoints that fail for days (with notice), and thin payloads with a fetch-by-ID option for sensitive data.

#### A partner's integration is causing excessive load and rate-limit errors on your API. What do you do?

**Immediately:** make sure other customers are protected. Confirm rate limiting works per partner app and per customer, and if needed apply a temporary tighter limit to that app, telling the partner at once.

**Diagnose with data:** which endpoints, what request pattern, which customers. Common causes: polling for changes every few seconds instead of using webhooks, fetching full lists without incremental filters, retry storms with no backoff, N+1 calls per record, or a bug after their release.

**Work with the partner:**
- Share the evidence (graphs, request IDs) without blame.
- Recommend fixes: webhooks or change feeds instead of polling, `updated_since` filters, bulk endpoints, exponential backoff with jitter that respects `Retry-After`, and caching.
- Agree a timeline; offer an engineer for a review session.

**Fix our side too:** clear rate-limit headers, documented limits, bulk and change-feed endpoints if they are missing, and better guidance in the docs and SDK (the SDK should back off by default).

**Prevent:** add per-partner usage dashboards and alerts, and include load behaviour in integration certification.

#### Take-home: build a minimal connector that syncs records between two APIs with pagination, retries and idempotency. How do you approach it?

**Plan (first 30 minutes):** read both APIs' docs, write down assumptions (direction, which object, match key, conflict rule), and define "done": a script or small service, tests, and a README.

**Build:**
- **Config and auth:** credentials from environment variables, never hard-coded.
- **Read with pagination:** follow cursors or `next` links until done; use an `updated_since` filter for incremental runs and save the high-water mark only after a successful batch.
- **Transform:** a pure mapping function with unit tests, including missing and malformed fields.
- **Write idempotently:** upsert by an external ID, or send an idempotency key derived from the source ID and version, so reruns create no duplicates.
- **Retries:** exponential backoff with jitter for 429 and 5xx, respecting `Retry-After`; no retries on 4xx validation errors, which go to an error report instead.
- **Observability:** structured logs with counts read, written, skipped and failed.

**Tests:** mock both APIs (recorded responses), test pagination end, retry behaviour and rerun idempotency.

**README:** how to run it, design decisions, what you would add with more time (webhooks, parallelism, dead-letter handling). Reviewers weight the README and tests heavily.

### Cloud marketplaces

#### How do cloud marketplaces work, and why do customers buy through them?

A cloud marketplace (AWS Marketplace, Microsoft commercial marketplace including Azure Marketplace, Google Cloud Marketplace) lets a customer buy third-party software and have it billed on their existing cloud invoice. The vendor lists the product, the cloud provider handles billing and collects payment, takes a listing fee (a percentage of the transaction), and pays the vendor.

**Why customers buy this way:**
- **Procurement is faster:** the cloud provider is already an approved vendor, so new supplier onboarding and some legal review can be skipped, often using standard contract terms.
- **Committed spend:** many enterprises have a committed spend agreement with their cloud provider, and eligible marketplace purchases can count toward it (program rules differ by provider and listing).
- **Consolidated billing and governance:** one invoice, central control of which software can be bought.

**Why vendors care:** shorter sales cycles, access to the cloud provider's sellers through co-sell programs, and budget that customers have already committed.

**Listing types** include SaaS (subscriptions, contracts or both), machine images, containers and professional services. For a PSE the main work is the SaaS integration, private offers, and making the listing eligible for the provider's co-sell programs.

#### Walk through the technical work to list a SaaS product on AWS Marketplace.

1. **Register as a seller** and create a SaaS product in the AWS Marketplace Management Portal, choosing the pricing model: subscription (pay-as-you-go), contract, or contract with consumption.
2. **Build a registration (fulfilment) URL:** after subscribing, the buyer is redirected to your landing page with a registration token. Your backend calls the AWS Marketplace Metering Service `ResolveCustomer` API to exchange it for a customer identifier and product code, then links it to a new or existing account in your product.
3. **Handle lifecycle events:** subscribe to the notifications AWS sends for subscription changes (subscribe success or failure, unsubscribe pending and success) and for contract entitlement updates, and act on them (enable, suspend, deprovision). AWS has been moving these notifications to Amazon EventBridge, so check the current docs.
4. **Check entitlements** for contracts with the AWS Marketplace Entitlement Service (`GetEntitlements`) to know what the customer has paid for.
5. **Report usage** for usage-based dimensions with `BatchMeterUsage`, typically hourly.
6. **Test** with a limited (private) listing, then submit for AWS review and publish.

Azure (SaaS fulfilment API and marketplace metering service) and Google Cloud (Partner Procurement API and usage reporting) follow the same shape: landing page, lifecycle events, entitlement, metering.

#### What is a private offer, and how does marketplace purchasing interact with a customer's committed cloud spend?

**Private offer:** a custom offer made to one specific buyer through the marketplace, with negotiated price, contract length, payment schedule and custom terms (often the customer's own EULA or a standard contract). It is how most enterprise marketplace deals happen: the deal is negotiated as normal, then transacted through the marketplace. On AWS, channel partner private offers let a reseller or SI create the offer with their own margin, so partners can stay in the deal.

**Committed spend:** large customers often sign a commitment to spend a set amount with their cloud provider over several years. When the program rules allow it (for example eligible listings in Microsoft's consumption commitment program, or marketplace purchases counting toward an AWS or Google Cloud commitment), buying your product through the marketplace draws down that commitment. This turns your software into budget the customer has already approved, which can remove a procurement obstacle.

**What the PSE does:** makes sure the listing is technically eligible, explains the flow to the account team and the customer's procurement, coordinates with the cloud provider's partner manager, and avoids promising eligibility without checking the current rules for that provider and listing.

#### How do you meter usage-based billing for a marketplace listing, and what goes wrong?

**Design:**
- Define billing dimensions that map clearly to value and that you can measure exactly (API calls, GB processed, seats).
- Record usage events in your own system first, as the source of truth, keyed by customer and time window.
- An hourly job aggregates usage per customer and dimension and sends it to the marketplace metering API, with an idempotent identifier per customer, dimension and hour so retries do not double-charge.
- Store each submission and the marketplace response; reconcile monthly against marketplace billing reports.

**What goes wrong:**
- **Missed or late submissions:** marketplaces only accept records within a limited time window, so a stuck job loses revenue. Alert on gaps.
- **Double counting** after retries without idempotency.
- **Customer mapping errors:** usage sent for the wrong customer identifier after account merges.
- **Cancelled subscriptions:** sending usage after cancellation fails; stop metering on the unsubscribe event.
- **Disputes:** customers ask why the bill is high. Give them a usage dashboard that matches what you meter.
- **Contract plus overage confusion:** be clear which usage is prepaid and which is billed on demand.

### Partner enablement and certification

#### Design a partner enablement program for a new product launch.

**Goal:** within a set time after launch, a defined number of partners can sell, demo and implement the product without our engineers on every call.

**Segment the audience:** partner sellers (positioning, qualification, when to involve us), pre-sales engineers (demo, architecture, objections), and delivery consultants (implementation, troubleshooting).

**Content and formats:**
- A short launch briefing and a FAQ for sellers.
- A hands-on lab: a working sandbox, a step-by-step guide and a reference implementation in a public repository.
- A reference architecture and an implementation playbook (sizing, security, common pitfalls).
- Recorded sessions plus live office hours each week for the first months.
- A certification path for delivery consultants (see the next answer).

**Sequence:** start with a design-partner group of two or three strategic partners before launch, use their feedback to fix content, then scale to the wider program through the partner portal.

**Measure:** partners completing the lab, certified consultants, partner-led demos and deals, implementation quality, and the questions partners still ask us (each repeated question becomes a doc).

#### How would you design a technical certification program for SI partners?

**Purpose:** give customers confidence that a certified SI consultant can deliver, and give us a lever on delivery quality.

**Design:**
- **Levels:** for example an associate level (fundamentals, multiple-choice exam) and a professional level (hands-on practical exam or a reviewed project).
- **Role-based tracks:** architect, developer or implementer, and administrator, built from a job-task analysis of what a good implementation actually requires.
- **Assessment that tests skill, not memory:** scenario questions, hands-on labs in a sandbox, and for the top level a reviewed customer project or a capstone build.
- **Exam integrity:** question pools, proctoring for higher levels, and regular refreshes.
- **Expiry and renewal:** certifications expire (for example every two years) or require short update modules as the product changes.
- **Firm-level tiers:** partner firms reach tiers (for example registered, select, premier) by number of certified people, completed customer projects and customer satisfaction, and tiers unlock benefits such as lead sharing and marketplace private-offer participation.

**Measure:** pass rates, certified people actually staffed on projects, and implementation outcomes of certified versus non-certified teams. If certified teams do not deliver better results, the exam is testing the wrong things.

#### An SI partner delivered a poor implementation for a joint customer. How do you handle it?

**First the customer:** the customer does not care whose fault it is. Join a call with the customer and the SI, listen, and agree on the immediate problems and a recovery plan with dates. If needed, assign our own engineer to help stabilise.

**Then the facts:** review the implementation with the SI's lead: architecture, configuration, data, and where it departed from our best practices. Separate their mistakes from gaps in our product or documentation.

**Then the partner:**
- Have a direct, private conversation with the SI's practice lead, with evidence, not blame.
- Agree corrective actions: staff the project with certified consultants, a design review by us at key milestones, and a lessons-learned session.
- Update our side: fix documentation or training gaps that contributed.

**Then the program:** record it in partner health tracking. One bad project is a coaching moment; a pattern should affect tier status, lead sharing and whether we recommend them.

In the interview, show that you protect the customer, stay fair to the partner, and fix the system, not just the incident.

#### How do you write documentation and sample code that partners can build on without your help?

- **Start from the partner's tasks, not our features:** "authenticate", "sync contacts", "handle a webhook", "go live", each with one clear path.
- **A quickstart that works in under 15 minutes:** sandbox credentials, copy-paste commands, and a visible result.
- **Complete, runnable sample code** in a public repository, in the languages partners use, with tests and CI so it does not rot. Show error handling, retries and pagination, not only the happy path.
- **Reference docs generated from the API specification** (OpenAPI), so they match the real API.
- **Explain the hard parts honestly:** rate limits, eventual consistency, ordering, idempotency, and known limitations.
- **A go-live checklist and troubleshooting guide** with common error codes and fixes.
- **Changelog and deprecation notices** in one place partners can subscribe to.
- **Close the loop:** track which questions partners still ask in office hours and support, and turn each repeated question into a doc update.

The test: a new partner engineer can build a working integration without a call with us.

#### Enablement exercise: teach a partner's engineers our product in 20 minutes. How do you structure it?

**Before:** ask who is in the room (sellers, architects, developers) and what they need to do afterwards. Teach to that outcome.

**Structure (20 minutes):**
1. **Why it matters to them (2 min):** the customer problem and the business opportunity for the partner, such as the services work it creates.
2. **Mental model (4 min):** one architecture diagram showing the main components and where their work fits.
3. **Live demo (6 min):** the most common customer scenario end to end, narrated as "here is what you would do on a project".
4. **Integration points and pitfalls (4 min):** APIs, auth, what usually goes wrong on implementations and how to avoid it.
5. **Next steps (2 min):** lab, certification, sandbox access, where to get help.
6. **Questions (2 min),** plus a follow-up channel.

**Tips:** check understanding with one or two questions to the audience, have a recorded backup of the demo, keep jargon low, and end with one action each person should take this week. Interviewers grade clarity, audience awareness and how you handle a question you cannot answer: say so, and promise a follow-up.

### Co-selling and alliances

#### What is co-selling, and what is the partner engineer's role in a co-sell deal?

**Co-selling** is when your sales team and a partner's sales team work the same customer opportunity together, each bringing something: a cloud provider brings its relationship and the customer's committed spend; an SI brings implementation capacity and trust; an ISV brings a complementary product. Cloud providers run formal programs for this (for example AWS's APN Customer Engagements (ACE) program for sharing opportunities, and Microsoft's co-sell programs in Partner Center), usually with eligibility requirements such as a technical validation or a marketplace listing.

**The partner engineer's role:**
- **Technical bridge:** brief the partner's technical team on the joint solution and make sure both companies' engineers tell the same story.
- **Joint architecture:** design how the products fit together for this customer, and lead or support a joint proof of concept.
- **Answer the hard questions:** integration depth, security, data flows, who supports what.
- **Readiness:** keep co-sell requirements met (technical validation, reference architectures, case studies) so the field teams are allowed to co-sell.
- **Register and update the opportunity** correctly, so the partner and our team get credit, which drives future co-sell support.

Success looks like a won deal where the customer saw one team, not two vendors pointing at each other.

#### How do you handle channel conflict between a partner and your direct sales team?

**Channel conflict** happens when a partner and your direct team compete for the same customer or deal, for example both proposing or the partner fearing you will take over the services work.

**Prevention:**
- Clear rules of engagement: deal registration (the first to register a qualified opportunity gets protection for a period), defined segments or territories, and which services the partner owns.
- Compensation that does not punish your sellers for partner-involved deals (for example neutral or credit-sharing comp).

**When it happens:**
1. Get the facts: who registered, who is actually working the customer, what the customer wants.
2. Bring the account executive, the partner manager and the partner's lead together quickly; do not let it fester in email.
3. Decide by the rules of engagement and by what serves the customer best. Often the answer is to split roles: we lead the licence, the partner leads implementation.
4. Escalate to sales and partnerships leaders if the rules do not cover it, and update the rules afterwards.

**As the engineer:** stay neutral and technical, and never disparage the partner to the customer. Partners remember who treated them fairly.

#### A strategic partner asks for a roadmap commitment to win a deal. How do you respond?

**Understand the real need:** what exactly the customer requires, by when, and whether it is a blocker or a preference. Often a workaround, a configuration or an integration through an existing API covers it.

**Do not commit what you cannot deliver.** Product commitments are made by product management, not by a partner engineer, and an unmet promise damages both the partner and the customer relationship.

**Steps:**
1. Gather details: use case, customer, deal size, deadline, and how many other partners or customers want the same thing.
2. Propose what we can do now: a workaround, a reference implementation, a services-built extension by the partner, or a phased approach.
3. Take a structured request to product with the business case. If product agrees to a date, put it in writing with the right caveats; if not, tell the partner clearly and quickly.
4. Help the partner position the answer with the customer honestly.

**Phrase to use:** "I can't commit our roadmap, but I can get you a clear answer from product by Friday, and here is what we can do today."

#### What goes into the technical part of a joint business plan with a partner?

A joint business plan sets shared goals for the year. The technical part makes the commercial goals achievable:

- **Target solutions:** two or three joint solutions or use cases the plan focuses on, tied to the customer segments in the plan.
- **Integration work:** what each side will build or improve, with owners, milestones and a definition of done (for example a certified connector, a marketplace listing, a joint reference architecture).
- **Enablement targets:** number of partner people trained and certified, by role, by quarter.
- **Assets:** demos, reference architectures, solution briefs, workshops, sample code, a joint case study.
- **Co-sell motions:** which accounts or segments, the technical support model for joint deals, and how opportunities are shared.
- **Support model:** who handles which issues for joint customers, escalation paths and response times.
- **Metrics and review rhythm:** quarterly business reviews checking integration adoption, certified practitioners, joint pipeline and delivery quality.
- **Dependencies and risks:** roadmap items needed from either side.

Keep it short and owned: every line has a name on each side and a date.

#### How do you work with a cloud provider's partner team to get co-sell support?

**Become eligible:** meet the provider's program requirements, which typically include a marketplace listing, a technical validation of your product (for example AWS's Foundational Technical Review for software partners) and published customer references. Requirements change, so check the current program guide.

**Make it easy for their sellers:** cloud provider sellers are measured on cloud consumption and their own goals, so show how your product drives usage of their services and helps their customers. Provide a one-page "why it matters to you" brief, a simple qualification guide and a joint reference architecture.

**Build relationships at several levels:** the partner development manager (program owner), partner solutions architects (technical validation, joint architectures) and field sellers in the regions that matter.

**Share opportunities properly:** register joint opportunities in the provider's partner portal, keep them updated, and close the loop on wins. Accurate pipeline data is how you earn more support.

**Give back:** help their field teams win their deals, run joint workshops and events, and publish joint content. Co-sell is a relationship; partners who only take rarely get prioritised.

### Joint reference architectures and ecosystem design

#### Whiteboard: design a joint reference architecture for our product running on a cloud provider for a regulated customer.

**Clarify:** the regulation (for example HIPAA, PCI DSS, or data residency rules), deployment model (our SaaS, customer-hosted, or SaaS with private networking), data types, and the customer's cloud account structure.

**A typical design (our SaaS connecting to a customer's AWS environment):**
- **Network:** private connectivity instead of the public internet, for example AWS PrivateLink from the customer's VPC to our service endpoint; no public endpoints for data paths.
- **Identity:** SSO through the customer's identity provider (SAML or OIDC), SCIM for provisioning, and cross-account IAM roles with an external ID for any access to customer resources, scoped to least privilege.
- **Data:** encryption in transit (TLS) and at rest with customer-managed keys (KMS) so the customer can revoke access; data stays in the required region.
- **Logging and audit:** our audit logs exported to the customer's logging or SIEM; cloud audit logging enabled on their side.
- **Shared responsibility:** a clear table of what the customer, we and the cloud provider each own.
- **Resilience:** multi-AZ deployment, stated RTO and RPO, backups.

**Deliverables:** the diagram, an infrastructure-as-code template the customer can deploy, a security and compliance mapping, and a deployment guide. Then ask the interviewer which part to go deeper on.

#### How do you validate and maintain a joint reference architecture so it does not go stale?

**Validate:**
- Build it for real from the published infrastructure-as-code in a clean account, not just as a diagram.
- Test the main scenarios, failure cases and security controls; record performance and cost numbers with their assumptions.
- Review it with the partner's architects and security team, and get sign-off from both sides.
- Pilot it with at least one real customer and update it from what you learn.

**Maintain:**
- Keep the code in a repository with CI that deploys and tests it on a schedule (for example monthly) and on new versions of either product.
- Give it an owner on each side and a "last validated" date and product versions on the page.
- Subscribe to both companies' changelogs and deprecations; review it each quarter.
- Collect field feedback from partner and customer implementations.
- Retire architectures that no one uses rather than leaving stale ones online.

The common failure is a beautiful diagram published once and never tested again. Automated deployment tests are what keep it honest.

#### How do you scope and build a joint solution or accelerator with an SI?

**Pick the right problem:** a repeated customer need the SI has seen several times, where our product plus their services shortens delivery (for example an industry data model, a migration toolkit or a prebuilt integration pack).

**Scope it like a product:**
- Target customer and use case, the time it should save (for example cut implementation from twelve weeks to six), and what is in and out of scope.
- Ownership: who builds what, who owns the code and intellectual property, who supports it, and how it is licensed or sold (often the SI owns it and sells services around it).
- A minimum version to pilot with one real customer.

**Build:**
- Joint team with a technical lead on each side, short iterations and a shared repository.
- We provide product expertise, design reviews, sandbox environments and early API access; the SI provides domain knowledge and delivery patterns.
- Package it: installation guide, configuration options, tests and a demo.

**Launch:** pilot with a customer, publish a case study, train the SI's wider practice, and list it where customers look (partner directory or marketplace). Review usage after two quarters; kill or improve it based on adoption.

#### Design the review and certification process for third-party integrations listed in our app directory.

**Goals:** customers can trust listed apps; partners get a predictable, fast process.

**Process:**
1. **Submission:** app metadata, requested scopes with justifications, privacy policy, support contact, test account and a demo video.
2. **Automated checks:** OAuth configuration, requested scopes against the minimum needed, webhook signature verification, rate-limit behaviour against a test environment, dependency and secret scanning where code is provided.
3. **Security review:** data handling (what is stored, where, retention), encryption, vulnerability disclosure process; a questionnaire for low-risk apps and a deeper review (possibly a third-party penetration test report) for apps with broad data access.
4. **Functional review:** install, uninstall and reinstall, error handling, and that the listing describes what the app really does.
5. **Publication** with tier badges (for example "verified" versus "built by us").

**Ongoing:** recertify yearly or on scope changes, monitor error rates and complaints, have a takedown and customer notification process, and publish the review guidelines and expected timelines so partners can prepare. Large platforms (for example Salesforce AppExchange, Atlassian Marketplace, Shopify App Store) run similar reviews.

### Joint customer issues and feedback

#### A joint customer's integration broke after the partner updated their product. Walk through how you triage it.

1. **Stabilise and communicate:** acknowledge to the customer quickly, name a single owner (often me) so they are not bounced between vendors, and set an update schedule.
2. **Scope:** which customers are affected (only this one or all users of the integration), since when, and which operations fail.
3. **Correlate with the change:** compare the failure start time with the partner's release; read their release notes and changelog for API, auth or schema changes.
4. **Get evidence:** our logs and request IDs, error codes from the partner's API, a minimal reproduction in a sandbox with the new partner version.
5. **Engage the partner's engineers** through the agreed escalation path with the evidence, not a vague "it is broken on your side".
6. **Mitigate:** a workaround on our side (handle the new field, fall back to another endpoint), or ask the partner to roll back or hotfix.
7. **Fix and verify** with the customer.
8. **Prevent:** a shared test suite run against the partner's preview or beta releases, subscription to their changelog, contract tests, and an agreed advance-notice period for breaking changes in the partnership agreement.

#### How do you handle security and data-sharing reviews when integrating with a partner?

**Map the data flow first:** which data moves, in which direction, where it is stored, for how long, and who can access it. A data flow diagram is the core artefact of the review.

**Review areas:**
- **Authentication and authorisation:** least-privilege scopes, token storage, rotation, revocation.
- **Data protection:** encryption in transit and at rest, data residency, retention and deletion when the customer disconnects.
- **Partner security posture:** their certifications and reports (for example SOC 2 Type II, ISO 27001), penetration test summaries, incident response and breach notification commitments.
- **Legal and privacy:** data processing agreement, sub-processor listing, regulations that apply (GDPR, HIPAA or local data protection laws).
- **Customer control:** customers can see what is shared, consent to it and switch it off.

**Process:** involve security and legal early, not at launch; use a standard questionnaire to avoid starting from scratch every time; record decisions and accepted risks with an owner; and re-review when scopes or data flows change.

For a joint customer's security review, prepare a shared document both companies stand behind so the customer gets one consistent answer.

#### How do you bring partner feedback to your product team and get integration features prioritised?

**Collect it in one place:** a structured log of requests with the partner, the joint customers affected, the revenue or pipeline at stake, the workaround and the urgency. Anecdotes from calls are easy to ignore; aggregated evidence is not.

**Translate into product language:** describe the problem and the user, not the partner's proposed solution. Group similar requests across partners to show a pattern.

**Quantify:** deals blocked or slowed, integration adoption limited, support tickets caused, and what the partner will commit in return (for example building on the feature, joint marketing).

**Use the product process:** bring the top items to product planning on its cycle, with a named product manager owner; do not go around them through executives unless it is truly urgent.

**Close the loop:** tell partners what was accepted, what was declined and why, and when. Partners tolerate "no" much better than silence.

**Contribute where you can:** a prototype, a sample implementation or an SDK change from the partner engineering team can make a yes easier.

### Behavioral (role-specific)

#### Tell me about a time you influenced an external team you had no authority over.

**What they look for:** you understood the other team's incentives, built trust, used evidence, and got a result without escalation as the first move. Partner work is almost entirely influence without authority.

**STAR outline:**
- **S:** [The external team (vendor, partner, another company's engineers) and what you needed from them]
- **T:** [Why it mattered and why they had no reason to prioritise it]
- **A:** [How you learned their goals, framed the request around their benefit, made it easy (did part of the work, provided data or a prototype), and kept a regular cadence]
- **R:** [What changed, measurable impact, and the relationship afterwards]

**Example (adapt to your own experience):** "We needed a vendor to add a bulk endpoint because our integration hit their rate limits. Their roadmap was full. I gathered data on how many calls we made and how many of their other customers likely had the same pattern, wrote a short proposal with a suggested API shape, and offered to test a beta. They shipped it in the next quarter; our sync time dropped sharply, and the vendor later asked us to review other API changes."

#### Tell me about a time you built something that others reused at scale.

**What they look for:** you think in leverage, the role's core skill. You built a tool, template, integration, document or training that many others used, and you designed it to be reusable.

**STAR outline:**
- **S:** [The repeated problem many people or teams faced]
- **T:** [What you decided to build and why a reusable version was worth the extra effort]
- **A:** [Design choices for reuse: documentation, configuration, tests, ownership; how you got people to adopt it]
- **R:** [How many people or teams used it, time saved, quality improvements, how it was maintained]

**Example (adapt to your own experience):** "Each integration our team built re-implemented authentication, retries and logging. I extracted these into a small shared library with tests and a template project, then wrote a one-page guide. New integrations started from the template; setup time dropped from days to hours, and integration incidents caused by missing retries largely disappeared."

#### Tell me about a time you balanced the interests of two parties who wanted different things.

**What they look for:** you understood both sides' real interests, stayed fair and transparent, and found a solution that kept both relationships. This mirrors channel conflict and joint-customer disputes.

**STAR outline:**
- **S:** [The two parties (teams, a partner and a customer, sales and engineering) and the conflict]
- **T:** [Your role and what was at stake]
- **A:** [How you uncovered each side's underlying interest, the options you proposed, how you brought them together, how you communicated the decision]
- **R:** [Outcome for both parties; relationships afterwards; any rule or process you changed]

**Example (adapt to your own experience):** "Our sales team wanted a custom feature promised to close a deal; engineering wanted to protect a release date. I met each side, found sales needed a specific report rather than the full feature, and engineering could expose that through an existing export in a few days. Both agreed; the deal closed on time and the release did not slip. We then added a short intake step for deal-driven requests."

#### Tell me about a time you said no to a partner or customer.

**What they look for:** you can protect the company and the relationship at the same time: a clear no, a reason, and an alternative, delivered quickly and respectfully.

**STAR outline:**
- **S:** [The request and who made it]
- **T:** [Why saying yes would have been wrong: cost, risk, precedent, impossible timeline]
- **A:** [How you checked your reasoning, who you consulted, how you said no, and what alternative you offered]
- **R:** [How they reacted, the outcome, and what happened to the relationship]

**Example (adapt to your own experience):** "A large customer asked for direct read access to our production database for reporting. It would have broken our security model and set a precedent. I explained the risk plainly, and offered a scheduled data export to their warehouse with the fields they needed. They accepted after seeing a sample, and the export became a standard feature for other customers."

---

## Prep checklist

1. Re-read [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md) sections A and B until you can explain OAuth 2.0 flows, webhooks, idempotency, pagination and rate limiting without notes.
2. Build one small connector (two public APIs, incremental sync, retries, idempotent upserts, tests, README) and publish it; it doubles as your take-home practice.
3. Read the seller documentation for one cloud marketplace (AWS Marketplace SaaS integration is the most common) and be able to explain landing page, lifecycle events, entitlements and metering.
4. Learn the partner program of the company's main cloud provider: co-sell eligibility, technical validation, and how opportunities are shared.
5. Prepare a 20-minute enablement session on a product you know well, and deliver it to someone who will ask hard questions.
6. Draw one joint reference architecture (your product plus a cloud provider, with private networking, SSO, customer-managed keys and audit logging) from memory.
7. Research the company's partner ecosystem: its app directory or marketplace, top SI and technology partners, and which cloud marketplaces it lists on.
8. Prepare STAR stories for influence without authority, building something reused at scale, balancing two parties, and saying no, using [10 Behavioral](../../topics/10-behavioral/ANSWERS.md).
9. Practise two partner role-plays out loud: channel conflict, and a partner asking for a roadmap commitment.
10. Follow the [study plan](STUDY-PLAN.md) for a week-by-week schedule.

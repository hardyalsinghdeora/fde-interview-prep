# Target: HighLevel — Staff Engineer, CRM Backend

HighLevel (GoHighLevel) is an all-in-one CRM and marketing automation platform sold to agencies, which resell it to their own clients. Its engineering hiring for India is mostly remote and listed on [jobs.lever.co/gohighlevel](https://jobs.lever.co/gohighlevel).

> **Status check before applying:** this file was built from search-engine snapshots of the postings (the Lever site itself couldn't be opened from the session that wrote it). Titles, requirements and whether a role is still open can change. Open each link and confirm before applying or tailoring a résumé.

Related: [PROFILE.md](PROFILE.md) · [STORY-BANK.md](STORY-BANK.md) · [MY-ANSWERS.md](MY-ANSWERS.md)

---

## 1. Which openings fit my career

Ranked by how well my record matches today. All are backend; most ask for **8+ years** and **Node.js (TypeScript) or Go**.

| Rank | Role | What the team owns (from the posting) | Why it fits me | Main gap |
|---|---|---|---|---|
| 1 | [Staff Backend Engineer — Phone Billing](https://himalayas.app/companies/highlevel/jobs/staff-backend-engineer-phone-billing) | Metering and monetising phone usage (voice, messages, numbers): usage metering, rating, reserve/commit lifecycle, idempotency, reconciliation, audit trails. Asks for 2+ years at Staff level or comparable technical leadership, and a background in correctness-critical systems (billing, payments, ledgers) | **Closest match.** Invoicing and payment mapping (S3), reconciliation and drift detection (S4), the idempotency guard in the migration (S1), finance-auditable billing-period calculations | Usage-based (metered) billing rather than invoice/subscription billing; Node.js; telecom usage vocabulary |
| 2 | [Staff Engineer — Contacts Platform](https://jobs.lever.co/gohighlevel/f049d95e-5a06-4774-89bd-44b65c8ea312) | The contact creation engine and contact search; Elasticsearch, Firestore, ClickHouse indexing and schema design; 8+ years backend | CRM is my domain: CRM integrations, CRM → Postgres mapping and data porting, duplicate and ID-mapping problems from S1 | Elasticsearch, ClickHouse and Firestore at scale; Node.js or Go |
| 3 | [Staff Backend Engineer — Users](https://jobs.lever.co/gohighlevel/22d8e701-923e-4911-bbfc-35366b77ddcc) | Identity, access, auditing, notifications and security foundations; tenant isolation at data, API and infrastructure level; Agency → Account hierarchies for 100k+ agencies; RBAC/ABAC; API keys, OAuth, JWTs, scoped tokens; impersonation; large identity migrations | Row-level access scoping (S8), JWT auth at the API gateway, live migrations (S1, S7) | Deep OAuth and authorisation design at multi-tenant scale; Node.js |
| 4 | [Staff Engineer — CRM & Automation](https://jobs.lever.co/gohighlevel/4eb09872-4256-45b7-b754-634ba1a2f0c7) | Workflows, the automation engine: 3.1 billion enrollments and 21.5 billion action executions a month, peaks above 28,000 requests per second; GCP Pub/Sub, Cloud Tasks, Redis, MongoDB, Firestore, ClickHouse, Elasticsearch; Node.js (TypeScript) and Go on GKE; **10+ years** and ownership of large distributed systems through real failures | Business rules engines (S2), event-driven sync with ordering and retries (S1), CRM domain | The biggest scale jump; distributed queue internals; 10+ years expected |
| 5 | [Staff Backend Engineer — Trust & Control Safety Systems](https://jobs.lever.co/gohighlevel/7cc61c43-4203-467c-8b67-721925cb0fd1) | A real-time platform securing signups, SaaS checkouts, user management and communication usage (fraud and abuse controls) | Checkout and commerce flows, access control | Fraud/abuse detection, real-time risk scoring |
| 6 | Staff Engineer — Backend (general; seen on [Glassdoor](https://www.glassdoor.co.in/job-listing/staff-engineer-backend-highlevel-JV_KO0,22_KE23,32.htm?jl=1010270156446) and [freehire](https://freehire.me/jobs/staff-engineer-backend-gohighlevel-oguae5ih)) | Async processing with queues, events, streams and workers; Kafka, Pub/Sub, RabbitMQ or SQS experience | Broad backend and integration record; I've used AWS SQS | Depends on the team it maps to |

**Close but below my level (fallback, or a door in):** [SDE III — CRM Opportunities](https://jobs.lever.co/gohighlevel/ab0b9373-1219-4194-ae9a-6bdf50891749) (the deals and pipeline engine; 4+ years, Node.js or Go; Kafka/Redis Streams preferred). Its domain is the closest of all to my order and subscription work, but it's a level below Principal. There's also an AI variant, [SDE III — CRM Opportunities — AI](https://jobs.lever.co/gohighlevel/a718fab2-3fc6-4acd-b364-883f5fc59cac).

**Not a fit for this search:** Revenue Operations Analyst, the full-stack Vue.js roles (Workflows, Contacts SDE II), and Labs (feature flags and staged rollouts; interesting, but full-stack and mid-level).

**Suggested strategy:** apply first to **Phone Billing** (best evidence match) and **Contacts Platform** (best domain match). Prepare **Users** as a third. Prepare for **CRM & Automation** only after the Node.js and distributed-queue gaps below are closed, since it expects 10+ years at large distributed scale.

**Title note:** my current title is Principal SDE; HighLevel's equivalent band is Staff. Frame it as "the same scope: technical direction for a domain across teams", not a step down.

---

## 2. What I need to learn (gap → learning material)

Common stack across these postings: **Node.js (TypeScript), often NestJS, and Go; GCP (GKE, Pub/Sub, Cloud Tasks); MongoDB, Firestore, MySQL; Elasticsearch; ClickHouse; Redis; Kubernetes.** My record is PHP, SQL (MySQL and Postgres) and AWS (S3, SQS). [I-33: confirm which other languages I'm interview-ready in.]

| Priority | Gap | Why it matters here | Learning material | Practise by |
|---|---|---|---|---|
| **P1** | **Node.js and TypeScript in depth** | Every posting; reported interviews start with Node.js basics | [Node.js docs: the event loop, timers and `process.nextTick()`](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick); [Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop); book *Node.js Design Patterns* (Casciaro & Mammino); [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) | Rewrite one PHP service you know (e.g. the discount approval API) in TypeScript with NestJS, with tests |
| **P1** | **NestJS** | Named in at least one Staff posting; the common Node framework for large backends | [NestJS docs](https://docs.nestjs.com/) (modules, providers and DI, guards, interceptors, microservices) | Add auth guards and a queue consumer to the same service |
| **P1** | **Staff-level system design at multi-tenant scale** | Every Staff loop | *Designing Data-Intensive Applications* (Kleppmann); *System Design Interview* vol. 1 and 2 (Alex Xu); repo: [03 system design](../topics/03-system-design/ANSWERS.md), [13 CS fundamentals](../topics/13-cs-fundamentals/ANSWERS.md) | The design questions in section 3 below, timed, out loud |
| **P1** | **Billing correctness: metering, rating, idempotency, ledgers, reconciliation** (Phone Billing) | The core of the best-fit role | Stripe engineering blog: [Designing robust and predictable APIs with idempotency](https://stripe.com/blog/idempotency); Martin Fowler's accounting patterns ([Account](https://martinfowler.com/eaaDev/Account.html), [Accounting Entry](https://martinfowler.com/eaaDev/AccountingEntry.html)); repo: [double-entry primer](../topics/05-learning-round/ANSWERS.md#a-domain-concept-supply-chain-clinical-trials-double-entry-bookkeeping-options-pricing), [idempotency keys](../topics/08-cloud-devops-integration/ANSWERS.md#idempotency-what-is-it-and-how-do-you-implement-idempotency-keys) | Design the phone billing system in section 3 and write the reserve/commit code |
| **P2** | **GCP Pub/Sub and Cloud Tasks** | The Workflows stack; async processing everywhere | Google Cloud docs: [Pub/Sub overview](https://cloud.google.com/pubsub/docs/overview), ordering keys, exactly-once delivery, dead-letter topics; [Cloud Tasks overview](https://cloud.google.com/tasks/docs/dual-overview); map to what I know from SQS with repo topic [16-aws, section E](../topics/16-aws/README.md#e-application-integration) | Build a small Pub/Sub consumer with retries and a dead-letter topic on the free tier |
| **P2** | **MongoDB and Firestore data modelling** | Primary stores in several teams | [MongoDB University](https://learn.mongodb.com/) (free: data modelling, indexes, aggregation); [Firestore data model docs](https://firebase.google.com/docs/firestore/data-model) | Model contacts with custom fields and tags in MongoDB; explain the index choices |
| **P2** | **Elasticsearch** (Contacts Platform) | Contact search and smart lists | [Elasticsearch guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html): mappings, analysers, shards and replicas, aliases and reindexing, keeping it in sync with the primary database | Index 1M fake contacts; build filtered search and a zero-downtime reindex with aliases |
| **P2** | **Redis patterns** | Caching, rate limits, locks, queues | [Redis docs](https://redis.io/docs/latest/) (data types, streams, distributed locks and their limits, eviction) | Implement a per-tenant rate limiter (repo: [02 coding](../topics/02-coding/README.md)) |
| **P3** | **ClickHouse** | Analytics on contacts and workflows | [ClickHouse docs](https://clickhouse.com/docs): MergeTree engines, ORDER BY keys, materialised views | Load event data; compare query times with Postgres |
| **P3** | **Go basics** | Accepted alternative to Node.js; some services | [A Tour of Go](https://go.dev/tour/), [Effective Go](https://go.dev/doc/effective_go) | Port the rate limiter to Go |
| **P3** | **Kubernetes on GKE** | Thousands of pods; on-call | Repo: [08 section D](../topics/08-cloud-devops-integration/README.md#d-containers-orchestration--iac); [GKE docs](https://cloud.google.com/kubernetes-engine/docs) on autoscaling | Explain HPA and noisy-neighbour isolation |
| **P3** | **The Staff role itself** | Staff interviews test technical leadership across teams | *Staff Engineer* (Will Larson) and *The Staff Engineer's Path* (Tanya Reilly) | Map S1, S9, S10 and S12 to "set direction, de-risk, multiply others" |
| **Product** | **HighLevel's product** | "Why HighLevel?" and product-sense questions | Start a free trial; the [HighLevel help centre](https://help.gohighlevel.com/) (contacts, opportunities, workflows); the [developer API docs](https://highlevel.stoplight.io/) *(verify current URL)* | Build one workflow (form submit → tag → wait → SMS) and note what was confusing |

**Repo topics that already cover a lot of this:** [02 coding](../topics/02-coding/README.md) (DS&A and LLD), [03 system design](../topics/03-system-design/README.md), [07 data and SQL](../topics/07-data-and-sql/README.md), [08 cloud and integration](../topics/08-cloud-devops-integration/README.md), [12 debugging](../topics/12-debugging-troubleshooting/README.md), [13 CS fundamentals](../topics/13-cs-fundamentals/README.md).

**Reported interview process** (aggregators and a few candidate reports; *verify with the recruiter*): recruiter screen → technical round (Node.js basics, a DS&A or recursion problem, backend theory) → a take-home or machine-coding exercise for some roles (one report: a MERN app handling time zones) → system design for senior roles → engineering leadership and team-fit conversations, over a few weeks. Sources: [Glassdoor](https://www.glassdoor.com/Interview/highlevel-Senior-Software-Engineer-Interview-Questions-EI_IE3453927.0,9_KO10,34.htm), [Dataford](https://dataford.io/interview-guides/gohighlevel), [Knok](https://knok.work/blog/highlevel-software-engineer-interview.html).

**Six-week plan** (adjust to [I-41: hours per week]):

| Week | Focus |
|---|---|
| 1 | Node.js event loop, async patterns, TypeScript; start the NestJS rewrite |
| 2 | NestJS service with auth guards and tests; MongoDB data modelling; DS&A mediums daily ([02 coding](../topics/02-coding/README.md)) |
| 3 | Billing correctness deep dive; Phone Billing and Contacts designs from section 3 |
| 4 | Pub/Sub, Cloud Tasks, Redis; Workflows and multi-tenant authorisation designs |
| 5 | Elasticsearch and ClickHouse; the rest of section 3; mock system design twice |
| 6 | Staff behavioural stories, "Why HighLevel", full mock loop; tailor the résumé per role |

---

## 3. Questions and model answers

Built from the postings and from my own record. Answers use my stories (S1–S12 in [STORY-BANK.md](STORY-BANK.md)); fill `[I-xx]` placeholders from [INPUTS-NEEDED.md](INPUTS-NEEDED.md) before using them.

### A. Fit and motivation

- [ ] Why HighLevel, and why this team?
- [ ] Walk me through your most relevant CRM and billing experience.
- [ ] You're a Principal engineer today. Why a Staff role?
- [ ] Our stack is Node.js, Go and GCP; your background is PHP, SQL and AWS. How will you close that gap?

### B. System design (HighLevel-style)

- [ ] Design a usage metering and billing system for phone calls and SMS (reserve/commit, idempotency, reconciliation).
- [ ] Design the contact creation engine: creating and updating contacts from many sources with deduplication.
- [ ] Design contact search and smart lists over hundreds of millions of contacts.
- [ ] Design a workflow automation engine: triggers, waits, branches and actions at 28,000 requests per second.
- [ ] Design multi-tenant authorisation for an Agency → Sub-account → User hierarchy, including impersonation.
- [ ] Design API keys, OAuth and scoped tokens for a public API and app marketplace.
- [ ] Design reliable outbound webhooks to customers' systems.
- [ ] Design a bulk import of one million contacts from a CSV.
- [ ] Design the opportunities (deals and pipeline) service, including stage history and reporting.

### C. Backend deep dives

- [ ] Explain the Node.js event loop. What blocks it, and how do you find and fix that in production?
- [ ] How do you run CPU-heavy or long work in Node.js without hurting API latency?
- [ ] Node.js vs. Go for a backend service: when would you choose each?
- [ ] Pub/Sub vs. Cloud Tasks vs. a Redis queue: when do you use each, and what delivery guarantees do you get?
- [ ] How do you keep Elasticsearch in sync with the primary database, and how do you reindex with zero downtime?
- [ ] How would you model contacts with custom fields and tags in MongoDB, and which indexes would you create?
- [ ] One large agency is overloading a shared service (a noisy neighbour). What do you do?
- [ ] How do you use Redis for distributed locks, and when are they unsafe?
- [ ] How would you migrate contacts from one datastore to another with zero downtime?

### D. Correctness and billing

- [ ] How do you guarantee a customer is never double-charged or under-charged?
- [ ] Explain the reserve/commit (authorise/capture) pattern for prepaid usage balances.
- [ ] How do you design reconciliation between your usage records, a carrier's invoice and customer charges?
- [ ] "Exactly-once processing": is it possible, and what do you actually build?

### E. Staff-level leadership and behavioural

- [ ] Tell me about a system you owned through a real production failure.
- [ ] Tell me about setting technical direction across several teams.
- [ ] Tell me about a large migration you led on a live system.
- [ ] Tell me about a time you disagreed with another senior engineer on architecture.
- [ ] How do you raise the engineering bar of the teams around you?
- [ ] How do you decide what to build versus buy (for example, a workflow engine or a billing system)?

### F. Coding

- [ ] Implement a per-tenant rate limiter in TypeScript (token bucket).
- [ ] Implement a retry helper with exponential backoff, jitter and an idempotency key.
- [ ] Implement contact deduplication: group records that share an email or phone number.

---

## Model answers

### A. Fit and motivation

#### Why HighLevel, and why this team?

> HighLevel is a CRM and automation platform at real scale, and that's exactly where my experience is: I've spent the last few years building the commercial platform of a business on top of a CRM, doing CRM integrations, order and subscription models, invoicing, and access control, and migrating live data between old and new systems without downtime. What attracts me is the multiplier: every agency resells the platform to its own clients, so a correctness or reliability improvement in, say, billing or contacts reaches hundreds of thousands of businesses. For **Phone Billing** specifically: it's a correctness-critical domain (metering, idempotency, reconciliation), which is the work I'm proudest of. For **Contacts**: contacts are the core object of a CRM, and I've dealt with exactly the identity, dedupe and data-porting problems that platform faces.

Add one specific product observation from your trial ("I built a workflow and noticed…"). Never say "remote work" as the main reason.

#### Walk me through your most relevant CRM and billing experience.

Structure: three projects, each with the problem, my design, and the result.

1. **Commercial platform on a CRM** (S2, S3): order and subscription data model, product and SKU catalogue, a discount rules engine with a four-tier approval ladder, invoicing integrated with the accounting system, payment mapping tied to cash collection. "This is the billing side: correctness, auditability, and a business metric (days-sales-outstanding)."
2. **Live migration** (S1, S4): legacy MySQL estate to Postgres services across six product lines, with a one-time backfill separated from a continuous reverse sync, an idempotency guard, ordering constraints, retries, rollback, and reconciliation. "This is the data platform side: moving and keeping in sync a lot of CRM-linked data with zero downtime."
3. **Access control** (S8): row-level scoping where leadership sees everything and others see records where they're the assigned salesperson *or* consultant, enforced on lists, KPI counts and writes. "This maps to the multi-tenant authorisation work in the Users team."

Close with: "I lead a squad of four and own delivery end to end." [I-36: total team size defensible.]

#### You're a Principal engineer today. Why a Staff role?

> Titles differ between companies; what matters is scope. My current scope is technical direction for one domain — service boundaries, contract-first integrations, migration patterns — that other engineers implement from my specs. HighLevel's Staff role is the same scope, on a much larger system and customer base. I'd rather have Staff scope at HighLevel's scale than a bigger title on a smaller one.

#### Our stack is Node.js, Go and GCP; your background is PHP, SQL and AWS. How will you close that gap?

> Be honest and specific. The concepts transfer directly: async processing, queues, idempotency, data modelling, and API design are the same problems in a different runtime. I've already [I-33: what you've built in Node.js/TypeScript so far — the NestJS rewrite of a service you know]. The parts I'm deliberately studying are the Node.js event loop and its failure modes, and GCP's Pub/Sub and Cloud Tasks semantics compared with SQS, which I've used. I learn fast when there's a real system (S7: I moved every user of a product to a new stack), and at Staff level the value I bring is in the design and correctness decisions, which don't depend on the language.

Only claim what you've actually built. Do the NestJS project before applying.

### B. System design (HighLevel-style)

#### Design a usage metering and billing system for phone calls and SMS (reserve/commit, idempotency, reconciliation).

**Clarify:** prepaid wallet or postpaid invoicing (or both, per agency)? Who is charged: the agency, which rebills its sub-accounts with a markup? Rating rules (per minute, per segment, by country and number type)? Event volume (millions of calls and messages a day)? Source of usage (the telecom provider's callbacks; for HighLevel's LC Phone this is a carrier/CPaaS provider)?

**Requirements:** never double-charge or lose usage; block usage when a prepaid balance runs out; a full audit trail; reconcile against the provider's invoices; agencies set rebilling markups for their clients.

**Architecture:**
```
Call/SMS starts -> Balance service: RESERVE estimated cost (hold on wallet)  -> allow / deny
Provider events (call ended, SMS delivered, with provider IDs) -> ingestion (dedupe on provider event ID)
  -> Rating engine (rate card version, country, number type, duration rounding) -> rated usage record
  -> Balance service: COMMIT actual cost, RELEASE the rest of the hold   (idempotent on usage ID)
  -> Ledger (append-only double-entry: wallet, revenue, provider cost, agency markup)
  -> Rebilling: agency -> sub-account charges with markup
Daily reconciliation: our usage vs provider records vs ledger -> discrepancies queue
Expiry job: holds never committed (lost "ended" events) -> query provider -> commit or release
```

**Deep dives:**
- **Idempotency keys** at every step: the provider's event ID for ingestion, a usage ID for commit. Use a unique constraint in the database, not only a cache check. Repeated callbacks are normal.
- **Reserve/commit** on the wallet with a conditional update (`balance - holds >= amount`) or a single-writer per wallet to avoid races; see the reserve/commit question below.
- **Rating:** versioned rate cards (a usage event is rated with the card valid at its event time); store the rate card version on every rated record so re-rating is explainable.
- **Ledger:** append-only, integer minor units, every movement as balanced entries; corrections as reversing entries, never edits. The wallet balance is derived from the ledger (or cached and checked against it).
- **Late and out-of-order events:** the "ended" event may come before "started", or hours later; the state machine must accept any order.
- **Partition by account** for ordering and throughput; hot accounts (large agencies) need sharding of holds.

**Trade-offs:** strict real-time balance checks add latency to call setup → reserve a small estimated amount and top up holds during long calls; accept that a tiny overrun is possible and bounded.

**Metrics:** unrated usage age, reconciliation mismatch rate and value, holds older than N hours, double-charge incidents (target zero), rating latency.

**My evidence:** S3 (invoice and payment mapping), S4 (reconciliation), S1 (idempotency guard and ordering). "The lesson I'd bring: build reconciliation on day one, not after the first drift."

#### Design the contact creation engine: creating and updating contacts from many sources with deduplication.

**Clarify:** sources (forms, imports, API, integrations, inbound calls and SMS, Facebook leads); dedupe rules per sub-account (email, phone, both, configurable); volume and peaks (a big agency's ad campaign); what happens on a match (merge, update, ignore) and who decides.

**Requirements:** one contact per real person per sub-account (per its dedupe settings); idempotent ingestion; concurrent creates for the same person must not create duplicates; low latency for interactive creates; triggers for workflows downstream.

**Architecture:**
```
Sources -> Contact API (validate, normalise: lowercase email, E.164 phone, trim names)
  -> Upsert with dedupe:
       lookup keys (sub_account, normalised email) / (sub_account, normalised phone)
       unique index or key table per identifier  -> single atomic upsert
  -> Primary store (contacts: document with standard + custom fields, tags)
  -> Change events (Pub/Sub) -> search index (Elasticsearch), analytics (ClickHouse), workflow triggers
```

**Deep dives:**
- **Race on concurrent creates:** two webhooks for the same email arrive in the same millisecond. Fix with a **unique constraint on (sub_account_id, normalised identifier)** in a key table, and insert-or-get semantics; a lock in Redis alone is not enough (see the Redis lock question).
- **Normalisation** is most of the dedupe quality: phone number parsing to E.164 with a default country per sub-account, email canonicalisation (lowercase; don't strip Gmail dots unless the business agrees).
- **Merge rules:** which source wins for each field (latest non-empty, or source priority), with an audit trail; manual merge and un-merge.
- **Events:** emit "contact created/updated" after commit (outbox pattern) so search and workflows never see a contact that didn't commit.
- **Hot tenants:** partition queues by sub-account; rate-limit bulk sources so one agency's import doesn't slow everyone.

**My evidence:** S1: an idempotency guard stopped double-porting records that had already arrived through the CRM path, plus ID mapping between systems.

#### Design contact search and smart lists over hundreds of millions of contacts.

**Clarify:** query types (full-text name/email/phone, filters on tags, custom fields, dates, activity like "opened an email in the last 7 days"); freshness requirement (seconds?); result counts and exports; per sub-account isolation.

**Architecture:** primary store → change events → **Elasticsearch** index of contacts (denormalised: standard fields, custom fields, tags, last-activity fields), with routing by sub-account; activity aggregates (email opens, appointments) computed in **ClickHouse** or a stream processor and written back as fields; the API translates smart-list filters into ES queries, always with a mandatory `sub_account_id` filter.

**Deep dives:**
- **Index design:** keyword vs text fields (exact filters vs full-text), `phone` normalised as keyword plus an n-gram field for partial search; custom fields as a nested or flattened structure, since the number of fields is unbounded (avoid "mapping explosion": don't create a new mapped field per customer custom field).
- **Sharding:** routing by sub-account keeps a tenant's data on few shards (fast queries); very large agencies may need their own index.
- **Freshness:** async indexing means a few seconds of lag; for "I just created it and can't find it", read-your-writes via a short-lived fallback to the primary store, or refresh on demand for single-contact views.
- **Counts and exports:** exact counts on huge sets are expensive; show approximate counts above a threshold, and run exports as background jobs with `search_after`/point-in-time, not deep pagination.
- **Isolation:** the tenant filter is applied in one shared query builder, never per endpoint (a missed filter is a data leak).
- **Reindexing:** see the Elasticsearch sync question.

#### Design a workflow automation engine: triggers, waits, branches and actions at 28,000 requests per second.

**Clarify:** triggers (contact created, tag added, form submitted, appointment booked, inbound message); step types (send email/SMS, wait for a duration or until a date or event, if/else, webhook, update contact, goal steps); volume (billions of enrollments and actions a month; peaks of tens of thousands per second); guarantees (each action at most once in effect; ordering per contact); user edits to a live workflow.

**Core model:** a workflow **definition** (versioned graph of steps) and an **enrollment** (a contact moving through one version), with state = current step, context data, and status. Each step execution is a **task**.

**Architecture:**
```
Domain events (contact.updated, form.submitted, ...) -> Trigger matcher
   (index of active triggers per sub-account; filter cheaply before loading definitions)
   -> create enrollment (idempotent on event ID + workflow ID)
   -> Step executor workers (stateless, autoscaled on queue depth)
        - run the step (call the email/SMS/CRM service) with an idempotency key = enrollment ID + step ID + attempt group
        - persist new state (enrollment store) and enqueue the next step
   -> Waits: schedule the next task for a future time (Cloud Tasks / a timer service / a delay queue in a sorted set)
   -> Wait-for-event: register a waiter; the event router wakes matching enrollments
Dead-letter + retry policy per step type; per-tenant quotas and fairness
Observability: per-step latency, failures, backlog by tenant
```

**Deep dives:**
- **Idempotent actions:** retries are guaranteed to happen; an SMS must not be sent twice. Pass an idempotency key to the messaging service and record "step done" atomically with enqueuing the next step (outbox) so a crash between them doesn't double-execute.
- **Timers at scale:** millions of waiting enrollments; don't hold them in memory. Store `wake_at` and use a scheduler that pulls due items in batches (bucketed by minute), or a managed scheduler; handle the "thundering herd" of many waits ending at the same time (e.g. 9:00 a.m. in each time zone).
- **Fairness and noisy neighbours:** one agency enrolling 2 million contacts must not delay everyone; per-tenant queues or weighted fair scheduling, and quotas.
- **Versioning:** existing enrollments continue on the version they started with, or migrate explicitly; editing a step must not corrupt in-flight state.
- **Loops and runaway workflows:** a workflow that updates a contact which re-triggers itself; detect loops and cap re-entry.
- **Ordering per contact:** process one contact's events in order (partition or ordering key by contact), without serialising the whole tenant.
- **Build vs buy:** at this scale a custom engine on queues is normal; mention durable-execution engines (Temporal, Cloud Workflows) as an option for lower-volume, long-running flows and their cost per execution at billions a month.

**My evidence:** S2 (a business rules engine with an approval ladder), S1 (event-driven sync with ordering, retries and idempotency).

#### Design multi-tenant authorisation for an Agency → Sub-account → User hierarchy, including impersonation.

**Model:** tenants form a tree: **agency → sub-accounts (locations) → users**. Users have roles at a level (agency admin, agency user with access to some sub-accounts, sub-account admin, sub-account user) and **scoped permissions** (contacts: read/write; opportunities: own-only; settings: none).

**Design:**
- **Authorisation service / policy library:** one place answers `can(user, action, resource)`; resources carry `agency_id` and `sub_account_id`. RBAC for coarse roles plus attribute checks for "own records only" (assigned user) — that's ABAC. My S8 rule ("see records where I'm the assigned salesperson *or* consultant") is exactly this kind of attribute rule.
- **Tenant isolation in depth:** the tenant is resolved from the **token**, not from a client-supplied ID; a client-supplied sub-account ID is only a selector that must be checked against the user's memberships on every request. Enforce the tenant filter in the data access layer (shared repository code or database-level policies), so a forgotten check in a handler doesn't leak data.
- **Tokens:** short-lived access tokens with the user, the active sub-account and scopes; refresh tokens; permission changes take effect quickly (short TTL or a revocation list).
- **Impersonation** (agency support logs in as a sub-account user): an explicit, time-limited, permission-gated action; the token carries **both** identities (`actor` and `subject`); every audit log entry records both; sensitive actions (changing billing, exporting data) blocked or re-confirmed while impersonating; the impersonated user can see it in their activity log.
- **Audit:** append-only audit events for permission changes, logins, impersonation and data exports.
- **Performance:** permission checks happen on every request; cache the user's memberships and role permissions per token, with invalidation on change.
- **Migration:** moving from an old permission model to a new one: dual-evaluate (old and new) in shadow mode, log differences, then switch (my S1 and S7 approach to risky migrations).

Repo background: [RBAC vs ABAC vs ReBAC](../topics/08-cloud-devops-integration/ANSWERS.md#rbac-vs-abac-vs-rebac), [JWTs](../topics/08-cloud-devops-integration/ANSWERS.md#jwts--structure-validation-risks).

#### Design API keys, OAuth and scoped tokens for a public API and app marketplace.

- **Two kinds of clients:** (1) an agency's own integrations → **API keys** or private integration tokens scoped to an agency or sub-account; (2) **marketplace apps** used by many agencies → **OAuth 2.0 authorisation code** flow, where an agency installs the app and grants scopes for chosen sub-accounts.
- **Scopes** map to resources and actions (`contacts.readonly`, `contacts.write`, `opportunities.write`); tokens carry scopes plus the tenant they apply to; the API checks scope **and** tenant on every call.
- **API key handling:** show once, store only a hash (with a prefix for lookup and identification), allow multiple keys for rotation, expiry dates, last-used timestamps, instant revocation.
- **OAuth details:** PKCE, exact redirect URI matching, short-lived access tokens and rotating refresh tokens (detect reuse), per-installation tokens so uninstalling revokes them.
- **Rate limits** per app and per tenant, with clear headers and 429s; webhooks for app events.
- **Abuse and security:** anomaly detection on token use, secret scanning partnerships for leaked keys, audit logs visible to agency admins.

Repo background: [OAuth 2.0 flows](../topics/08-cloud-devops-integration/ANSWERS.md#oauth-20-flows-authorization-code--pkce-client-credentials-device-code), [API keys vs OAuth vs mTLS](../topics/08-cloud-devops-integration/ANSWERS.md#api-keys-vs-oauth-tokens-vs-mtls).

#### Design reliable outbound webhooks to customers' systems.

- **Flow:** domain event → webhook dispatcher → for each subscription, a delivery task (event ID, endpoint, payload) on a queue → workers POST with a timeout (e.g. 10 s) → record the attempt.
- **Retries** with exponential backoff and jitter over hours; then mark the delivery failed and alert the subscriber; auto-disable endpoints that fail for days.
- **At-least-once:** include a unique **event ID** so receivers can dedupe; document that duplicates and out-of-order delivery can happen; include a timestamp and sequence or version per object.
- **Security:** HMAC signature over timestamp + body with a per-subscription secret, replay window; HTTPS only; block private IP ranges and metadata endpoints (SSRF) when customers provide URLs.
- **Isolation:** per-endpoint concurrency limits and circuit breakers, so one slow customer endpoint can't back up everyone's deliveries (separate queues or fair scheduling).
- **Operability:** delivery logs and manual replay in the UI; metrics on success rate and latency per endpoint.

Repo background: [webhooks vs polling](../topics/08-cloud-devops-integration/ANSWERS.md#webhooks-vs-polling-vs-websockets-vs-server-sent-events).

#### Design a bulk import of one million contacts from a CSV.

1. **Upload** directly to object storage via a signed URL; create an import job record.
2. **Validate and preview:** parse the header, let the user map columns to fields (including custom fields), show sample rows and errors before starting.
3. **Process in chunks** (e.g. 1,000 rows) as parallel tasks; each chunk is idempotent (job ID + chunk number), so retries don't duplicate. Use the same normalisation and dedupe upsert path as the contact creation engine; batch writes to the database.
4. **Throttle per tenant** so one import doesn't starve interactive traffic; lower priority than real-time creates.
5. **Downstream effects:** decide whether imported contacts fire workflow triggers (usually opt-in, since a million triggers at once is a load spike); index into search in bulk.
6. **Progress and results:** progress bar from completed chunks; a downloadable error file with row numbers and reasons; a summary (created, updated, skipped, failed).
7. **Cancel and undo:** cancel stops remaining chunks; tagging imported contacts with the import ID makes undo possible.

This is the same "backfill as a separate, re-runnable, idempotent flow" design as S1.

#### Design the opportunities (deals and pipeline) service, including stage history and reporting.

- **Model:** pipeline (ordered stages, per sub-account) → opportunity (contact, value, currency, status: open/won/lost/abandoned, stage, owner, custom fields). **Stage changes as events** in an append-only history table (`opportunity_id, from_stage, to_stage, changed_by, at`), not just the current stage.
- **Writes:** optimistic concurrency (version field) for drag-and-drop updates from several users; validation of stage belonging to the pipeline; triggers for workflows (stage changed, status won).
- **Reporting:** conversion rates between stages, time in stage, forecast (value × stage probability), won revenue by owner. Compute from the history events in an analytics store (ClickHouse) rather than querying the transactional store; handle backdated changes.
- **Access:** "own opportunities only" users (ABAC on owner), like S8.
- **Currency:** store amount in minor units with currency; report in the sub-account's currency with a defined conversion date (an open question I raised in S2).
- **Scale:** board views need fast "opportunities in stage X" queries: compound index on (sub_account, pipeline, stage, updated_at); paginate per column.

My evidence: the order and subscription data model and discount approval ladder (S2) are the same kind of commercial state machine.

### C. Backend deep dives

#### Explain the Node.js event loop. What blocks it, and how do you find and fix that in production?

- Node runs JavaScript on **one main thread**. I/O (network, most file operations via libuv's thread pool) is asynchronous; when it completes, callbacks are queued and the **event loop** runs them.
- **Phases** each loop iteration: timers (`setTimeout`/`setInterval` callbacks) → pending callbacks → poll (wait for and run I/O callbacks) → check (`setImmediate`) → close callbacks. Between callbacks, Node drains the **microtask queues**: `process.nextTick` first, then promise callbacks.
- **What blocks it:** any long synchronous work on the main thread: big `JSON.parse`/`JSON.stringify`, synchronous crypto or compression, regex with catastrophic backtracking, sorting huge arrays, `fs.*Sync` calls, tight loops; also unbounded recursive `process.nextTick` or promise chains that starve I/O. While blocked, **every** request on that process waits.
- **Find it:** event-loop lag / delay metrics (`perf_hooks.monitorEventLoopDelay`, APM tools), p99 latency spikes across unrelated endpoints, CPU profiles (`--cpu-prof`, clinic.js flame graphs), `--trace-sync-io` in testing.
- **Fix it:** stream and paginate big payloads; move CPU work to worker threads or a separate service (next question); chunk work with `setImmediate`; use async APIs; fix pathological regexes; limit payload sizes.

Read: [Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop).

#### How do you run CPU-heavy or long work in Node.js without hurting API latency?

- **Don't do it in the request path.** Accept the request, enqueue a job (Pub/Sub, Cloud Tasks, BullMQ on Redis), return `202` with a job ID.
- **Worker processes** consume the queue, separately deployed and autoscaled on queue depth, so API pods stay responsive.
- Inside a process, **worker threads** (`worker_threads`, often via a pool such as Piscina) for CPU-bound tasks (image processing, PDF generation, heavy parsing).
- Use the right language for heavy compute: a Go service, or a native module.
- **Graceful shutdown:** on SIGTERM (Kubernetes rolling deploys), stop taking new jobs, finish or release in-flight ones (nack so they're redelivered), then exit. Idempotent jobs make redelivery safe.
- **Timeouts and cancellation** with `AbortController` for downstream calls.

#### Node.js vs. Go for a backend service: when would you choose each?

| | Node.js (TypeScript) | Go |
|---|---|---|
| Concurrency | Single-threaded event loop; excellent for I/O-bound work; CPU work needs workers | Goroutines across all cores; good for both I/O and CPU-bound work |
| Performance | Good; GC and single thread limit CPU-heavy services | Lower latency and memory per request; predictable performance |
| Ecosystem | Huge (npm), shared code and types with the frontend, fast to build product features | Strong standard library for networking; fewer frameworks, more explicit code |
| Typical use at a CRM company | Product APIs, BFFs, integrations, most CRUD and workflow orchestration | High-throughput pipelines, proxies, schedulers, messaging and metering hot paths |

**What to say:** "I'd keep product services in TypeScript for speed of development and team familiarity, and use Go where profiles show CPU or memory is the bottleneck: high-throughput event processing, the metering hot path, or a dispatcher handling tens of thousands of requests per second."

#### Pub/Sub vs. Cloud Tasks vs. a Redis queue: when do you use each, and what delivery guarantees do you get?

| | **GCP Pub/Sub** | **Cloud Tasks** | **Redis queue (e.g. BullMQ, Redis Streams)** |
|---|---|---|---|
| Model | Publish once, **many subscribers** (fan-out); push or pull | **Explicit task** sent to one HTTP handler (or App Engine), with retries | In-memory queue/stream you operate |
| Scheduling | No per-message delay (use another mechanism) | **Schedule a task for a future time**; per-queue rate and concurrency limits | Delayed jobs supported (sorted sets) |
| Delivery | **At least once** by default; optional **exactly-once delivery** for pull subscriptions (within a region) *(verify limits)*; **ordering keys** for per-key order; dead-letter topics; retention for replay | At least once; task names deduplicate creates *(verify window)* | At least once with acknowledgements (Streams consumer groups); durability depends on Redis persistence and replication |
| Use for | Domain events consumed by several services (contact updated → search, workflows, analytics) | "Do this job at time T" (workflow waits), throttled calls to a rate-limited API | Low-latency internal jobs, short-lived queues, where a Redis outage is acceptable |

**What to say:** in every case consumers must be **idempotent**, because redelivery happens; and "exactly-once delivery" in Pub/Sub doesn't make your side effects exactly-once (see section D). SQS comparisons: [16-aws, section E](../topics/16-aws/README.md#e-application-integration).

#### How do you keep Elasticsearch in sync with the primary database, and how do you reindex with zero downtime?

**Sync:**
- Emit change events from the primary store **after commit**: an **outbox table** written in the same transaction, or change streams / CDC (MongoDB change streams, Debezium for MySQL/Postgres). Avoid dual writes from the API (one write can fail and they drift).
- Indexer consumers update ES with **versioning** (use the source record's version or updated-at as an external version) so out-of-order events don't overwrite newer data.
- Bulk API for throughput; retries with backoff; dead-letter for poison documents.
- **Reconciliation job** that samples or fully compares counts and versions per tenant and repairs drift (S4 lesson: build it from day one).

**Zero-downtime reindex** (mapping change, new analyser):
1. Queries and writes go through **aliases** (`contacts_read`, `contacts_write`), never index names.
2. Create the new index with the new mapping.
3. Start **dual-writing** live changes to both indexes (or record the change-stream position).
4. Backfill the new index from the source of truth (or the `_reindex` API), throttled.
5. Replay changes since the backfill started; verify counts and sample queries.
6. Atomically switch the read alias; keep the old index for rollback; then stop writing to it and delete later.

#### How would you model contacts with custom fields and tags in MongoDB, and which indexes would you create?

```js
// contacts collection, one document per contact
{
  _id, subAccountId, agencyId,
  firstName, lastName, email, emailNorm, phone, phoneE164,
  tags: ["lead", "webinar-2026"],              // multikey index
  customFields: [ { fieldId: "cf_123", value: "Gold" }, ... ],   // attribute pattern
  source, assignedUserId, createdAt, updatedAt, version, deletedAt
}
```

- **Attribute pattern** for custom fields (array of `{fieldId, value}`) rather than one top-level key per field: the number of distinct custom fields across tenants is unbounded, and this allows one compound index.
- **Indexes** (every one starts with the tenant key):
  - `{subAccountId: 1, emailNorm: 1}` unique (partial: where `emailNorm` exists), same for `phoneE164`, if dedupe is by identifier; otherwise a separate key collection.
  - `{subAccountId: 1, updatedAt: -1}` for recent lists and sync.
  - `{subAccountId: 1, tags: 1}` for tag filters.
  - `{subAccountId: 1, "customFields.fieldId": 1, "customFields.value": 1}` for custom field lookups (complex filtering belongs in Elasticsearch).
- **Don't embed unbounded arrays** like all notes or messages in the contact document (16 MB document limit, write amplification); keep them in their own collections referencing `contactId`.
- **Sharding:** shard key on `subAccountId` (plus a hashed component for very large tenants to avoid jumbo chunks and hot shards).
- **Soft delete and version** fields support sync, audit and optimistic concurrency.

#### One large agency is overloading a shared service (a noisy neighbour). What do you do?

1. **Stop the bleeding:** per-tenant rate limits or concurrency caps on the affected path; shed or delay that tenant's lowest-priority work (bulk jobs before interactive requests). Communicate with the account team.
2. **Find out why:** is it legitimate growth, a misconfigured workflow loop, an integration hammering the API, or abuse?
3. **Structural fixes:**
   - **Fair scheduling:** per-tenant queues or weighted fair queuing so one tenant's backlog can't delay others.
   - **Quotas** by plan, visible to customers, with 429s and `Retry-After`.
   - **Isolation for the largest tenants:** dedicated partitions, shards, indexes or worker pools ("cells"), routed by tenant.
   - **Bulkheads:** separate pools for interactive and batch traffic.
4. **Observability by tenant:** usage, latency and errors broken down per tenant, with alerts on sudden changes.
5. **Product follow-up:** if a legitimate use case causes it, design a supported path (bulk API, batch endpoints).

#### How do you use Redis for distributed locks, and when are they unsafe?

- **Basic lock:** `SET lock:key <random token> NX PX 30000`; release only if the value still matches your token (a Lua script compares and deletes atomically). The TTL frees the lock if the holder crashes.
- **Why it's unsafe for correctness:** a process can pause (GC, CPU starvation, network delay) past the TTL; the lock expires, another process takes it, and both now think they hold it. Failover between Redis primary and replica can also lose a lock. Redlock (multiple Redis nodes) is debated for the same timing reasons.
- **What to do instead when correctness matters:** use the database's guarantees: **unique constraints**, conditional updates (`UPDATE ... WHERE version = n`), or transactions; or **fencing tokens** (a monotonically increasing number from the lock service that the protected resource checks, rejecting older tokens).
- **Redis locks are fine for efficiency:** preventing duplicate work (two workers rebuilding the same cache), not for preventing double charges.

In an interview: "For billing or dedupe I rely on a unique constraint or a conditional write, and use Redis locks only to reduce wasted work."

#### How would you migrate contacts from one datastore to another with zero downtime?

My S1 pattern, applied:
1. **Dual-write or change capture:** live changes flow to the new store (via CDC or an outbox consumer) while the old store stays the source of truth.
2. **Backfill** history from a snapshot, re-runnable and idempotent (upsert by ID with version checks so a backfill never overwrites a newer live change).
3. **Reconcile continuously:** counts and checksums per tenant, sampled field-by-field comparison, drift reports. Fix the pipeline, not just the data.
4. **Shadow reads:** read from both, compare results, serve from the old; measure mismatch rate and latency.
5. **Cut over per tenant** (small tenants first, a few big ones last), behind a flag; keep reverse sync to the old store so you can roll back.
6. **Decommission** after a stable period with zero drift.

Key line: "I separate the one-time backfill from the continuous sync. People keep treating them as one problem, and that's where migrations go wrong."

### D. Correctness and billing

#### How do you guarantee a customer is never double-charged or under-charged?

- **Idempotency end to end:** every usage event has a stable ID from its source; ingestion dedupes on it with a **unique constraint**; every charge carries an idempotency key; retries reuse the key.
- **One write path for money:** all balance changes go through the ledger service as balanced, append-only entries in integer minor units; no service updates balances directly.
- **State machine for each usage item:** received → rated → reserved/committed → invoiced; illegal transitions are rejected; each transition is idempotent.
- **Atomicity:** "mark rated + post ledger entry + emit event" in one transaction plus an outbox, so a crash can't leave half-applied state.
- **Reconciliation** (detects what the above misses): our usage vs the provider's records vs ledger vs invoices, daily, with discrepancies queued for investigation and alerting on value thresholds.
- **Under-charging** (lost usage) is caught by completeness checks: every provider record must map to a rated usage record; holds that never commit are swept.
- **Testing:** property tests for rating, replaying production event logs in staging, chaos tests that kill workers mid-transaction.

#### Explain the reserve/commit (authorise/capture) pattern for prepaid usage balances.

When a customer has a prepaid wallet and the final cost isn't known up front (a call's duration):

1. **Reserve (hold):** before allowing the call, place a hold for an estimated amount: `available = balance − active holds`; if `available < estimate`, deny. Implemented as one atomic conditional operation (a database transaction with a row lock on the wallet, or a conditional update), so two simultaneous calls can't both use the last credit.
2. **Extend:** for long calls, periodically increase the hold, or cut the call when it can't be extended.
3. **Commit:** when the final usage is rated, convert the hold into a charge for the actual amount (ledger entries) and **release** the remainder. Idempotent on the usage ID.
4. **Release / expire:** if the call never happens, release the hold; holds older than a limit are swept and resolved by querying the provider.

**Design points:** holds are records with IDs and states (`active, committed, released, expired`), not just a number; the wallet balance is derived from ledger entries; per-wallet serialisation (a single writer or row lock) to avoid races; very hot wallets (big agencies) may use sub-wallets or pre-allocated budget slices.

This is the same as card payments: authorise (hold) then capture (charge).

#### How do you design reconciliation between your usage records, a carrier's invoice and customer charges?

- **Three-way match:** (1) the provider's usage records or invoice lines, (2) our rated usage records, (3) our ledger charges to customers.
- **Keys:** the provider's record ID (call SID, message ID) is the join key; normalise time zones and currencies.
- **Checks:** missing on either side (lost or phantom usage), duplicates, amount differences beyond rounding tolerance (rate card errors), timing differences (an event in our month but the provider's next month).
- **Process:** a daily job writes a **discrepancy table** (type, amount, records); auto-resolve known patterns (timing); queue the rest for an owner; alert when value or count crosses a threshold; trend the mismatch rate.
- **Margin view:** provider cost vs customer revenue per product, per agency, which also catches pricing mistakes.
- **Close process:** month-end close only after discrepancies are resolved or explained; corrections posted as adjustment entries.

My S4 story is the lesson: "We built reconciliation after the first drift; I'd now build it before the first event flows."

#### "Exactly-once processing": is it possible, and what do you actually build?

- **Exactly-once delivery** across a network isn't achievable in general: a sender can't distinguish "message lost" from "acknowledgement lost", so it must retry, which can duplicate. Systems that advertise exactly-once (Kafka transactions, Pub/Sub exactly-once delivery) give it **within their own boundary**.
- What you build is **effectively-once**: **at-least-once delivery + idempotent processing**.
  - Dedupe on a stable message or business ID (unique constraint, or a processed-IDs table written in the same transaction as the effect).
  - Make the effect itself idempotent (upsert, conditional update, idempotency keys on external calls like sending an SMS or charging a card).
  - Use the outbox pattern so "state change + emitted event" can't diverge.
- For external side effects that don't support idempotency, record intent before the call and check status after a crash before retrying.

### E. Staff-level leadership and behavioural

#### Tell me about a system you owned through a real production failure.

Use **S4** (sync drift during the live migration): what drifted and how it was noticed [I-05], how I traced the cause, how I fixed the affected records, and the reconciliation and drift reporting I built afterwards. Or **[I-26]** if you have a bigger incident. Staff-level emphasis: what you changed in the **system and process** so the class of failure can't recur, and how you communicated during it.

#### Tell me about setting technical direction across several teams.

Use **S1** plus the objectives line from [PROFILE.md](PROFILE.md): "Led service boundary decisions, contract-first integrations, and migration design patterns (ID mapping, retries, monitoring, rollback)." Show: the decision, how you got agreement (written specs, reviews), how other engineers executed without you (seven specifications), and the outcome. Add **S12** (phased rollout across two national tracks) for cross-team dependency planning.

#### Tell me about a large migration you led on a live system.

**S1** (legacy MySQL estate to Postgres services across six product lines with zero downtime), with **S7** (moving every user off a PHP 5.3 stack) as a second example. Structure from section C's migration answer: backfill separate from sync, idempotency guard, ordering, reconciliation, rollback. [I-01, I-08 for numbers.]

#### Tell me about a time you disagreed with another senior engineer on architecture.

Use **[I-24]** once filled in. If you don't have one yet, S5 (buy vs build: arguing against self-hosting with a break-even analysis) shows the method: make the disagreement about evidence and reversibility, write the options down, agree on the decision criteria first, and commit fully once decided.

#### How do you raise the engineering bar of the teams around you?

- **Written standards and examples:** specifications others implement from (S1), code review and QA walkthrough standards (S9).
- **Onboarding** plans that make new engineers productive fast (S9).
- **Patterns, not one-offs:** turn a solved problem into a reusable approach (migration patterns: ID mapping, retries, monitoring, rollback).
- **Coaching** on prioritisation and design (S9) [I-10].
- **Make quality cheap:** idempotent, re-runnable jobs, guard conditions written first, feature flags; known debt written down with an owner.

#### How do you decide what to build versus buy (for example, a workflow engine or a billing system)?

Use **S5**'s framework: total cost including engineering time; whether it's core differentiation (at HighLevel, Workflows and billing correctness are core, so build); volume (managed per-execution pricing at billions of actions a month is very expensive); data-protection and control constraints; and reversibility (put a thin adapter in front of anything bought). "For HighLevel's Workflows engine I'd build on queues and a scheduler; for a commodity part like tax calculation or email delivery I'd buy behind an adapter."

### F. Coding

#### Implement a per-tenant rate limiter in TypeScript (token bucket).

```typescript
type Bucket = { tokens: number; updatedAt: number };

export class TenantRateLimiter {
  private buckets = new Map<string, Bucket>();

  constructor(
    private capacity: number,          // max burst
    private refillPerSecond: number,   // sustained rate
    private now: () => number = () => Date.now(),
  ) {}

  tryAcquire(tenantId: string, cost = 1): boolean {
    const t = this.now();
    const b = this.buckets.get(tenantId) ?? { tokens: this.capacity, updatedAt: t };
    const elapsed = (t - b.updatedAt) / 1000;
    b.tokens = Math.min(this.capacity, b.tokens + elapsed * this.refillPerSecond);
    b.updatedAt = t;
    const allowed = b.tokens >= cost;
    if (allowed) b.tokens -= cost;
    this.buckets.set(tenantId, b);
    return allowed;
  }
}

// usage / tests
let clock = 0;
const rl = new TenantRateLimiter(2, 1, () => clock);
console.assert(rl.tryAcquire("a") && rl.tryAcquire("a"), "burst of 2 allowed");
console.assert(!rl.tryAcquire("a"), "third request blocked");
console.assert(rl.tryAcquire("b"), "other tenant unaffected");
clock = 1000;
console.assert(rl.tryAcquire("a"), "refilled after 1s");
```

**Talk through:** injectable clock for tests; per-tenant isolation; memory growth (evict idle buckets with an LRU or TTL). **Distributed version:** the same logic in a Redis Lua script keyed by tenant (atomic read-refill-decrement), or a sliding-window counter; return `Retry-After`. Different limits per plan come from configuration.

#### Implement a retry helper with exponential backoff, jitter and an idempotency key.

```typescript
import { randomUUID } from "node:crypto";

type RetryOpts = { retries: number; baseMs: number; maxMs: number;
                   isRetryable?: (e: unknown) => boolean };

const sleep = (ms: number) => new Promise((r) => setTimeout(r, ms));

export async function withRetry<T>(
  fn: (idempotencyKey: string, attempt: number) => Promise<T>,
  { retries, baseMs, maxMs, isRetryable = () => true }: RetryOpts,
): Promise<T> {
  const key = randomUUID();                       // same key on every attempt
  for (let attempt = 0; ; attempt++) {
    try {
      return await fn(key, attempt);
    } catch (e) {
      if (attempt >= retries || !isRetryable(e)) throw e;
      const cap = Math.min(maxMs, baseMs * 2 ** attempt);
      await sleep(Math.random() * cap);           // "full jitter"
    }
  }
}

// usage / test: fails twice, then succeeds, with the same key each time
(async () => {
  const keys: string[] = [];
  const result = await withRetry(async (key, attempt) => {
    keys.push(key);
    if (attempt < 2) throw new Error("503");
    return "ok";
  }, { retries: 5, baseMs: 10, maxMs: 100 });
  console.assert(result === "ok" && new Set(keys).size === 1 && keys.length === 3);
})();
```

**Talk through:** the idempotency key is created **once** and reused, so the server can dedupe; retry only retryable errors (timeouts, 429, 5xx; not 400 or 409); respect `Retry-After`; full jitter avoids synchronized retry storms; cap total time; in production add a circuit breaker and metrics. Repo: [flaky third-party API](../topics/08-cloud-devops-integration/ANSWERS.md#how-do-you-handle-a-flaky-third-party-api-retries-backoff-circuit-breakers-timeouts).

#### Implement contact deduplication: group records that share an email or phone number.

Union-find over records, joined through shared normalised identifiers (transitively: A shares an email with B, B shares a phone with C → one group).

```typescript
type Contact = { id: string; email?: string; phone?: string };

export function groupDuplicates(contacts: Contact[]): string[][] {
  const parent = new Map<string, string>();
  const find = (x: string): string => {
    while (parent.get(x) !== x) {
      parent.set(x, parent.get(parent.get(x)!)!);   // path halving
      x = parent.get(x)!;
    }
    return x;
  };
  const union = (a: string, b: string) => { parent.set(find(a), find(b)); };

  const firstWithKey = new Map<string, string>();
  for (const c of contacts) {
    parent.set(c.id, c.id);
    const keys = [
      c.email && "e:" + c.email.trim().toLowerCase(),
      c.phone && "p:" + c.phone.replace(/\D/g, ""),
    ].filter(Boolean) as string[];
    for (const k of keys) {
      const seen = firstWithKey.get(k);
      if (seen) union(c.id, seen); else firstWithKey.set(k, c.id);
    }
  }
  const groups = new Map<string, string[]>();
  for (const c of contacts) {
    const r = find(c.id);
    groups.set(r, [...(groups.get(r) ?? []), c.id]);
  }
  return [...groups.values()];
}

// test
const g = groupDuplicates([
  { id: "1", email: "A@x.com" },
  { id: "2", email: "a@x.com ", phone: "+91 98765-43210" },
  { id: "3", phone: "919876543210" },
  { id: "4", email: "other@x.com" },
]);
console.assert(g.length === 2 && g.some((grp) => grp.length === 3));
```

**Talk through:** normalisation decides quality (here simplified: real phone normalisation should parse to E.164 with a default country, e.g. with libphonenumber); O(n α(n)) time; transitive merges can over-merge (a shared office phone links unrelated people), so production systems add rules (ignore very common identifiers, require two matches) and a human review step; at scale, block by identifier in the database or a batch job. Repo: [entity resolution design](../topics/03-system-design/ANSWERS.md#design-a-system-to-unify-customer-records-from-10-systems-entity-resolution--master-data).

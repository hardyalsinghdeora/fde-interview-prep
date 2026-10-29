# System Design (Senior / Principal)

At senior and principal level the system design round stops being "draw boxes for a URL shortener" and becomes a test of judgement: which requirements actually drive the design, what the data model must guarantee, where it breaks, how you would migrate to it, and what it costs. Principal/Staff Engineer, Software Architect and Agentic-AI Architect loops weight this round most heavily; Engineering Manager and Forward Deployed Engineer loops use it to check you can still go deep.

> Related: [System design prompts](../topics/03-system-design/README.md) · [Decomposition](../topics/04-decomposition/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [AI / LLM](../topics/06-ai-llm/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Walk me through how you run a 45-minute design interview: what you do in the first 5 minutes and what you deliberately leave until the end.
- [ ] Which non-functional requirements do you always pin down, and how do you turn "it must be fast and reliable" into numbers (latency percentiles, availability, RPO/RTO, durability)?
- [ ] Do a back-of-envelope estimate out loud: 2M orders a day, peak 10x average — what are write QPS, rows per year, and storage after three years with indexes?
- [ ] Why do you design the data model before the services? What goes wrong when you start from boxes?
- [ ] Strong, read-your-writes, monotonic and eventual consistency: give one place in a commerce system where each is the right choice.
- [ ] Cache-aside, read-through, write-through and write-behind: which would you never use for account balances, and why?
- [ ] Queue vs. log vs. pub/sub: what does each guarantee about ordering, replay and fan-out?
- [ ] At-least-once delivery plus idempotent consumers vs. "exactly-once": what can actually be guaranteed end to end?

## B. Working depth
- [ ] Model the core entities of a subscription billing system (customer, plan, price, subscription, invoice, line item, payment, credit note). Which tables are append-only and why?
- [ ] How do you store money, currencies, tax and rounding so that an invoice re-rendered in two years gives the same total?
- [ ] Design the API surface for creating an order that triggers payment, inventory reservation and fulfilment. Which calls are synchronous and which are events?
- [ ] Where do you put idempotency keys in that flow, how long do you keep them, and what do you return on a duplicate with a different payload?
- [ ] Explain the transactional outbox pattern and when you would choose it over publishing directly after commit.
- [ ] How do you set timeouts, retries and backoff across three service hops so retries do not multiply into a retry storm?
- [ ] How do you invalidate a cache for a product catalogue that changes thousands of prices at once, without a thundering herd?
- [ ] What dead-letter queue policy do you set, who owns the DLQ, and how is a message safely replayed?
- [ ] How do you design for a hot key or hot tenant (one enterprise customer with 40% of traffic)?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] A single primary database is the bottleneck. Walk through the options in order — query and index work, read replicas, caching, vertical scaling, partitioning, sharding — and the cost and risk of each step.
- [ ] Choose a shard key for a multi-tenant billing platform. What are the cross-shard queries you will regret, and how do you rebalance later?
- [ ] Your design is eventually consistent. What does the user see during the inconsistency window, and how do you bound and measure it?
- [ ] What are the failure modes of the outbox relay, the consumer and the downstream service? Which of them lose data, duplicate it or reorder it?
- [ ] How do you design graceful degradation: which features switch off first when the payment provider or the LLM provider is slow?
- [ ] How do you evolve the data model of a live system: expand/contract, dual reads, backfill, and how the design anticipates that from day one?
- [ ] Estimate the monthly cloud cost of your design at launch and at 10x. Which component dominates, and what design change halves it?
- [ ] Build vs. buy for billing, search, queues or an LLM gateway — how do you argue it to a CTO, including exit cost?
- [ ] How do you design multi-region for a system that must not double-charge? Active-active vs. active-passive, and where the money ledger lives.
- [ ] What would you write in an architecture decision record for this design, and which decisions are reversible vs. one-way doors?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design an order-to-cash platform: quote, order, invoice, payment, dunning and revenue reporting, with a full audit trail.
- [ ] Design usage-based metering for an API or AI product: ingest billions of usage events, aggregate per tenant per hour, and bill accurately despite late and duplicate events.
- [ ] Design a double-entry ledger service used by several product teams. How do you guarantee balances never drift?
- [ ] Design flash-sale checkout for limited stock: no overselling, fair queueing, and a payment step that can fail.
- [ ] Design a multi-tenant LLM agent platform: tool calling, per-tenant credentials, rate limits, cost attribution, audit and human approval for risky actions.
- [ ] Design a hybrid search (keyword + vector) service over a product catalogue or knowledge base with per-tenant permissions and freshness under a minute.
- [ ] Design the reconciliation system that compares your payments table with the payment provider's settlement files every night.
- [ ] Redesign a monolith's reporting queries that are slowing the primary database: what moves to replicas, a warehouse or CDC, and in what order?

## E. Hands-on checks (things to be able to DO)
- [ ] Produce a one-page estimate sheet (QPS, storage, bandwidth, cache size, cost) for a given prompt in under 10 minutes.
- [ ] Write the DDL for the core tables of your design, including keys, constraints and the indexes for the top three queries.
- [ ] Write a sequence diagram for a payment flow including timeout, retry and duplicate-webhook paths.
- [ ] Write an ADR for "outbox + queue vs. direct publish" with context, options, decision and consequences.
- [ ] Sketch the dashboards and alerts (SLIs, SLO, burn-rate alert) you would ship with the design.

---

**Good answers show:**
- Requirements and numbers drive every choice; the candidate says which requirement would change the design if it changed.
- The data model and its invariants (uniqueness, immutability, money correctness) come before services and technology names.
- Failure handling is designed, not bolted on: idempotency, retries, ordering and reconciliation are explicit.
- A migration and evolution path from today's system, not only a greenfield end state.
- Cost and operational load are estimated, and simpler options are preferred until the numbers force otherwise.
- Clear trade-off language: what is given up, how it is measured, and when to revisit.

**Prove it:** Build a small usage-metering service (Postgres + a queue) that ingests duplicate and late events, aggregates per tenant per hour, produces invoices and runs a reconciliation check — then write a two-page design doc with estimates, an ADR and a load-test result.

# Data Migration & Legacy Modernisation

Almost every company has a legacy system it cannot switch off, so engineers who have moved live data and traffic without downtime or data loss are in demand — and interviewers probe hard for real detail, because this is where confident hand-waving fails. Principal/Staff Engineer, Software Architect and Forward Deployed Engineer loops ask most; Engineering Manager loops focus on risk, sequencing and stakeholder communication.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [System design](../topics/03-system-design/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md) · [Cloud, DevOps & integration](../topics/08-cloud-devops-integration/README.md) · [Behavioural](../topics/10-behavioral/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is the strangler fig pattern, and what are the concrete steps to apply it to a monolith?
- [ ] Big-bang cutover vs. incremental migration: when is big-bang actually the right call?
- [ ] Dual writes vs. change data capture (CDC): how each works and the classic way dual writes lose or corrupt data.
- [ ] What is a backfill, and why must it be idempotent and restartable?
- [ ] Expand/contract (parallel change) for schema migrations: describe the phases for renaming a column on a live table.
- [ ] What does "source of truth" mean during a migration, and how do you make the switch of source of truth explicit?
- [ ] What is reconciliation, and what is the difference between count checks, checksums and field-level comparison?

## B. Working depth
- [ ] How does log-based CDC work for PostgreSQL or MySQL (logical decoding, binlog), and what operational risks does it bring (replication slots filling disk, schema changes, snapshot consistency) *(verify for your CDC tool)*?
- [ ] Ordering and duplicates in CDC streams: how do you apply changes idempotently and in the right order per entity?
- [ ] Backfill design: batch size, keyset iteration, throttling against primary load and replica lag, checkpointing and resume.
- [ ] Reconciling a backfill that runs while live writes continue: how do you avoid overwriting newer data with older data?
- [ ] Reverse sync: why you keep the old system updated from the new one after cutover, and how you prevent sync loops.
- [ ] ID mapping between old and new systems: mapping tables, preserving legacy IDs, and handling merges or splits of records.
- [ ] Shadow traffic and dark reads: comparing responses from old and new paths before switching, and dealing with expected differences.
- [ ] Feature flags and routing for gradual cutover by tenant, region or percentage.
- [ ] Data quality problems in legacy data (duplicates, orphans, invalid states, encoding issues): find them early, decide fix vs. carry over.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you define "done" and the success criteria for a migration, and which metrics do you put on the dashboard during cutover?
- [ ] Design the rollback plan: up to which point can you roll back cleanly, what is the point of no return, and how do you make it later?
- [ ] Dual-running two systems is expensive. How do you decide how long to run in parallel and justify the cost?
- [ ] Migrating financial or billing data: invariants that must hold (totals, balances, invoice numbering, tax), and how you prove them to finance and auditors.
- [ ] Zero-downtime schema changes on large tables: online schema-change tools vs. native operations, lock risks, and replica impact *(verify tool behaviour)*.
- [ ] Changing the data model during the migration (not just copying): transform logic, versioned mappings, and testing transformations with production-like data.
- [ ] Risk management: how you break a migration into reversible steps, run rehearsals, and decide go/no-go.
- [ ] Organisational side: getting other teams to move off the old system, handling consumers you did not know existed, and communicating timelines.
- [ ] When should you stop modernising and leave a legacy component as it is, wrapped behind an interface?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Move a live order and billing database from a self-managed MySQL to managed PostgreSQL with zero downtime. Walk through every phase and the rollback at each.
- [ ] A monolith owns customers, orders and invoices in one database. Extract invoicing into its own service and database without stopping invoicing.
- [ ] Mid-cutover, reconciliation shows 0.3% of records differ between old and new systems. What do you do in the next hour, the next day and before continuing?
- [ ] A legacy system only exports nightly CSV files over SFTP. Design a migration and sync path to a modern platform.
- [ ] Migrate 300 tenants from per-tenant databases into a shared multi-tenant schema, a few tenants at a time, with per-tenant rollback.
- [ ] After cutover, a downstream report breaks because it read directly from the old database. How should this have been caught, and how do you fix it now?
- [ ] Plan the migration of a 5 TB table with continuous writes to a partitioned layout.

## E. Hands-on checks (things to be able to DO)
- [ ] Write an idempotent, resumable backfill script with keyset batching, throttling and a checkpoint table.
- [ ] Write a reconciliation query or job that compares two tables by key and reports missing, extra and differing rows.
- [ ] Write the expand/contract migration steps (SQL plus application changes) for renaming a heavily used column.
- [ ] Write a cutover runbook: preconditions, step-by-step actions, verification checks, go/no-go owners and rollback steps.

---

**Good answers show:**
- Specific mechanics — CDC positions, batch sizes, lock levels, checkpoint tables — rather than pattern names alone.
- Reconciliation as a first-class part of the plan, with defined tolerances and owners for mismatches.
- Every step reversible until a clearly stated point of no return, with rehearsed rollback.
- Data correctness invariants (especially money) stated up front and proven, not assumed.
- Honest stories about what went wrong in past migrations and what changed in the approach afterwards.
- The people side: consumers discovered, teams persuaded, timelines communicated.

**Prove it:** Migrate a sample orders schema from MySQL to PostgreSQL locally using CDC plus a backfill, run reverse sync, inject a few mismatches and catch them with a reconciliation job, then perform a scripted cutover and rollback — and write the runbook you used.

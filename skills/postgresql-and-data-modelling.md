# PostgreSQL & Data Modelling

Most backend systems live or die by their database, so senior interviews probe whether you understand what PostgreSQL is actually doing — plans, MVCC, locks, vacuum — and whether your schemas hold up under load and change. Principal/Staff Engineer and Software Architect loops go deepest here; Forward Deployed Engineer loops ask it because customer data almost always ends up in Postgres or something like it.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [System design](../topics/03-system-design/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is a B-tree index good for, and which queries can it not help with? When would you use a composite index and how does column order matter?
- [ ] What are GIN, GiST, BRIN and hash indexes each for? Give one real use for each.
- [ ] How do you index a `jsonb` column: `jsonb_ops` vs. `jsonb_path_ops`, expression indexes on a single key, and when to promote a key to a real column?
- [ ] Explain MVCC in PostgreSQL: what `xmin`/`xmax` are, why an UPDATE creates a new row version, and what a dead tuple is.
- [ ] What do Read Committed, Repeatable Read and Serializable mean in PostgreSQL specifically, and which anomalies does each allow?
- [ ] What are partial indexes and covering indexes (`INCLUDE`), and when does an index-only scan actually happen?
- [ ] Normalisation to third normal form vs. deliberate denormalisation: when do you denormalise and how do you keep it correct?
- [ ] Natural keys vs. surrogate keys; `bigint` identity vs. UUID (v4 vs. time-ordered v7) as primary keys — effects on index size and locality.

## B. Working depth
- [ ] Read this `EXPLAIN (ANALYZE, BUFFERS)` output: where is the time going, why did the planner choose a sequential scan, and what is the row-estimate error telling you?
- [ ] Nested loop vs. hash join vs. merge join: when does the planner choose each, and how do stale statistics make it choose wrongly?
- [ ] What do `autovacuum`, `ANALYZE` and `VACUUM FULL` each do? Why can a table bloat even with autovacuum on?
- [ ] What is transaction ID wraparound, how do you monitor for it, and what does an anti-wraparound vacuum do to a busy table?
- [ ] Row-level locks: `FOR UPDATE`, `FOR NO KEY UPDATE`, `FOR SHARE`, `SKIP LOCKED`. Build a job queue on a table with `SKIP LOCKED` and name its limits.
- [ ] Which DDL statements take an `ACCESS EXCLUSIVE` lock, and why can a quick `ALTER TABLE` block all traffic behind a long-running query? What does `lock_timeout` do for you?
- [ ] How do you diagnose a deadlock from the logs, and how do you design writes to avoid them?
- [ ] Declarative partitioning: range vs. list vs. hash, partition pruning, and what happens to unique constraints and foreign keys across partitions.
- [ ] Why does PostgreSQL need a connection pooler? PgBouncer session vs. transaction vs. statement pooling, and what breaks in transaction mode (prepared statements, `SET`, advisory locks) *(verify for your PgBouncer version)*.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Streaming (physical) replication vs. logical replication: what each can and cannot do, and how replica lag shows up as user-visible bugs.
- [ ] Synchronous vs. asynchronous replication: what you lose on failover with each, and how managed services (RDS, Aurora, Cloud SQL) change the picture *(verify)*.
- [ ] Multi-tenancy: shared tables with `tenant_id`, schema per tenant, database per tenant. Compare isolation, migrations, noisy neighbours, backups and cost at 50 vs. 50,000 tenants.
- [ ] Row-level security for tenant isolation: how it works, how you set the tenant in a pooled connection safely, and the performance cost.
- [ ] Zero-downtime schema changes: adding a NOT NULL column, renaming a column, changing a type, and adding a foreign key (`NOT VALID` then `VALIDATE`). Walk through each.
- [ ] `CREATE INDEX CONCURRENTLY`: what it costs, how it can fail and leave an invalid index, and how you recover.
- [ ] A table has grown to 2 billion rows. When do you partition, when do you archive, and how do you move to partitions on a live system?
- [ ] Serializable isolation vs. explicit locking vs. optimistic version columns for preventing lost updates on balances — trade-offs and retry handling.
- [ ] When would you move a workload off PostgreSQL (to a warehouse, a search engine, a key-value store or a vector store) and when is `pgvector` or full-text search good enough *(verify current pgvector index options)*?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] p99 latency on the orders API doubled overnight with no deploy. Walk through your investigation in PostgreSQL.
- [ ] Design the schema for invoices and line items with immutable history, corrections via credit notes, and fast "unpaid invoices for this customer" queries.
- [ ] Design an audit log table that receives 5,000 writes per second and must be queryable by entity for seven years.
- [ ] A customer stores most attributes in a `jsonb` blob and queries are slow. How do you decide what to extract, index or leave alone?
- [ ] Replica lag spikes to minutes every night. What are the likely causes and fixes?
- [ ] You must move a single-tenant-per-database estate of 300 databases into a shared multi-tenant schema. Plan it.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a query using `pg_stat_statements` to find the top queries by total time, and explain what you would do with the result.
- [ ] Find blocking and blocked sessions with `pg_stat_activity` and `pg_locks` (or `pg_blocking_pids`).
- [ ] Write a migration that adds a NOT NULL column with a default to a large table safely, with batched backfill and `lock_timeout`.
- [ ] Write a keyset-paginated query and the index that supports it.
- [ ] Set up a partitioned table by month with a default partition and show partition pruning in `EXPLAIN`.

---

**Good answers show:**
- Reasoning from what PostgreSQL does internally (MVCC, locks, planner statistics), not from rules of thumb.
- Reading real plans and numbers before proposing an index or a new technology.
- Every schema change is planned around lock levels, batch sizes, rollback and the impact on replicas.
- Multi-tenancy and partitioning choices are justified by tenant count, data size and operational cost.
- Awareness of what managed services change (failover, extensions, parameters) and what they do not.

**Prove it:** Load 50M rows into a local PostgreSQL, reproduce a slow query and a blocking `ALTER TABLE`, then fix both: write up the before/after plans, the safe migration script, and a monthly-partitioned version of the table with timings.

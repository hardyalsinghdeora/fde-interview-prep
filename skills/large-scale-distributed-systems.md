# Large-scale distributed systems

Principal and Staff interviews almost always include a round that pushes a design past one database and one region, to see whether you can reason about partitioning, consistency and failure rather than name-drop technologies. Staff/Principal Engineer, Software Architect and senior Forward Deployed Engineer roles ask most, usually as an open-ended "now make it 100x bigger" follow-up.

> Related: [System design](../topics/03-system-design/README.md) · [Decomposition](../topics/04-decomposition/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Vertical vs. horizontal scaling — what limits each, and when is vertical still the right answer?
- [ ] State the CAP theorem accurately, and explain why PACELC is often more useful in practice.
- [ ] Strong, sequential, causal, read-your-writes and eventual consistency — give a user-visible example of each.
- [ ] Partitioning strategies: range, hash and directory-based — pros and cons of each.
- [ ] Leader-follower vs. multi-leader vs. leaderless replication.
- [ ] Synchronous vs. asynchronous replication — what do you lose on failover with each?
- [ ] What are the latency numbers every engineer should know, roughly (memory, SSD, same-region network, cross-region)?
- [ ] Why are idempotency and retries with backoff and jitter essential in distributed systems?

## B. Working depth
- [ ] Consistent hashing: how does it work, why virtual nodes, and what happens on node add/remove?
- [ ] Quorums (N, R, W): how do they give tunable consistency, and what do sloppy quorums and hinted handoff change?
- [ ] Cache patterns: cache-aside, read-through, write-through, write-behind — and how each fails.
- [ ] Cache stampede and thundering herd: request coalescing, early refresh and jittered TTLs.
- [ ] Backpressure: how do bounded queues, load shedding and rate limiting protect a service?
- [ ] Timeouts, retries, circuit breakers and bulkheads — how do they interact, and how do retries cause retry storms?
- [ ] Read replicas and replication lag: how do you avoid showing users stale data after their own write?
- [ ] Distributed transactions: two-phase commit vs. sagas with compensation — when is each acceptable?
- [ ] Clocks: why wall-clock time is unsafe for ordering; Lamport clocks, vector clocks and hybrid logical clocks.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Explain Raft at a whiteboard level: leader election, log replication, terms, and what happens in a network partition.
- [ ] Where would you use a consensus system (etcd, ZooKeeper, a Raft-based DB) and where is it overkill?
- [ ] Leases, fencing tokens and distributed locks — why is a lock in Redis alone unsafe for correctness-critical work?
- [ ] Resharding a live PostgreSQL or MySQL database: approaches, dual writes vs. CDC backfill, and the cut-over.
- [ ] Hot keys and celebrity problems: detection and mitigation in caches, partitions and queues.
- [ ] Multi-region active-passive vs. active-active: conflict resolution, data residency, failover drills and cost.
- [ ] Tail latency at scale: why p99 dominates fan-out requests, and techniques such as hedged requests.
- [ ] Gray failures and partial outages: how do you detect a node that is slow but not dead?
- [ ] Capacity planning: how do you turn business growth into QPS, storage, connection and cost estimates with headroom?
- [ ] Cell-based architecture and blast-radius reduction — when does it pay off?
- [ ] Conflict-free replicated data types (CRDTs) vs. last-write-wins — when would you accept each?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Your billing system handles 1,000 orders a minute on one PostgreSQL primary. Plan for 100x without losing correctness on money.
- [ ] Design a globally distributed rate limiter for a public API.
- [ ] Design a distributed job scheduler that runs each job exactly once at the right time, even when nodes die.
- [ ] Design a multi-region read-heavy product catalogue with sub-100 ms reads worldwide and writes in one region.
- [ ] A Redis cache cluster restarts and the database falls over within a minute. Explain what happened and redesign to prevent it.
- [ ] Design a URL shortener or ID generator that produces unique, roughly sortable IDs without a single coordinator.
- [ ] Estimate the infrastructure for a chat or notifications system with 10M daily active users, and say which number you would validate first.

## E. Hands-on checks (things to be able to DO)
- [ ] Do a back-of-the-envelope estimate (QPS, storage per year, bandwidth) in under five minutes for a given prompt.
- [ ] Implement consistent hashing with virtual nodes and measure key movement when a node is added.
- [ ] Implement a token-bucket rate limiter backed by Redis with an atomic script, and test it under concurrency.
- [ ] Run a three-node etcd or Raft demo, kill the leader, and observe election and recovery.
- [ ] Use a load-testing tool to find the knee of a service's latency curve and show the effect of a bounded queue.

---

**Good answers show:**
- Starting from requirements and numbers (QPS, data size, consistency needs) before reaching for components.
- Choosing consistency per operation (money vs. likes), not one model for the whole system.
- Explicit failure-mode analysis: partitions, slow nodes, retries, cache loss, failover data loss.
- Knowing the cost of each "nine" and of multi-region, and being willing to recommend the simpler design.
- Migration paths from today's system to the target, with safe, reversible steps.
- Precise vocabulary used correctly (linearisable, quorum, fencing) without hand-waving.

**Prove it:** Build a small sharded key-value or ledger service: consistent-hash routing across three PostgreSQL or Redis nodes, idempotent writes, a read-your-writes guarantee, and a scripted resharding from three to four nodes under load. Publish the load-test results and a short failure log (kill a node, add latency, partition the network) with what the system did.

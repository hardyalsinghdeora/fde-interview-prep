# Kafka & event streaming

Event streaming sits at the centre of most large backends, and interviewers use it to test whether you understand ordering, delivery guarantees and data contracts rather than just "put a queue in front of it". Staff/Principal backend, Software Architect and data-heavy Forward Deployed Engineer roles ask most, often as "Kafka or SQS, and why?".

> Related: [System design](../topics/03-system-design/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is the difference between a log-based stream (Kafka, Kinesis) and a message queue (SQS, RabbitMQ)?
- [ ] Explain topics, partitions, offsets and brokers. What does a partition give you?
- [ ] What is a consumer group, and what happens when you have more consumers than partitions?
- [ ] Where is ordering guaranteed in Kafka, and how does the message key affect it?
- [ ] At-most-once vs. at-least-once vs. exactly-once delivery — what does each mean in practice?
- [ ] What is retention, and how is a compacted topic different from a time-retained one?
- [ ] What does the replication factor protect against? What are leaders, followers and in-sync replicas?
- [ ] When is a simple SQS queue (standard or FIFO) a better choice than Kafka?

## B. Working depth
- [ ] Producer settings: `acks`, retries, idempotent producer, batching and linger — what does each trade off? *(verify)*
- [ ] How do consumers commit offsets, and what goes wrong with auto-commit when processing fails?
- [ ] What triggers a consumer-group rebalance, and how do you limit the pain (cooperative rebalancing, static membership)? *(verify)*
- [ ] How do you design an idempotent consumer? Where do you store the dedupe key, and for how long?
- [ ] How do you handle poison messages: retry topics, dead-letter topics, and backoff without blocking a partition?
- [ ] How do you choose the number of partitions, and why is increasing it later painful for keyed data?
- [ ] What is consumer lag, how do you monitor it, and what does growing lag usually mean?
- [ ] Explain the transactional outbox pattern. Why not just publish to Kafka inside the request handler after the DB commit?
- [ ] Schema Registry with Avro/Protobuf/JSON Schema: what do backward, forward and full compatibility mean? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How does Kafka "exactly-once" actually work (idempotent producer + transactions + read-committed), and where does it stop applying?
- [ ] CDC with Debezium: how does log-based capture from PostgreSQL/MySQL work, and what are the failure modes (replication slots filling disk, schema changes, snapshots)? *(verify)*
- [ ] Outbox via polling vs. outbox via CDC — latency, ordering and operational trade-offs.
- [ ] Hot partitions: how do you detect and fix a skewed key without breaking per-entity ordering?
- [ ] Event-carried state transfer vs. event notification vs. event sourcing — when is each worth the complexity?
- [ ] Schema evolution in practice: renaming a field, splitting an event, and retiring a topic with many consumers.
- [ ] Self-managed Kafka vs. a managed service (MSK, Confluent Cloud) vs. Kinesis vs. Google Pub/Sub — ordering, retention, throughput and cost. *(verify)*
- [ ] Multi-region streaming: replication between clusters, offset translation and active-active pitfalls. *(verify)*
- [ ] Choreography vs. orchestration for multi-step business processes (sagas) — how do you debug a stuck saga?
- [ ] How do you reason about end-to-end latency and throughput budgets from producer to sink?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] An order-to-cash system publishes `OrderPlaced`, `InvoiceIssued` and `PaymentReceived`. Design the topics, keys and consumers so a customer's events are never processed out of order.
- [ ] A consumer has charged customers twice after a rebalance. Walk through the root cause and the fix.
- [ ] You are migrating from SQS to Kafka (or the reverse). What would justify it, and how do you cut over without losing or duplicating messages?
- [ ] Design a CDC pipeline from a production PostgreSQL database to a warehouse and a search index, with a backfill.
- [ ] Design a webhook fan-out system on top of a stream with per-customer retries and rate limits.
- [ ] Lag has grown to six hours on one partition only. How do you investigate and recover?
- [ ] Design an audit-event stream that compliance can replay for seven years without keeping the brokers that long.

## E. Hands-on checks (things to be able to DO)
- [ ] Run a local Kafka (or Redpanda) in Docker, create a topic with several partitions, and show ordering per key with the CLI tools.
- [ ] Write a Node.js producer and consumer group with manual offset commits and an idempotent handler backed by PostgreSQL.
- [ ] Implement a transactional outbox table plus relay, and show no lost or duplicate events when you kill the process mid-run.
- [ ] Register a schema, make a compatible change and an incompatible one, and show the registry rejecting the second.
- [ ] Add a retry topic and a dead-letter topic, and build a lag dashboard or alert.

---

**Good answers show:**
- A clear rule for choosing a queue vs. a stream, tied to replay, fan-out, ordering and ops cost.
- Precise language on delivery semantics: "exactly-once processing" comes from idempotent side effects, not the broker alone.
- Partition-key design driven by the business entity whose order matters.
- Data contracts treated as an API: schema registry, compatibility rules, versioned events and deprecation plans.
- Operational instincts: lag, rebalances, hot partitions, DLQs and backfills.
- Honest cost and team-skill trade-offs, including when SQS/SNS is simply enough.

**Prove it:** Build a small order-events pipeline: a Node.js API writes orders to PostgreSQL with an outbox, Debezium or a relay publishes to Kafka keyed by customer, and two consumer groups (invoice and notifications) process events idempotently with a DLQ. Kill brokers and consumers during a load test, then write up what broke, what held, and the measured duplicate and loss counts.

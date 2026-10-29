# Observability and reliability

Senior engineers are judged on how their systems behave at 3 a.m., so interviewers ask how you know a service is healthy, how you set reliability targets and how you lead an incident. Principal/Staff Engineer, Engineering Manager and platform-leaning Architect roles ask most, often through a "tell me about an outage" story.

> Related: [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [System design](../topics/03-system-design/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [Behavioural](../topics/10-behavioral/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Logs vs. metrics vs. traces: what question does each answer best?
- [ ] Monitoring vs. observability: is there a real difference?
- [ ] SLI vs. SLO vs. SLA. Give an example of each for an API.
- [ ] What is an error budget and what happens when it runs out?
- [ ] The four golden signals; RED vs. USE methods.
- [ ] What is OpenTelemetry and what problem does it solve?
- [ ] Why use percentiles (p50, p95, p99) instead of averages for latency?
- [ ] What is a blameless postmortem?

## B. Working depth
- [ ] Structured logging: which fields should every log line have, and how do you correlate logs with traces?
- [ ] How does trace context propagate across HTTP, queues (SQS, Kafka) and async jobs?
- [ ] Head-based vs. tail-based sampling: when does each lose the traces you need?
- [ ] Metric cardinality: why is a `user_id` label dangerous, and how do you get per-tenant visibility anyway?
- [ ] Counters, gauges and histograms. Why can't you average percentiles across instances?
- [ ] What makes a good alert? Symptoms vs. causes, and paging vs. ticket alerts.
- [ ] Multi-window, multi-burn-rate alerting on an SLO: how does it work?
- [ ] Health checks: liveness vs. readiness vs. deep dependency checks, and how each can cause outages.
- [ ] Timeouts, retries with backoff and jitter, circuit breakers and bulkheads: how they stop cascading failures.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you pick SLO targets with product, and what do you do when a team wants 99.99% without paying for it?
- [ ] Observability costs are growing faster than traffic. Where do you cut (sampling, retention, log levels, cardinality)?
- [ ] Retry storms and thundering herds: how have they caused outages, and how do you design against them?
- [ ] Capacity planning: how do you forecast load, find the bottleneck and decide headroom?
- [ ] Load shedding and graceful degradation: what does your service drop first under overload?
- [ ] Chaos testing: what do you inject first, how do you limit blast radius, and when is a team not ready for it?
- [ ] How do you make postmortem action items actually get done?
- [ ] Observability for LLM features: tracing prompts, tool calls, token cost and latency, quality signals *(verify)*.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] p99 latency doubled after a deploy but p50 is unchanged. Walk through the investigation.
- [ ] Design the observability stack for 30 microservices: collection, storage, dashboards, alerting, cost.
- [ ] You are incident commander for a payment outage. Run the first 30 minutes: roles, comms, decisions.
- [ ] Define SLIs and SLOs for a billing system where correctness matters more than latency.
- [ ] On-call is burning out the team with 40 pages a week. What do you change?

## E. Hands-on checks (things to be able to DO)
- [ ] Instrument a Node.js service with the OpenTelemetry SDK and see one trace span across an HTTP call and a queue message *(verify)*.
- [ ] Write PromQL for error rate and p99 latency from a histogram.
- [ ] Write an SLO burn-rate alert rule.
- [ ] Write a blameless postmortem for a real or invented incident: timeline, impact, contributing factors, actions.

---

**Good answers show:**
- Reliability targets tied to user experience and business cost, not chosen by habit.
- Alerting on symptoms against SLOs, with a clear story on reducing noise.
- Understanding the cost of telemetry and making deliberate sampling and retention choices.
- Calm, structured incident leadership: roles, communication cadence, mitigate first and diagnose later.
- Postmortems that fix systems and processes rather than blame people, with follow-through.
- Concrete examples with numbers: MTTR, pages per week, SLO attainment before and after.

**Prove it:** Instrument two small services and a queue worker with OpenTelemetry, send data to a local Grafana/Prometheus/Tempo (or similar) stack, define one SLO with a burn-rate alert, then break a dependency on purpose and write a short postmortem from what the telemetry showed.

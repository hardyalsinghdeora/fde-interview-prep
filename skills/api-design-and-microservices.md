# API Design & Microservices

APIs are the contracts other teams and customers build on, and service boundaries decide how fast an organisation can move, so senior interviews test whether you can design interfaces that survive years of change and split (or refuse to split) a system sensibly. Software Architect and Principal/Staff Engineer loops go deepest; Forward Deployed Engineer loops focus on integration, idempotency and versioning against customer systems.

> Related: [Cloud, DevOps & integration](../topics/08-cloud-devops-integration/README.md) · [System design](../topics/03-system-design/README.md) · [Decomposition](../topics/04-decomposition/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What makes an API "RESTful" in practice: resources, verbs, status codes, and which of the REST constraints teams actually honour?
- [ ] When do you choose REST, gRPC or GraphQL for public APIs, internal service-to-service calls and a mobile front end? Name the cost of each.
- [ ] Safe vs. idempotent methods: which HTTP methods are which, and why is POST not idempotent by default?
- [ ] 400 vs. 409 vs. 422, 401 vs. 403, 502 vs. 503 vs. 504: when do you return each?
- [ ] Offset vs. cursor (keyset) pagination: correctness under concurrent inserts, performance, and how you encode the cursor.
- [ ] What does an API gateway do (routing, auth, rate limiting, transformation) and what should not live in it?
- [ ] What is a backend-for-frontend (BFF) and when is it better than one general-purpose API?

## B. Working depth
- [ ] Implement idempotency keys for a "create payment" endpoint: storage, key scope, TTL, concurrent duplicates, and returning the original response.
- [ ] Versioning strategies — URL, header, media type, or no versions with additive change only. Which do you pick and how do you deprecate?
- [ ] Which changes are backward compatible for JSON APIs, protobuf and GraphQL schemas, and which silently break clients?
- [ ] Design error responses: a consistent error shape, machine-readable codes, and which details you never leak.
- [ ] Rate limiting algorithms (token bucket, leaky bucket, fixed vs. sliding window) and implementing one in Redis across many gateway nodes.
- [ ] Webhooks you send: signing, retries with backoff, ordering, replay protection and letting customers re-fetch missed events.
- [ ] Long-running operations: async request/response with `202 Accepted`, status resources, callbacks vs. polling.
- [ ] Bulk and batch endpoints: partial failure semantics and how you report them.
- [ ] Service-to-service auth: mTLS, signed tokens, service identities, and propagating the end user's identity.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you find service boundaries: domain-driven design, bounded contexts, data ownership, team ownership — and the signs a boundary is wrong?
- [ ] When should you not split a monolith? Argue for a modular monolith in a specific situation.
- [ ] Sagas: orchestration vs. choreography, compensating actions, and what happens when a compensation itself fails.
- [ ] Why are two-phase commit and distributed transactions usually avoided between microservices, and what do you use instead (outbox, idempotent consumers, reconciliation)?
- [ ] Shared database between services: why it is a problem, and how you migrate away from one safely.
- [ ] Contract testing (consumer-driven contracts, schema checks in CI): how it prevents breaking changes and where it gives false confidence.
- [ ] Synchronous call chains: cascading failures, timeout budgets, circuit breakers, bulkheads and load shedding.
- [ ] Observability across services: correlation IDs, distributed tracing, and making one request's journey debuggable.
- [ ] GraphQL at scale: N+1 resolvers and dataloaders, query cost limits, caching problems and authorisation per field.
- [ ] The operational and cost overhead of microservices (deploy pipelines, on-call, network hops, data duplication) — how you quantify it before deciding.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design the public REST API for a billing product: customers, subscriptions, invoices, payments, with pagination, filtering, idempotency and webhooks.
- [ ] Order placement spans orders, payments, inventory and shipping services. Design the saga, including timeouts and every failure path.
- [ ] You need to make a breaking change to an API used by 400 external customers. Plan the change from announcement to removal.
- [ ] Your gateway must enforce per-tenant rate limits and quotas tied to the customer's pricing plan. Design it.
- [ ] A mobile app makes 14 calls to render its home screen. Design a BFF or aggregation layer and explain the trade-offs.
- [ ] Design the internal API that lets LLM agents call your services as tools: schemas, auth scopes, idempotency, and human approval for risky actions.

## E. Hands-on checks (things to be able to DO)
- [ ] Write an OpenAPI spec for a small resource with pagination, error schema and an idempotency header.
- [ ] Write a protobuf service definition and show which field changes are safe.
- [ ] Implement a sliding-window or token-bucket rate limiter in Redis with a Lua script or atomic commands.
- [ ] Write a consumer-driven contract test, or a CI schema-diff check that fails on breaking changes.

---

**Good answers show:**
- APIs designed from the consumer's needs and the domain, with stable identifiers, clear errors and explicit idempotency.
- A deliberate evolution policy: what counts as breaking, how deprecation is communicated, and how it is enforced in CI.
- Service boundaries drawn around data ownership and teams, with an honest case for not splitting when that is right.
- Failure handling across services — sagas, outbox, retries, reconciliation — instead of pretending distributed transactions are free.
- Awareness of cost: latency per hop, operational burden and the people needed to run each service.

**Prove it:** Build two small services (orders and payments) behind a gateway with per-tenant rate limiting, idempotency keys and an orchestrated saga with compensation; add a contract test in CI and a short write-up of each failure you injected and how the system recovered.

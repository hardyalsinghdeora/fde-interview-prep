# Java & Spring Boot

Many enterprise, fintech and large-product teams run on the JVM, so a Node.js/TypeScript engineer moving into these shops will be asked to show they can reason about threads, memory and the Spring ecosystem, not just syntax. Staff/Principal backend, Software Architect and Engineering Manager roles in JVM-heavy organisations ask most, often framed as "how would you get productive in a Spring codebase?".

> Related: [Coding](../topics/02-coding/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [System design](../topics/03-system-design/README.md) · [Learning round](../topics/05-learning-round/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is the JVM, and what happens between `javac` and running code (bytecode, class loading, JIT)?
- [ ] Heap vs. stack vs. metaspace — what lives where?
- [ ] Primitive types vs. wrapper types, autoboxing, and why `==` on `Integer` can surprise you.
- [ ] `equals` and `hashCode`: the contract, and what breaks in a `HashMap` if you get it wrong.
- [ ] Checked vs. unchecked exceptions — when should each be used?
- [ ] Interfaces, abstract classes, records and sealed types — when would you use each? *(verify)*
- [ ] Core collections: `ArrayList` vs. `LinkedList`, `HashMap` vs. `TreeMap` vs. `ConcurrentHashMap`.
- [ ] What does Spring's dependency injection do, and what is the difference between a bean, a component and a configuration class?
- [ ] What does Spring Boot add on top of Spring (auto-configuration, starters, embedded server)?

## B. Working depth
- [ ] Compare the Node.js event loop with Java's thread-per-request model. How do virtual threads change that picture? *(verify)*
- [ ] `synchronized`, `volatile`, `ReentrantLock` and atomics — what does each guarantee?
- [ ] `ExecutorService`, `CompletableFuture` and thread pools: how do you size a pool for I/O-bound vs. CPU-bound work?
- [ ] Streams API vs. loops: laziness, parallel streams and when not to use them.
- [ ] Bean scopes and lifecycle; what goes wrong with a stateful singleton bean?
- [ ] How does `@Transactional` work (proxies), and why does calling a transactional method from the same class not start a transaction?
- [ ] JPA/Hibernate: entity states, persistence context, lazy vs. eager loading, and the N+1 problem.
- [ ] Spring profiles, `application.yml`, externalised configuration and secrets in production.
- [ ] Maven vs. Gradle: dependency scopes, multi-module builds and dependency conflicts.
- [ ] Testing: JUnit 5, Mockito, `@SpringBootTest` vs. slice tests (`@WebMvcTest`, `@DataJpaTest`), and Testcontainers.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Garbage collectors (G1, ZGC, Shenandoah) — how do you choose, and what do you tune first? *(verify)*
- [ ] How do you diagnose a memory leak or rising GC pauses in production (heap dumps, GC logs, JFR)?
- [ ] JVM in containers: heap sizing vs. container memory limits, startup time, and why pods get OOM-killed. *(verify)*
- [ ] Spring MVC vs. WebFlux (reactive) vs. virtual threads — which would you pick for a new I/O-heavy service and why? *(verify)*
- [ ] Hibernate at scale: batching, second-level cache, optimistic locking with `@Version`, and when to drop to plain SQL or jOOQ.
- [ ] Database migrations with Flyway or Liquibase on a live system — expand/contract with a JVM service.
- [ ] Startup and footprint: GraalVM native images and class-data sharing — trade-offs for serverless and autoscaling. *(verify)*
- [ ] Observability: Actuator, Micrometer, OpenTelemetry tracing — what would you wire in on day one? *(verify)*
- [ ] Coming from TypeScript, what would you keep (types, small modules, contract tests) and what would you change in a Java codebase?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A Spring Boot service's p99 latency doubles under load with low CPU. How do you find out whether it is the connection pool, thread pool, GC or the database?
- [ ] An endpoint loads an order with its lines and payments and fires 200 queries. Find and fix it.
- [ ] Design a billing service in Spring Boot that must never double-charge: transactions, idempotency keys and retries.
- [ ] You inherit a large legacy Spring monolith. How do you get productive in the first two weeks, and how would you begin to split it?
- [ ] Your team is TypeScript-first and the organisation mandates Java for new services. How do you plan the transition as a lead?
- [ ] Two threads update the same account balance and the total drifts. Explain the bug and two ways to fix it.

## E. Hands-on checks (things to be able to DO)
- [ ] Build a Spring Boot REST API with validation, error handling, a JPA repository and Flyway migrations against PostgreSQL.
- [ ] Write a unit test with Mockito and an integration test with Testcontainers for the same feature.
- [ ] Reproduce an N+1 query with Hibernate SQL logging on, then fix it with a fetch join or entity graph.
- [ ] Take a heap dump and a thread dump from a running service and explain what you see.
- [ ] Containerise the service with a multi-stage build and set sensible JVM memory flags for the container.

---

**Good answers show:**
- A clear mental model of threads, memory and GC, compared honestly with the Node.js event loop.
- Knowing what Spring does "by magic" (proxies, auto-configuration) and how to debug it when it misbehaves.
- ORM pragmatism: seeing the SQL, preventing N+1, and knowing when to step outside JPA.
- Production tuning grounded in measurement (GC logs, profilers, pool metrics) rather than flag folklore.
- Transferable principles from TypeScript work: typed contracts, testing pyramids, safe migrations, idempotency.
- A credible plan for learning a new stack quickly and helping a team adopt it.

**Prove it:** Port one small Node.js service you know well (for example an invoices API with PostgreSQL) to Spring Boot with JPA, Flyway, Testcontainers tests and Actuator metrics. Load-test both versions, compare latency, memory and startup, and write a one-page note on what was easier, harder and surprising.

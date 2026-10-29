# Node.js & TypeScript

For TypeScript-heavy backend teams, the senior bar is not "can you write an Express route" but whether you understand the runtime well enough to explain a latency spike, a memory leak or a stuck process, and whether you can use the type system to make bad states unrepresentable. Principal/Staff Engineer and Forward Deployed Engineer loops ask this most, often with live coding and a production-debugging story.

> Related: [Coding](../topics/02-coding/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [Cloud, DevOps & integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Explain the event loop phases (timers, pending callbacks, poll, check, close) and where `process.nextTick` and promise microtasks run relative to them.
- [ ] Predict the output order of a snippet mixing `setTimeout(0)`, `setImmediate`, `process.nextTick` and `Promise.resolve().then`.
- [ ] What does libuv's thread pool do, which APIs use it (fs, dns.lookup, crypto, zlib), and what happens when it is saturated?
- [ ] `type` vs. `interface`; structural typing; what `unknown`, `any` and `never` each mean and when you use them.
- [ ] Union and intersection types, discriminated unions and exhaustiveness checking with `never`.
- [ ] CommonJS vs. ES modules in Node: interop problems, `"type": "module"`, and how TypeScript's `module`/`moduleResolution` settings affect it *(verify for current Node and TypeScript versions)*.
- [ ] Error handling with async/await: unhandled rejections, `Promise.all` vs. `Promise.allSettled`, and what happens to the other promises when one rejects.

## B. Working depth
- [ ] Streams: readable, writable, duplex, transform; what backpressure is and how `pipeline` handles it and errors. Why can `readable.on('data')` without backpressure blow up memory?
- [ ] When do you use worker threads vs. child processes vs. the cluster module vs. just more containers?
- [ ] Generics with constraints, conditional types, `infer`, and mapped types — write a `DeepPartial<T>` or a typed `pick` function.
- [ ] Template literal types and key remapping: give a real use (typed event names, route params).
- [ ] How do you validate untrusted input at runtime and keep the static type in sync (schema libraries that infer types) *(verify library choice)*?
- [ ] Branded / nominal types for IDs and money: why `CustomerId` and `InvoiceId` should not be interchangeable strings.
- [ ] Express vs. Hono vs. NestJS vs. Fastify: middleware model, performance, dependency injection, and when you would pick each *(verify)*.
- [ ] How do you structure a large TypeScript service: modules, layering, dependency injection, and where domain types live.
- [ ] Testing: unit vs. integration vs. contract tests in Node; test runners (Jest, Vitest, `node:test`); how you test code that uses time, randomness and the network.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] The event loop is blocked: how do you detect it in production (event-loop lag, `monitorEventLoopDelay`), and what typically causes it (JSON.parse of huge payloads, sync crypto, regex backtracking)?
- [ ] Walk through finding a memory leak: heap snapshots, comparing retained sizes, common culprits (closures, caches without bounds, listeners, global maps).
- [ ] How does V8 garbage collection (young vs. old generation) affect latency, and when do you tune `--max-old-space-size` in a container?
- [ ] Graceful shutdown: handling SIGTERM in Kubernetes or ECS, draining HTTP keep-alive connections, finishing in-flight jobs, and closing DB pools.
- [ ] Connection pool sizing for PostgreSQL from many Node replicas: why more connections can make things slower.
- [ ] `AbortController` and timeouts end to end: cancelling outbound HTTP calls, DB queries and LLM streams when the client disconnects.
- [ ] Type-system limits: where TypeScript gives false confidence (type assertions, `any` leaking from libraries, unchecked index access) and which compiler flags you enforce.
- [ ] Monorepo choices for TypeScript services: project references, shared packages, build times, and versioning internal libraries.
- [ ] Supply-chain risk in npm: lockfiles, audit, provenance, postinstall scripts, and how you keep dependencies current without breaking things.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] An API's p99 jumps from 80 ms to 2 s under load but CPU is only 40%. What do you suspect and how do you confirm it?
- [ ] Memory grows by 50 MB per hour until the container is OOM-killed. Describe exactly what you collect and how you narrow it down.
- [ ] Design a service that streams a 5 GB CSV export from PostgreSQL to S3 without loading it into memory.
- [ ] Build a rate-limited, retrying client for a flaky third-party API in TypeScript, with typed errors and cancellation.
- [ ] Design the types for an order state machine so invalid transitions fail to compile.
- [ ] Proxy an LLM's streamed response to a browser via Server-Sent Events, handling disconnects, timeouts and token accounting.

## E. Hands-on checks (things to be able to DO)
- [ ] Take and compare two heap snapshots, and produce a CPU profile or flame graph of a hot endpoint.
- [ ] Write a transform stream that parses NDJSON with correct backpressure, used via `pipeline`.
- [ ] Write a `tsconfig.json` for a strict Node service and explain each important flag.
- [ ] Write an integration test that runs against a real PostgreSQL in a container.

---

**Good answers show:**
- A precise mental model of the event loop, microtasks and the thread pool, used to explain real incidents.
- Production debugging done with evidence: metrics, profiles and heap snapshots rather than guesses.
- Types used to encode business rules (states, IDs, money), with runtime validation at the boundaries.
- Framework choice argued from team size, performance needs and maintainability, not fashion.
- Operational care: graceful shutdown, timeouts and cancellation, pool sizing, dependency hygiene.

**Prove it:** Build a small TypeScript service that streams a large PostgreSQL export to S3-compatible storage, deliberately add a memory leak and a blocking call, then find both with profiling tools and write up the investigation with screenshots of the before/after metrics.

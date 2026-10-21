# 13 — CS Fundamentals (Rapid-Fire)

These come up as follow-ups in coding, system design, or debugging rounds. Know each in 2–3 sentences.

---

## Networking
- [ ] What happens when you type a URL into a browser and press Enter?
- [ ] TCP vs. UDP.
- [ ] TCP three-way handshake.
- [ ] HTTP/1.1 vs. HTTP/2 vs. HTTP/3 (QUIC).
- [ ] HTTPS / TLS — how does it work?
- [ ] DNS resolution — how does it work? What are A, CNAME, MX, TXT records?
- [ ] OSI model / TCP-IP layers.
- [ ] Load balancers: L4 vs. L7; algorithms (round robin, least connections, consistent hashing).
- [ ] Reverse proxy vs. forward proxy.
- [ ] CDN — how and why?
- [ ] Cookies vs. sessions vs. tokens.
- [ ] WebSockets — how do they work?
- [ ] NAT, firewalls, ports.
- [ ] IPv4 vs. IPv6; CIDR notation.

## Operating systems
- [ ] Process vs. thread.
- [ ] Concurrency vs. parallelism.
- [ ] Context switching.
- [ ] Virtual memory, paging.
- [ ] Stack vs. heap.
- [ ] What is a deadlock? Four conditions? How to prevent?
- [ ] Mutex vs. semaphore vs. lock vs. condition variable.
- [ ] Race conditions — example and fix.
- [ ] Python GIL — what is it and how does it affect threading?
- [ ] async/await vs. threads vs. multiprocessing — when to use each.
- [ ] File descriptors; what happens with "too many open files"?
- [ ] Signals (SIGTERM vs. SIGKILL); graceful shutdown.
- [ ] Garbage collection basics.

## Databases
- [ ] SQL vs. NoSQL — when to use each.
- [ ] Types of NoSQL (key-value, document, column-family, graph).
- [ ] ACID vs. BASE.
- [ ] CAP theorem; PACELC.
- [ ] Indexing internals (B-tree, LSM-tree).
- [ ] Replication: leader–follower, multi-leader, leaderless.
- [ ] Sharding / partitioning strategies; hot partitions.
- [ ] Consistent hashing.
- [ ] Optimistic vs. pessimistic locking.
- [ ] Connection pooling.
- [ ] Caching strategies: cache-aside, write-through, write-back; cache invalidation; eviction (LRU, LFU).
- [ ] Redis use cases.
- [ ] Vector databases — how do they differ from traditional DBs?

## Distributed systems
- [ ] Strong vs. eventual consistency.
- [ ] Idempotency and exactly-once processing.
- [ ] Consensus (Raft/Paxos) at a high level.
- [ ] Leader election.
- [ ] Distributed transactions: 2PC, sagas, outbox pattern.
- [ ] Back-pressure.
- [ ] Rate-limiting algorithms: token bucket, leaky bucket, fixed window, sliding window log/counter.
- [ ] Circuit breaker, bulkhead, retry with jitter.
- [ ] Monolith vs. microservices — trade-offs.
- [ ] Event-driven architecture; event sourcing; CQRS.
- [ ] Clock skew; logical clocks.

## Software engineering practices
- [ ] SOLID principles.
- [ ] Common design patterns (singleton, factory, strategy, observer, adapter, decorator).
- [ ] Unit vs. integration vs. end-to-end tests; mocking.
- [ ] TDD — pros and cons.
- [ ] Code review — what do you look for?
- [ ] Git: merge vs. rebase; resolving conflicts; branching strategies.
- [ ] Technical debt — how do you manage it?
- [ ] How do you write maintainable code that a customer team can take over?

## Language-specific (Python)
- [ ] List vs. tuple vs. set vs. dict — complexity of common operations.
- [ ] Mutable default arguments pitfall.
- [ ] Generators and `yield`; iterators.
- [ ] Decorators and context managers.
- [ ] `*args`, `**kwargs`.
- [ ] Shallow vs. deep copy.
- [ ] `asyncio` basics: event loop, `gather`, semaphores.
- [ ] Type hints, dataclasses, Pydantic.
- [ ] Virtual environments and dependency management (pip, poetry, uv).

## Language-specific (JavaScript/TypeScript)
- [ ] Event loop, microtasks vs. macrotasks.
- [ ] Promises and async/await.
- [ ] Closures; `this` binding.
- [ ] `==` vs. `===`; `let`/`const`/`var`.
- [ ] TypeScript generics and utility types.

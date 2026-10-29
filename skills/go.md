# Go

Go is common for infrastructure, platform tooling and high-throughput backend services, so senior candidates moving from Node.js are asked whether they understand its concurrency model and idioms rather than writing "TypeScript in Go". Platform, Staff/Principal backend and Forward Deployed Engineer roles ask most, especially where Kubernetes operators, CLIs or network services are involved.

> Related: [Coding](../topics/02-coding/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is a goroutine, and how is it different from an OS thread and from a Node.js async callback?
- [ ] Unbuffered vs. buffered channels — what does each guarantee about synchronisation?
- [ ] What does `select` do, and how do you use it for timeouts and cancellation?
- [ ] How does Go handle errors (`error` values, wrapping with `%w`, `errors.Is` / `errors.As`), and why no exceptions?
- [ ] Interfaces are satisfied implicitly — what does that enable, and why "accept interfaces, return structs"?
- [ ] Slices vs. arrays: length, capacity, and the aliasing bug when you `append` to a shared slice.
- [ ] Value vs. pointer receivers — how do you choose?
- [ ] What is `defer` for, and in what order do deferred calls run?
- [ ] Modules and packages: `go.mod`, versioning, and exported vs. unexported names.

## B. Working depth
- [ ] What is `context.Context` for? How do cancellation and deadlines propagate through an HTTP handler to the database?
- [ ] `sync.Mutex`, `RWMutex`, `WaitGroup`, `Once` and atomics — when would you use each instead of channels?
- [ ] How do goroutine leaks happen, and how do you find and prevent them?
- [ ] What does the race detector catch, and what can it miss?
- [ ] Worker pools and fan-out/fan-in with channels; how do you bound concurrency? What does `errgroup` add?
- [ ] `panic` and `recover` — when is panicking acceptable, and how should an HTTP server handle a panic in a handler?
- [ ] The nil interface gotcha: why can an interface holding a nil pointer be non-nil?
- [ ] Generics: what problems do they solve, and when do you still prefer interfaces? *(verify)*
- [ ] Table-driven tests, subtests, benchmarks and fuzzing with the standard `testing` package.
- [ ] `net/http` server basics: timeouts, middleware, graceful shutdown with `Shutdown(ctx)`.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Explain the scheduler at a high level (G, M, P), and what `GOMAXPROCS` means inside a container with CPU limits. *(verify)*
- [ ] How does Go's garbage collector work at a high level, and what do `GOGC` and the memory limit setting tune? *(verify)*
- [ ] Escape analysis and allocations: how do you reduce heap allocations in a hot path, and when is it worth it?
- [ ] Profiling with `pprof`: CPU, heap, goroutine and block/mutex profiles — what does each tell you?
- [ ] When would you choose Go over Node.js/TypeScript, Java or Rust for a new service, and when would you not?
- [ ] Error-handling strategy across a large codebase: sentinel errors vs. typed errors vs. wrapping, and what to log where.
- [ ] Project layout and dependency management for a multi-service Go monorepo; keeping packages small and acyclic.
- [ ] Building Kubernetes controllers or operators in Go: the reconcile loop, informers and idempotency. *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A Go service's memory grows steadily until it is OOM-killed. Walk through how you find the cause.
- [ ] Design a concurrent crawler or fetcher that respects a global rate limit, per-host limits and cancellation.
- [ ] Your Node.js API gateway is CPU-bound on JSON transformation. Make the case for (or against) rewriting it in Go, and plan the migration.
- [ ] Two goroutines sometimes deadlock in production. How do you reproduce and diagnose it?
- [ ] Design a small, reliable SQS or Kafka consumer service in Go with bounded concurrency, retries and graceful shutdown.
- [ ] Review this code that starts a goroutine per request and writes results into a shared map — what is wrong?

## E. Hands-on checks (things to be able to DO)
- [ ] Write an HTTP service with `net/http`, context-aware handlers, structured logging, timeouts and graceful shutdown.
- [ ] Implement a bounded worker pool that stops cleanly on context cancellation, with tests run under `-race`.
- [ ] Capture and read a CPU and heap profile with `pprof` and fix one hotspot.
- [ ] Write table-driven tests and a benchmark, and compare two implementations with `benchstat`.
- [ ] Build a small CLI that calls a REST API, handles errors with wrapping, and ships as a single static binary in a minimal container image.

---

**Good answers show:**
- Idiomatic Go: small interfaces, explicit errors, simple packages, and no needless abstraction.
- Concurrency with ownership: who closes the channel, who cancels the context, how goroutines end.
- Measurement before optimisation, using `pprof`, benchmarks and the race detector.
- Production awareness: timeouts everywhere, graceful shutdown, container CPU and memory settings.
- A balanced view of when Go is the right tool versus TypeScript or the JVM, including team skills and hiring.
- Clean, readable code that a reviewer can follow, valued over clever code.

**Prove it:** Write a small Go service that consumes jobs from a queue (SQS or Redis), processes them with a bounded worker pool, exposes Prometheus metrics and a `pprof` endpoint, and shuts down without losing work. Load-test it against an equivalent Node.js worker and write up throughput, memory, and what idioms took longest to learn.

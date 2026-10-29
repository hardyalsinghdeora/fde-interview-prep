# Python

Python is the default language of AI and data work and a common choice for backend services, so roles that touch LLM agents, RAG or data pipelines expect fluent, idiomatic Python even from engineers whose main stack is elsewhere. Agentic-AI Architect and Forward Deployed Engineer loops ask most, frequently through live coding in Python, with backend roles probing asyncio, FastAPI and packaging.

> Related: [Coding](../topics/02-coding/README.md) · [AI / LLM](../topics/06-ai-llm/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Mutable vs. immutable types, and the mutable default argument bug — why does it happen?
- [ ] What happens on `a = b` for a list, and what is the difference between shallow and deep copy?
- [ ] Explain the data model dunder methods you use most (`__init__`, `__repr__`, `__eq__`, `__hash__`, `__iter__`, `__enter__`/`__exit__`) and why `__eq__` without `__hash__` matters.
- [ ] Generators and iterators: what `yield` does, and when a generator expression beats a list comprehension.
- [ ] Decorators: write one that times a function and preserves its name with `functools.wraps`.
- [ ] Context managers: write one with a class and one with `contextlib.contextmanager`.
- [ ] `dataclass` vs. `NamedTuple` vs. pydantic model vs. plain dict — when do you use each?
- [ ] What is the GIL, what does it prevent and not prevent, and what is the status of free-threaded Python *(verify)*?

## B. Working depth
- [ ] Type hints: `Optional`, unions with `|`, `TypedDict`, `Protocol`, `Generic`/`TypeVar`, `Literal`. How strict is your type checker setup in CI?
- [ ] asyncio: event loop, coroutines vs. tasks, `gather` vs. `TaskGroup`, cancellation and timeouts. What happens if you call a blocking function inside a coroutine?
- [ ] Threads vs. processes vs. asyncio for I/O-bound vs. CPU-bound work; when do you reach for `concurrent.futures`?
- [ ] FastAPI: dependency injection, request validation, background tasks, sync vs. async endpoints and the thread pool behind sync ones.
- [ ] pydantic: validators, serialisation, strict vs. lax mode, and using models for LLM structured output *(verify pydantic version differences)*.
- [ ] Packaging: `pyproject.toml`, virtual environments, lock files, and tools such as pip, Poetry and uv — what do you use and why *(verify)*?
- [ ] pytest: fixtures and scope, parametrisation, monkeypatching, and testing async code.
- [ ] How do you talk to PostgreSQL from Python: drivers, SQLAlchemy core vs. ORM, async drivers, and connection pooling.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Profiling a slow Python service: `cProfile`, sampling profilers, line profiling — and the usual culprits you find.
- [ ] Memory: why a Python process keeps growing, reference cycles, `tracemalloc`, and processing a 20 GB file in constant memory.
- [ ] When is Python too slow, and what are the options (vectorise with NumPy/pandas/Polars, move to a compiled extension, use another service)?
- [ ] How do you run an async FastAPI service in production: worker count, event-loop blocking, graceful shutdown and connection limits?
- [ ] Structuring a medium-size Python codebase: packages, layering, avoiding circular imports, and keeping domain logic framework-free.
- [ ] Making LLM calls robust in Python: concurrency limits with semaphores, retries with backoff, timeouts, streaming, and cost tracking.
- [ ] Dependency and supply-chain hygiene: pinning, hashes, private indexes, and keeping heavy ML dependencies out of slim service images.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Write an async worker that pulls jobs from a queue, calls an external API with at most 20 concurrent requests, retries failures and stops cleanly on SIGTERM.
- [ ] Build a small FastAPI service exposing a tool that an LLM agent can call, with pydantic input/output schemas and validation errors the model can recover from.
- [ ] A batch job enriches 5M records with an LLM and takes three days. How do you make it faster and cheaper?
- [ ] An async endpoint has good average latency but terrible p99. What would you look for?
- [ ] Design the ingestion pipeline for a RAG system in Python: parsing, chunking, embedding in batches, idempotent re-runs.

## E. Hands-on checks (things to be able to DO)
- [ ] Solve a typical coding question idiomatically using `collections` (`Counter`, `defaultdict`, `deque`), `heapq`, `bisect` and `itertools`.
- [ ] Implement an LRU cache by hand and with `functools.lru_cache`, and explain the trade-off.
- [ ] Write a `pyproject.toml` for a service with dev dependencies, a CLI entry point and type-checker/linter config.
- [ ] Write a pytest suite with a fixture that spins up a test database and a parametrised test.
- [ ] Write a generator pipeline that streams and transforms a large CSV without loading it.

---

**Good answers show:**
- Idiomatic Python (comprehensions, generators, context managers, the standard library) rather than another language written in Python syntax.
- A correct model of concurrency: GIL, asyncio, threads and processes matched to the workload.
- Types and pydantic models used at boundaries, especially for LLM inputs and outputs.
- Production concerns — profiling, memory, packaging, graceful shutdown — not just scripts that work once.
- Pragmatism about when Python is the wrong tool and what to do instead.

**Prove it:** Build a FastAPI service with an async worker that runs an LLM-backed extraction over a few thousand documents with bounded concurrency, retries and pydantic-validated structured output; add pytest coverage and a short profiling write-up showing one optimisation.

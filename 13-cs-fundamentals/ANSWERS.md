# 13 — CS Fundamentals (Rapid-Fire): Model Answers

Questions: [README.md](README.md)

**How to use these answers:** These are reference answers. Aim to deliver each in 30–60 seconds, then stop and let the interviewer pull on a thread. Say them out loud, and tie them to systems you have actually built where you can.

---

## Networking

### What happens when you type a URL into a browser and press Enter?
1. **Parse URL** — scheme, host, port, path. Check HSTS list (force HTTPS).
2. **DNS** — browser cache → OS cache/hosts → recursive resolver → root → TLD → authoritative server → IP.
3. **Connect** — TCP three-way handshake (or QUIC over UDP for HTTP/3), then TLS handshake (cert verification, key exchange).
4. **HTTP request** — `GET /path` with headers and cookies. It may hit a CDN edge, a load balancer, a reverse proxy, then the app server, which may call caches/DBs.
5. **Response** — status, headers, body (possibly compressed, cached per `Cache-Control`). Redirects repeat the loop.
6. **Render** — parse HTML → DOM, CSS → CSSOM, run JS, fetch sub-resources (often over the same connection), layout, paint, composite.

Good follow-up hooks: connection reuse (keep-alive), caching layers, where latency goes (DNS + TCP + TLS RTTs).

### TCP vs. UDP.
| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (handshake) | Connectionless |
| Reliability | Ordered, retransmits, acks | Best-effort; loss/reorder possible |
| Control | Flow + congestion control | None (app decides) |
| Overhead | Higher header, latency from handshake/head-of-line blocking | 8-byte header, low latency |
| Model | Byte stream | Discrete datagrams |

Use TCP for HTTP/1–2, DBs, SSH, anything needing correctness. Use UDP for DNS, VoIP, video, games, and as a base for custom protocols like QUIC that implement their own reliability.

### TCP three-way handshake.
1. Client → **SYN** (seq = x)
2. Server → **SYN-ACK** (seq = y, ack = x+1)
3. Client → **ACK** (ack = y+1); connection ESTABLISHED.

It synchronizes initial sequence numbers in both directions and confirms both sides can send and receive. Costs one RTT before data flows (TLS 1.3 adds another; TCP Fast Open / QUIC 0-RTT reduce this). Teardown is a four-way FIN/ACK exchange; the side that closes first sits in `TIME_WAIT` (2×MSL), which is why busy clients can exhaust ephemeral ports. SYN floods abuse half-open connections; SYN cookies mitigate.

### HTTP/1.1 vs. HTTP/2 vs. HTTP/3 (QUIC).
- **HTTP/1.1** — text protocol, keep-alive, one outstanding request per connection (pipelining unused), so browsers open ~6 connections per host. Head-of-line (HOL) blocking at the HTTP layer.
- **HTTP/2** — binary framing, **multiplexed streams** over one TCP connection, HPACK header compression, stream priorities. Fixes HTTP-level HOL but a single lost TCP packet still stalls every stream (TCP HOL).
- **HTTP/3** — runs over **QUIC** (UDP). Streams are independent at the transport layer (no TCP HOL), TLS 1.3 built in, 1-RTT / 0-RTT setup, connection migration across networks (connection IDs, not 4-tuples). QPACK header compression.

Practical: HTTP/2 is the default for gRPC; HTTP/3 helps most on lossy mobile networks.

### HTTPS / TLS — how does it work?
TLS gives **confidentiality, integrity, and server authentication** on top of TCP. TLS 1.3 handshake (1 RTT):
1. **ClientHello** — supported ciphers, an ephemeral (EC)DHE key share, SNI (hostname).
2. **ServerHello** — chosen cipher + its key share. Both derive the same shared secret (Diffie–Hellman) → symmetric session keys. Forward secrecy because keys are ephemeral.
3. Server sends its **certificate** and a signature proving it owns the private key. Client validates the chain up to a trusted root CA, checks hostname and expiry.
4. **Finished** messages; application data flows encrypted with a symmetric AEAD cipher (AES-GCM, ChaCha20-Poly1305).

Asymmetric crypto is used only for authentication/key exchange; bulk data uses fast symmetric crypto. mTLS adds a client certificate for service-to-service auth.

### DNS resolution — how does it work? What are A, CNAME, MX, TXT records?
Client asks a **recursive resolver** (ISP, 8.8.8.8, VPC resolver). On cache miss it queries **root** → **TLD** (`.com`) → the domain's **authoritative** nameserver, then caches the answer for its **TTL**. Clients and OSes cache too, which is why DNS changes "propagate" slowly — lower TTLs before a migration.

- **A / AAAA** — name → IPv4 / IPv6 address.
- **CNAME** — alias to another name (`www` → `app.herokudns.com`). Cannot sit at the zone apex alongside other records (use ALIAS/ANAME).
- **MX** — mail servers for the domain, with priority.
- **TXT** — arbitrary text: SPF, DKIM, DMARC, domain-ownership verification.
- Also: **NS** (delegation), **SOA**, **SRV**, **PTR** (reverse lookup).

### OSI model / TCP-IP layers.
OSI (7): Physical, Data Link, Network, Transport, Session, Presentation, Application. The practical TCP/IP model (4):
- **Link** — Ethernet, Wi-Fi, MAC addresses, ARP.
- **Internet** — IP, routing, ICMP.
- **Transport** — TCP, UDP (ports).
- **Application** — HTTP, DNS, TLS (roughly OSI 5–7), SSH.

Why it matters: "L4 vs L7 load balancer", "L3 network ACL", and debugging layer by layer (can I ping? can I open the port with `nc`? does TLS handshake with `openssl s_client`? does `curl` return 200?).

### Load balancers: L4 vs. L7; algorithms (round robin, least connections, consistent hashing).
- **L4** — routes on IP/port (TCP/UDP) without reading payload. Fast, protocol-agnostic, can pass TLS through. E.g., AWS NLB.
- **L7** — terminates HTTP(S), routes on host/path/headers/cookies, can do TLS termination, retries, auth, rewrites, gRPC per-request balancing. E.g., AWS ALB, Nginx, Envoy.

Algorithms:
- **Round robin** (optionally weighted) — simple; good when requests are uniform.
- **Least connections / least outstanding requests** — better for variable request cost or long-lived connections.
- **Consistent hashing** (on client IP, user ID, key) — stickiness and cache affinity; minimal reshuffling when backends change.
- Also: random-of-two-choices ("power of two"), latency-aware (EWMA).

Plus health checks, connection draining, and avoiding sticky sessions where possible.

### Reverse proxy vs. forward proxy.
- **Forward proxy** — sits in front of **clients**; the server sees the proxy. Used for egress control, content filtering, caching, anonymity (corporate proxies, Squid). Client is configured to use it.
- **Reverse proxy** — sits in front of **servers**; the client thinks it is the origin. Used for TLS termination, load balancing, caching, compression, auth, rate limiting, hiding internal topology (Nginx, Envoy, HAProxy, Cloudflare).

One-liner: forward proxy acts on behalf of the client, reverse proxy acts on behalf of the server. FDE relevance: enterprise customers often force outbound traffic through a forward proxy with TLS inspection, which breaks naive HTTP clients (need `HTTPS_PROXY` + their CA bundle).

### CDN — how and why?
A CDN is a globally distributed network of edge caches (PoPs). DNS or anycast routes users to the nearest edge; on a cache hit content is served locally, on a miss the edge fetches from the **origin** (often via a mid-tier "origin shield") and caches it per `Cache-Control` / TTL.

**Why:** lower latency (shorter RTT, TLS terminated near the user), offloads origin bandwidth and load, absorbs DDoS, improves availability.
**What:** static assets, images, video segments; also dynamic acceleration and edge compute (Cloudflare Workers, Lambda@Edge).
**Gotchas:** invalidation (use versioned/hashed filenames instead of purges), cache keys (don't vary on cookies accidentally), never cache personalized responses publicly.

### Cookies vs. sessions vs. tokens.
- **Cookie** — a transport mechanism: small key/value the browser stores and sends automatically to the matching domain. Flags: `HttpOnly`, `Secure`, `SameSite`, expiry.
- **Server-side session** — server stores session state (Redis/DB); the cookie holds only an opaque session ID. Easy revocation; needs shared session store when scaling.
- **Token (e.g., JWT)** — signed, self-contained claims, typically sent in `Authorization: Bearer`. Stateless verification, good for APIs/mobile/service-to-service; hard to revoke before expiry, so use short-lived access tokens + refresh tokens.

Security: cookies are vulnerable to CSRF (mitigate with `SameSite` + CSRF tokens); tokens in `localStorage` are exposed to XSS. A common web pattern is a token in an `HttpOnly` cookie.

### WebSockets — how do they work?
1. Client sends an HTTP/1.1 request with `Upgrade: websocket`, `Connection: Upgrade`, and `Sec-WebSocket-Key`.
2. Server responds `101 Switching Protocols` with `Sec-WebSocket-Accept`.
3. The same TCP connection now carries **full-duplex, framed messages** (text/binary, ping/pong, close) in both directions with low overhead.

Use for chat, live dashboards, collaborative editing, streaming. Operational concerns: long-lived connections pin to a server (need sticky routing or a pub/sub backplane like Redis to fan out), LB/proxy idle timeouts (send heartbeats), reconnect with backoff, auth at handshake. Alternatives: **SSE** (server→client only, simpler, works over plain HTTP — common for LLM token streaming) and long polling.

### NAT, firewalls, ports.
- **Ports** — 16-bit numbers identifying a process endpoint on a host; a connection is the 5-tuple (src IP, src port, dst IP, dst port, protocol). Well-known: 22 SSH, 53 DNS, 80/443 HTTP(S), 5432 Postgres, 6379 Redis.
- **NAT** — rewrites private addresses (10.x, 192.168.x) to a public IP, tracking mappings by port. Lets many hosts share one IPv4 address; inbound connections are blocked unless port-forwarded. Cloud NAT gateways give private subnets outbound internet.
- **Firewalls** — allow/deny traffic by IP, port, protocol (stateless ACLs vs. stateful security groups that auto-allow return traffic); L7/WAF firewalls inspect HTTP.

FDE angle: "it works locally but not in the customer VPC" is usually a security group, NACL, missing NAT/egress rule, or IP allow-list.

### IPv4 vs. IPv6; CIDR notation.
- **IPv4** — 32-bit (`192.168.1.10`), ~4.3B addresses, exhausted, hence NAT.
- **IPv6** — 128-bit (`2001:db8::1`), effectively unlimited, no NAT needed, built-in autoconfiguration (SLAAC), simpler header. Dual-stack deployments are common.

**CIDR** `a.b.c.d/n` — first *n* bits are the network prefix; the rest are hosts. `/24` = 256 addresses, `/16` = 65,536, `/32` = one host, `0.0.0.0/0` = everything. `10.0.0.0/16` contains `10.0.5.0/24`. In AWS, a subnet loses 5 addresses to reservations. Useful for planning VPCs and avoiding overlapping ranges when peering with a customer network.

## Operating systems

### Process vs. thread.
- **Process** — an independent program instance with its **own address space**, file descriptors, and resources. Isolated: a crash doesn't take down others. Communicate via IPC (pipes, sockets, shared memory). Higher creation and context-switch cost.
- **Thread** — a unit of execution **inside** a process. Threads share the heap, globals, and file descriptors, but each has its own stack, registers, and program counter. Cheap to create and communicate, but shared memory means races and one bad thread can crash the whole process.

Rule of thumb: processes for isolation and CPU parallelism in Python; threads for shared-state concurrency and I/O.

### Concurrency vs. parallelism.
- **Concurrency** — structuring a program to *deal with* many tasks at once; tasks make progress in overlapping time periods, possibly interleaved on one core (event loop, threads).
- **Parallelism** — actually *executing* multiple tasks at the same instant on multiple cores/machines.

Concurrency is about design and handling waiting (I/O); parallelism is about throughput on compute. You can have concurrency without parallelism (asyncio on one core) and parallelism without much concurrency design (SIMD, a map over a process pool). Rob Pike's line: "Concurrency is about dealing with lots of things at once; parallelism is about doing lots of things at once."

### Context switching.
The OS saving the state of the running thread/process (registers, program counter, stack pointer) and restoring another's so the CPU can run it. Triggered by timer interrupts (preemption), blocking I/O, syscalls, or yields.

**Cost:** direct (save/restore, kernel entry, scheduler) plus indirect — cache and TLB pollution. Process switches are costlier than thread switches because the address space changes (page table switch, TLB flush unless tagged).

Why it matters: thousands of OS threads spend lots of time switching; that's why event loops (asyncio, Node, epoll-based servers) and green threads/goroutines scale better for I/O-heavy workloads. Monitor with `vmstat` (`cs` column) or `pidstat -w`.

### Virtual memory, paging.
Each process sees its own contiguous **virtual address space**; the MMU translates virtual → physical addresses using **page tables**, with a **TLB** caching recent translations. Memory is managed in fixed-size **pages** (typically 4 KB; huge pages 2 MB/1 GB).

Benefits: isolation between processes, programs larger than RAM, shared libraries mapped once, copy-on-write `fork`, memory-mapped files.

**Page fault** — access to a page not in RAM: the OS loads it from disk/swap (major fault) or maps it (minor fault). Heavy swapping = **thrashing**. Linux overcommits memory; when it truly runs out, the **OOM killer** terminates a process — common in containers hitting their cgroup memory limit (exit code 137).

### Stack vs. heap.
- **Stack** — per-thread, LIFO, holds function call frames: local variables, return addresses, arguments. Allocation is just moving the stack pointer — very fast, freed automatically on return. Limited size (often 1–8 MB) → deep recursion gives **stack overflow** (Python raises `RecursionError` at ~1000 frames by default).
- **Heap** — shared by all threads in the process, for dynamically allocated objects whose lifetime outlives a call. Slower allocation, can fragment, managed manually (C `malloc/free`) or by a GC.

In Python, essentially all objects live on the heap; the C stack holds interpreter frames that reference them.

### What is a deadlock? Four conditions? How to prevent?
A set of threads each waiting for a resource held by another, so none can proceed. E.g., T1 holds lock A and wants B; T2 holds B and wants A.

**Coffman conditions** (all four must hold):
1. **Mutual exclusion** — resource held exclusively.
2. **Hold and wait** — holding one while waiting for another.
3. **No preemption** — resources can't be forcibly taken.
4. **Circular wait** — a cycle in the wait-for graph.

**Prevention** — break one: **global lock ordering** (most practical; kills circular wait), acquire all locks at once, `try_lock` with **timeouts** and back off, use lock-free structures or a single owner (actor/queue). Databases detect deadlocks via wait-for graphs and abort a victim — so app code must retry.

### Mutex vs. semaphore vs. lock vs. condition variable.
- **Lock / mutex** — mutual exclusion: only one thread in the critical section at a time. A mutex has **ownership** (the thread that locks must unlock). "Lock" is the generic term; `threading.Lock` in Python. `RLock` is reentrant.
- **Semaphore** — a counter allowing up to **N** concurrent holders; `acquire` decrements, `release` increments, blocks at 0. No ownership. Used to cap concurrency (e.g., max 10 DB connections or API calls). Binary semaphore ≈ mutex without ownership.
- **Condition variable** — lets a thread **wait until a condition becomes true**, releasing the associated lock while sleeping; others `notify` after changing state. Always wait in a `while` loop (spurious wakeups).

```python
with cond:
    while not queue:
        cond.wait()
    item = queue.pop()
```
Also: read-write locks (many readers or one writer).

### Race conditions — example and fix.
A race condition is when correctness depends on the timing/interleaving of concurrent operations. Classic: read-modify-write.

```python
import threading
count = 0
lock = threading.Lock()

def incr():
    global count
    for _ in range(100_000):
        with lock:          # without the lock, count += 1 (load, add, store) can interleave
            count += 1
```
Without the lock, two threads both read 5 and both write 6 — an update is lost.

**Fixes:** locks around the critical section; atomic operations (DB `UPDATE t SET n = n + 1`, Redis `INCR`); optimistic concurrency (version column / compare-and-swap); avoid shared mutable state (queues, actors, immutability). Distributed version: check-then-act across services — fix with unique constraints, idempotency keys, or conditional writes.

### Python GIL — what is it and how does it affect threading?
The **Global Interpreter Lock** in CPython is a mutex that allows only one thread to execute Python bytecode at a time. It keeps reference counting and interpreter internals simple and makes single-threaded code fast.

Effects:
- **CPU-bound** pure-Python code gets **no speedup** from threads (often slower due to contention).
- **I/O-bound** code benefits: the GIL is released during blocking I/O and `time.sleep`, so threads overlap waiting.
- C extensions (NumPy, many ML libs) release the GIL during heavy work, so threads can parallelize those.

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
def cpu(n): return sum(i*i for i in range(n))
# ThreadPoolExecutor(4).map(cpu, [10**7]*4)   -> ~same time as serial
# ProcessPoolExecutor(4).map(cpu, [10**7]*4)  -> ~4x faster on 4 cores
```
For CPU parallelism use `multiprocessing`/`ProcessPoolExecutor`. Note: Python 3.13+ has an experimental free-threaded build (PEP 703) without the GIL.

### async/await vs. threads vs. multiprocessing — when to use each.
- **asyncio (async/await)** — single thread, cooperative multitasking on an event loop. Best for **high-concurrency I/O** (thousands of HTTP calls, websockets, LLM API fan-out). Very cheap per task. Pitfall: any blocking call (`requests`, `time.sleep`, CPU work) stalls everything — use async libs (`httpx`, `asyncpg`) or `asyncio.to_thread`.
- **Threads** — preemptive, shared memory. Good for **moderate I/O concurrency** with blocking libraries (boto3, `requests`, DB drivers) and simpler code. GIL limits CPU work; need locks for shared state.
- **Multiprocessing** — separate processes, true **CPU parallelism**, isolation. Costs: memory, startup, pickling data across processes. Use for CPU-bound work (parsing, image processing, pure-Python compute).

Combine them: an async web server that offloads CPU jobs to a process pool or a task queue (Celery, RQ).

### File descriptors; what happens with "too many open files"?
A **file descriptor** is a per-process integer handle to an open kernel object: file, socket, pipe, epoll instance. 0/1/2 are stdin/stdout/stderr. Every TCP connection consumes one.

`EMFILE: Too many open files` means the process hit its limit (`ulimit -n`, often 1024 by default; system-wide `fs.file-max`). Usually caused by a **leak** — files or sockets not closed, HTTP clients created per request without a shared session/pool, connections not returned — or genuinely high concurrency.

Debug: `ls /proc/<pid>/fd | wc -l`, `lsof -p <pid>`. Fix: close resources (`with` blocks), reuse clients/connection pools, then raise limits (`ulimit`, systemd `LimitNOFILE`, container `nofile`) if load is real.

### Signals (SIGTERM vs. SIGKILL); graceful shutdown.
Signals are async notifications to a process.
- **SIGTERM (15)** — polite request to terminate; can be caught. Default from `kill`, Kubernetes, Docker stop.
- **SIGKILL (9)** — kernel kills immediately; cannot be caught or ignored. No cleanup.
- **SIGINT (2)** — Ctrl-C. **SIGHUP** — often "reload config".

**Graceful shutdown:** on SIGTERM, stop accepting new work (fail readiness probe, close listener), finish or checkpoint in-flight requests/jobs, flush logs/metrics, close DB connections, exit 0 — within the grace period (K8s default 30s, then SIGKILL).

```python
import signal, sys
def handle(sig, frame):
    server.stop_accepting(); drain(); sys.exit(0)
signal.signal(signal.SIGTERM, handle)
```
Gotcha: in Docker, if PID 1 is a shell wrapper it may not forward signals — use `exec` or `tini`.

### Garbage collection basics.
Automatic reclamation of memory no longer reachable from roots (stack, globals, registers).
- **Reference counting** — free an object when its count hits 0. Immediate and predictable, but can't collect **cycles**. CPython uses this as the primary mechanism.
- **Tracing (mark-and-sweep)** — mark everything reachable from roots, sweep the rest. Handles cycles. Variants: mark-compact, copying.
- **Generational** — most objects die young, so collect the young generation often and old generations rarely. CPython's `gc` module is a generational cycle collector on top of refcounting; the JVM, .NET and V8 are generational tracers.

Trade-offs: pause times (stop-the-world vs. concurrent/incremental collectors), throughput, memory overhead. "Memory leaks" in GC'd languages are usually unintended references: growing caches, global lists, closures, event listeners.

## Databases

### SQL vs. NoSQL — when to use each.
**SQL (Postgres, MySQL):** structured, relational data; joins and ad-hoc queries; strong consistency and multi-row ACID transactions; schema enforces integrity. Default choice for most business apps — Postgres also covers JSON (`jsonb`), full-text, and vectors (`pgvector`).

**NoSQL:** when a specific access pattern or scale dominates:
- Massive write throughput / horizontal scale with known key-based access (DynamoDB, Cassandra).
- Flexible, nested, evolving documents (MongoDB).
- Low-latency caching/ephemeral data (Redis).
- Graph traversals (Neo4j), time series, search (Elasticsearch).

Trade-off: NoSQL usually gives up joins and flexible queries, so you **design tables around access patterns** up front. In an interview: "Start with Postgres unless there's a concrete reason — scale, schema, or access pattern — it can't handle."

### Types of NoSQL (key-value, document, column-family, graph).
- **Key-value** — opaque value by key; O(1) get/put. Redis, DynamoDB (also document-ish), Memcached. Use: caching, sessions, counters.
- **Document** — JSON/BSON documents with nested fields, secondary indexes, query by field. MongoDB, Firestore, Couchbase. Use: content, catalogs, user profiles with varying shape.
- **Column-family (wide-column)** — rows keyed by partition key, each holding many sorted columns; built for huge write volume across nodes. Cassandra, HBase, Bigtable, ScyllaDB. Use: time series, event logs, IoT.
- **Graph** — nodes and edges with properties; efficient multi-hop traversals. Neo4j, Neptune. Use: social graphs, fraud rings, recommendations, knowledge graphs.

Also: time-series (InfluxDB, TimescaleDB), search (Elasticsearch/OpenSearch), vector (Pinecone, Weaviate).

### ACID vs. BASE.
**ACID** (traditional RDBMS transactions):
- **Atomicity** — all or nothing.
- **Consistency** — constraints/invariants hold after each transaction.
- **Isolation** — concurrent transactions behave as if serialized (to a configured level: read committed, repeatable read, serializable).
- **Durability** — committed data survives crashes (WAL, fsync).

**BASE** (many distributed NoSQL systems):
- **Basically Available** — prioritize responding.
- **Soft state** — state may change without input as replicas converge.
- **Eventually consistent** — replicas agree given time without new writes.

ACID favors correctness (payments, inventory); BASE favors availability and scale (feeds, analytics, carts). It's a spectrum — DynamoDB and MongoDB now offer transactions, and Spanner/CockroachDB offer distributed ACID.

### CAP theorem; PACELC.
**CAP:** in the presence of a network **Partition**, a distributed system must choose between **Consistency** (every read sees the latest write — linearizability) and **Availability** (every request to a non-failed node gets a response). P isn't optional in real networks, so it's really **CP vs AP during a partition**.
- CP: etcd, ZooKeeper, HBase, Spanner — refuse/err on minority side.
- AP: Cassandra, DynamoDB (default), Riak — answer, maybe stale.

**PACELC** extends it: if **P**artition → choose **A** or **C**; **E**lse (normal operation) → choose **L**atency or **C**onsistency. Captures the everyday trade-off: synchronous replication costs latency. Cassandra/Dynamo = PA/EL; Spanner = PC/EC.

### Indexing internals (B-tree, LSM-tree).
- **B-tree / B+tree** — balanced, high-fanout tree of pages; leaves hold sorted keys (and in B+trees, link to neighbours for range scans). O(log n) lookups with ~3–4 page reads for millions of rows. Updates are in place (plus WAL). **Read-optimized**; random writes cost random I/O. Used by Postgres, MySQL InnoDB, SQLite.
- **LSM-tree** — writes go to a WAL + in-memory **memtable**, flushed as immutable sorted **SSTables**; background **compaction** merges them. Very high write throughput (sequential I/O). Reads may check several SSTables — mitigated with **Bloom filters** and compaction. Write/read/space amplification trade-offs. Used by Cassandra, RocksDB, LevelDB, ScyllaDB.

Index trade-offs generally: faster reads, slower writes, extra storage. Composite index column order matters (leftmost prefix); covering indexes avoid table lookups. Check with `EXPLAIN ANALYZE`.

### Replication: leader–follower, multi-leader, leaderless.
- **Leader–follower (primary–replica)** — all writes go to one leader, which streams its log to followers; reads can go to followers. Simple, no write conflicts. Issues: replication lag (stale reads, read-your-writes), failover complexity. Sync vs. async replication trades durability for latency. Postgres, MySQL, MongoDB.
- **Multi-leader** — several nodes accept writes (often one per region) and replicate to each other. Better write latency and regional availability, but **write conflicts** need resolution (last-write-wins, CRDTs, app logic). Used for multi-region and offline clients.
- **Leaderless** — clients write/read to multiple replicas; **quorums** (W + R > N) give overlap. Read repair and anti-entropy fix divergence. Highly available. Dynamo-style: Cassandra, Riak.

### Sharding / partitioning strategies; hot partitions.
Split data across nodes so each holds a subset.
- **Range-based** — by key range (dates, A–M). Efficient range scans; prone to hotspots (all new writes hit the latest range).
- **Hash-based** — `hash(key) % N` or consistent hashing. Even distribution; loses range queries; naive modulo reshuffles everything when N changes.
- **Directory/lookup-based** — a mapping service decides placement. Flexible (move a tenant), but an extra component.
- **Geo / tenant-based** — by region or customer — useful for data residency.

**Hot partitions** — one key gets disproportionate traffic (celebrity user, big tenant, today's date). Fixes: better key choice (high cardinality), **salting** (append a random suffix and fan out reads), splitting hot shards, caching hot keys, write sharding for counters. Cross-shard joins and transactions become hard — pick the shard key from the dominant access pattern.

### Consistent hashing.
Map both nodes and keys onto a hash **ring** (e.g., 0…2³²). A key is owned by the first node clockwise from its hash. When a node is added or removed, only the keys between it and its neighbour move — about **K/N** keys instead of nearly all of them with `hash % N`.

**Virtual nodes** — each physical node gets many points on the ring, smoothing load and letting stronger machines take more vnodes.

```python
import bisect, hashlib
class Ring:
    def __init__(self, nodes, vnodes=100):
        self.ring = sorted((self._h(f"{n}#{i}"), n) for n in nodes for i in range(vnodes))
        self.keys = [h for h, _ in self.ring]
    def _h(self, s): return int(hashlib.md5(s.encode()).hexdigest(), 16)
    def get(self, key):
        i = bisect.bisect(self.keys, self._h(key)) % len(self.ring)
        return self.ring[i][1]
```
Lookup O(log V). Used in DynamoDB/Cassandra partitioning, memcached clients, load balancers. Alternative: rendezvous (HRW) hashing.

### Optimistic vs. pessimistic locking.
- **Pessimistic** — lock the row before modifying so others wait: `SELECT ... FOR UPDATE`. Safe under high contention; costs throughput and risks deadlocks/long waits.
- **Optimistic** — don't lock; detect conflicts at write time using a **version** column or timestamp, retry if it changed.

```sql
UPDATE accounts SET balance = :new, version = version + 1
WHERE id = :id AND version = :expected_version;
-- 0 rows updated => someone else won; re-read and retry
```
Optimistic is better when conflicts are rare and transactions are short or span user think-time (web forms, REST `ETag`/`If-Match`). Pessimistic is better for hot rows with frequent conflicts (inventory decrement, seat booking) where retries would thrash. DynamoDB conditional writes and Redis `WATCH` are optimistic.

### Connection pooling.
Opening a DB connection is expensive (TCP + TLS + auth + server process/thread, e.g., Postgres forks a backend per connection). A **pool** keeps a set of open connections and lends them to requests, returning them afterwards.

Benefits: lower latency, bounded load on the DB, protection from connection storms. Key settings: min/max size, acquire timeout, max lifetime/idle timeout, health check on checkout.

Pitfalls: pool size × app instances can exceed `max_connections` (common with autoscaling or serverless — use **PgBouncer** or RDS Proxy); leaked connections exhaust the pool (always release in `finally`/`with`); bigger isn't better — DB throughput peaks around a small multiple of CPU cores. Examples: SQLAlchemy `QueuePool`, HikariCP, `asyncpg.create_pool`.

### Caching strategies: cache-aside, write-through, write-back; cache invalidation; eviction (LRU, LFU).
**Strategies:**
- **Cache-aside (lazy loading)** — app reads cache; on miss reads DB and populates cache. On write, update DB then **delete** the cache key. Most common; cache only holds what's used.
- **Write-through** — writes go to cache and DB synchronously. Cache always fresh; slower writes, caches data that may never be read.
- **Write-back (write-behind)** — write to cache, flush to DB asynchronously. Fast writes; risk of data loss on cache failure.
- (Read-through: the cache itself loads from DB on miss.)

**Invalidation:** TTLs as a safety net, explicit delete on write, event-driven invalidation (CDC), versioned keys. Watch for **stampedes** (many misses at once — use locks/single-flight, jittered TTLs, early refresh) and stale races between write and populate.

**Eviction:** **LRU** (evict least recently used — good default), **LFU** (least frequently used — better for stable popularity), FIFO, TTL. Redis supports `allkeys-lru`, `allkeys-lfu`, `volatile-ttl`, etc.

### Redis use cases.
In-memory data-structure server, single-threaded command execution (atomic ops), optional persistence (RDB snapshots, AOF).
- **Caching** — query results, rendered pages, LLM responses; TTLs + eviction.
- **Sessions** and short-lived tokens.
- **Rate limiting** — `INCR` + `EXPIRE`, or sorted sets for sliding windows, or Lua scripts for token bucket.
- **Distributed locks** — `SET key val NX PX 30000` (with care; fencing tokens for correctness).
- **Queues / streams** — lists (`LPUSH`/`BRPOP`), Redis Streams with consumer groups; backs Celery/RQ/Sidekiq.
- **Pub/Sub** — fan-out for websockets across servers.
- **Leaderboards** — sorted sets (`ZADD`, `ZRANGE`).
- **Counters, sets, HyperLogLog** (unique counts), geospatial, and vector search (Redis Stack).

Not a primary system of record for critical data unless persistence and replication are configured deliberately.

### Vector databases — how do they differ from traditional DBs?
They store **embeddings** (high-dimensional float vectors) and answer **similarity queries** — "top-k nearest to this vector" by cosine/dot/L2 — rather than exact matches or ranges.

Differences:
- **Indexing** — Approximate Nearest Neighbour (ANN) structures: **HNSW** (graph), **IVF** (clustering), **PQ** (compression). Trade recall for speed and memory; exact search is O(n·d).
- **Results are ranked and approximate**, not exact sets. Tuning knobs: `ef`, `nprobe`, recall@k.
- **Hybrid queries** — vector similarity + metadata filters (tenant, date) + keyword (BM25) for hybrid search.
- Weaker on transactions, joins, and complex updates.

Options: dedicated (Pinecone, Weaviate, Qdrant, Milvus) vs. extensions (pgvector, OpenSearch, Redis). FDE take: for most RAG apps under a few million vectors, **pgvector** keeps data, ACLs, and metadata in one place; reach for a dedicated DB at large scale or for advanced features.

## Distributed systems

### Strong vs. eventual consistency.
- **Strong (linearizable)** — once a write is acknowledged, every subsequent read anywhere sees it; the system behaves like a single copy. Requires coordination (synchronous replication, consensus), costing latency and availability during partitions. Needed for balances, inventory, uniqueness, locks.
- **Eventual** — replicas may return stale data, but converge if writes stop. Low latency, highly available. Fine for feeds, likes, analytics, search indexes.

In between: **read-your-writes**, **monotonic reads**, **causal** consistency, bounded staleness. Practical design: strong consistency for the core invariant, eventual for derived views; route a user's reads to the leader right after they write; make the UI tolerate staleness.

### Idempotency and exactly-once processing.
**Idempotent** = applying an operation multiple times has the same effect as once (`PUT`, `DELETE`, `SET x=5`; not `x += 1` or "charge card").

Networks force retries, so delivery is **at-least-once** in practice. True exactly-once *delivery* is impossible across arbitrary failures; what you build is **exactly-once effect** = at-least-once delivery + idempotent processing:
- **Idempotency keys** — client sends a unique key; server stores `key → result` (with a unique constraint) and returns the stored result on repeats (Stripe's pattern).
- **Dedup tables** of processed message IDs, written in the same transaction as the side effect.
- **Conditional/upsert writes** and natural keys.
- Kafka transactions give exactly-once within Kafka read-process-write; external side effects still need idempotency.

### Consensus (Raft/Paxos) at a high level.
Consensus lets a group of nodes agree on a value / an ordered log despite crashes, as long as a **majority (quorum)** is alive — so 3 nodes tolerate 1 failure, 5 tolerate 2.

**Raft** (designed to be understandable):
- Nodes are **leader**, **follower**, or **candidate**. Time is divided into **terms**.
- **Leader election** — followers that miss heartbeats time out (randomized), become candidates, request votes; majority wins.
- **Log replication** — leader appends client commands, replicates to followers; an entry is **committed** once a majority stores it, then applied to the state machine.
- Safety — only candidates with up-to-date logs can win.

**Paxos** — the original; proposers/acceptors/learners, two phases (prepare/promise, accept/accepted); Multi-Paxos elects a stable leader. Same guarantees, harder to implement.

Used in etcd, Consul, ZooKeeper (ZAB), CockroachDB, Kafka KRaft. You rarely implement it — you use these systems for config, locks, and leader election.

### Leader election.
Choosing a single node to coordinate (run the scheduler, own a partition, be the DB primary) so work isn't duplicated.

Approaches:
- **Consensus-backed** — use etcd/ZooKeeper/Consul: create an ephemeral node or acquire a **lease** with TTL; the holder renews it; on failure the lease expires and another node takes over. Kubernetes `Lease` objects work this way.
- **Database-based** — row lock or `UPDATE ... WHERE lease_expires < now()`; Postgres advisory locks.
- **Bully / ring algorithms** — classic textbook ones; rarely used directly.

Key hazard: **split brain** — an old leader that paused (GC, network) still thinks it's leader. Mitigate with **fencing tokens** (monotonically increasing epoch numbers checked by the storage layer), quorum-based decisions, and lease durations longer than clock skew.

### Distributed transactions: 2PC, sagas, outbox pattern.
- **Two-phase commit (2PC)** — a coordinator asks all participants to **prepare** (vote yes/no, holding locks), then sends **commit** or **abort**. Atomic across resources but **blocking**: if the coordinator dies after prepare, participants hold locks. Slow; used within DBs (XA, Spanner) more than across microservices.
- **Saga** — a sequence of local transactions, each with a **compensating action** (refund, release reservation) run on failure. **Orchestrated** (central workflow, e.g., Temporal, Step Functions) or **choreographed** (services react to events). Gives eventual consistency, no global locks; you must design compensations and handle intermediate states.
- **Transactional outbox** — solves "update DB *and* publish event" atomically: write the business row and an `outbox` row in the **same local transaction**; a relay (poller or CDC like Debezium) publishes outbox rows to Kafka/SQS. Consumers must be idempotent (at-least-once).

### Back-pressure.
A mechanism for a slow consumer to signal producers to slow down, instead of letting queues grow unbounded until memory blows up or latency explodes.

Techniques:
- **Bounded queues/buffers** — producers block or get rejected when full (`asyncio.Queue(maxsize=100)`).
- **Pull-based consumption** — consumers fetch at their own pace (Kafka, SQS, Reactive Streams `request(n)`).
- **Rejecting/shedding load** — return 429/503 with `Retry-After`; drop low-priority work.
- **Concurrency limits** — semaphores, connection pools, adaptive limits.
- TCP flow control is back-pressure at the transport layer.

Signals you need it: growing queue depth/consumer lag, rising latency, OOMs. Pair with autoscaling consumers on lag.

### Rate-limiting algorithms: token bucket, leaky bucket, fixed window, sliding window log/counter.
- **Token bucket** — bucket of capacity *B* refills at *r* tokens/sec; each request takes a token or is rejected. Allows **bursts** up to *B* while enforcing average rate. Most common (AWS, Stripe).
- **Leaky bucket** — requests enter a queue that drains at a constant rate; overflow dropped. **Smooths** output; no bursts.
- **Fixed window** — count per calendar window (e.g., per minute). Simple (`INCR` + `EXPIRE`), but allows up to 2× at window boundaries.
- **Sliding window log** — store a timestamp per request; count those within the last *W* seconds. Exact, but O(requests) memory.
- **Sliding window counter** — weighted mix of current and previous window counts: `curr + prev × (1 − elapsed/W)`. Near-exact, O(1) memory.

```python
import time
class TokenBucket:
    def __init__(self, rate, capacity):
        self.rate, self.cap, self.tokens, self.t = rate, capacity, capacity, time.monotonic()
    def allow(self):
        now = time.monotonic()
        self.tokens = min(self.cap, self.tokens + (now - self.t) * self.rate)
        self.t = now
        if self.tokens >= 1:
            self.tokens -= 1; return True
        return False
```
Distributed: keep state in Redis with an atomic Lua script.

### Circuit breaker, bulkhead, retry with jitter.
- **Retry with exponential backoff + jitter** — on transient errors (timeouts, 429, 503), retry after `random(0, base × 2^attempt)` capped. Jitter prevents synchronized retry storms (thundering herd). Only retry idempotent operations; cap attempts; respect `Retry-After`; use a retry budget.
- **Circuit breaker** — wraps calls to a dependency. **Closed** (normal) → after an error threshold, **Open** (fail fast, no calls) → after a cooldown, **Half-open** (let a few trial calls through) → close on success. Protects a struggling dependency and frees your threads.
- **Bulkhead** — isolate resources per dependency or tenant (separate thread pools, connection pools, semaphores) so one slow dependency can't exhaust everything — like ship compartments.

Together with **timeouts on every call**, these prevent cascading failures. Libraries: `tenacity` (Python), resilience4j, Envoy/Istio.

### Monolith vs. microservices — trade-offs.
**Monolith** — one deployable.
- Pros: simple to develop, test, debug, deploy; in-process calls; easy transactions and refactors; one observability story.
- Cons: scaling the whole app together, large blast radius, slow builds as it grows, team coupling.

**Microservices** — independently deployable services owning their data.
- Pros: independent deploys and scaling, team autonomy, fault isolation, tech diversity.
- Cons: network latency and failures, distributed transactions, eventual consistency, versioning, much heavier ops (service discovery, tracing, CI/CD per service). "Distributed monolith" if boundaries are wrong.

Typical stance: start with a **modular monolith** with clear internal boundaries; extract services when there's a concrete driver (team scaling, divergent scaling needs, isolation). Conway's law — architecture follows org structure. For FDE work with small customer teams, fewer moving parts usually wins.

### Event-driven architecture; event sourcing; CQRS.
- **Event-driven architecture** — services communicate by publishing events ("OrderPlaced") to a broker (Kafka, SNS/SQS, EventBridge); consumers react asynchronously. Loose coupling, easy fan-out, buffering. Costs: eventual consistency, harder debugging/tracing, ordering and duplicate handling, schema evolution (use a schema registry).
- **Event sourcing** — store the **sequence of state-changing events** as the source of truth instead of current state; derive state by replaying (with snapshots). Full audit trail, time travel, rebuild new projections. Costs: complexity, event versioning, eventual consistency of views, GDPR deletion is harder.
- **CQRS** — separate the **write model** (commands, validation, normalized) from **read models** (denormalized, optimized per query, possibly different stores). Often paired with event sourcing: events update read projections.

Use them for domains that need audit/history or very different read/write scaling — not by default.

### Clock skew; logical clocks.
Physical clocks on different machines drift and NTP only bounds skew to roughly milliseconds (worse under load or VM pauses), so **wall-clock timestamps can't reliably order events** across nodes — e.g., last-write-wins can silently drop a newer write. Also use monotonic clocks for measuring durations.

**Logical clocks** order events by causality instead:
- **Lamport clock** — counter incremented per event; on receive set `max(local, received) + 1`. If A → B then L(A) < L(B) (not the converse).
- **Vector clocks** — one counter per node; can tell **happened-before** vs **concurrent**, enabling conflict detection (Dynamo, Riak).
- **Hybrid logical clocks (HLC)** — physical time + logical counter; used by CockroachDB, MongoDB.
- **Google TrueTime** — bounded uncertainty intervals; Spanner waits out the uncertainty for external consistency.

## Software engineering practices

### SOLID principles.
- **S — Single Responsibility** — a module/class should have one reason to change (separate parsing, business rules, persistence).
- **O — Open/Closed** — open for extension, closed for modification: add a new payment provider by adding a class, not editing a giant `if/elif`.
- **L — Liskov Substitution** — subtypes must be usable wherever the base type is, without surprising behaviour (the Square/Rectangle problem).
- **I — Interface Segregation** — many small, client-specific interfaces over one fat one (Python: small `Protocol`s).
- **D — Dependency Inversion** — depend on abstractions; inject dependencies (pass a `Storage` interface, not a hard-coded S3 client) — this is what makes code testable.

Pragmatic note: apply them to reduce coupling where change is likely; over-applying produces needless abstraction layers.

### Common design patterns (singleton, factory, strategy, observer, adapter, decorator).
- **Singleton** — exactly one instance (config, connection pool). In Python a module-level object suffices; overuse = hidden global state, hard to test.
- **Factory** — a function/class decides which concrete type to create (`get_llm_client("anthropic")`), hiding construction logic.
- **Strategy** — interchangeable algorithms behind one interface, chosen at runtime (chunking strategies, pricing rules). In Python often just passing a function.
- **Observer (pub/sub)** — subjects notify registered listeners of events (UI events, webhooks, signals).
- **Adapter** — wrap an incompatible interface to fit the one you expect (wrap a customer's legacy SOAP API behind your clean `CRMClient`). Very common in FDE integration work.
- **Decorator** — wrap an object/function to add behaviour (logging, caching, retries) without changing it. Python's `@decorator` syntax is a functional form.

Mention others if relevant: builder, repository, dependency injection, command.

### Unit vs. integration vs. end-to-end tests; mocking.
- **Unit** — one function/class in isolation; fast (ms), deterministic, many of them. Dependencies faked.
- **Integration** — several components together with real dependencies (real Postgres in Docker/testcontainers, real HTTP between modules). Catches wiring, SQL, serialization bugs.
- **End-to-end** — the whole system through its real interface (browser via Playwright, API against staging). Highest confidence, slowest, flakiest — keep few, covering critical journeys.

The **test pyramid**: many unit, fewer integration, few E2E.

**Mocking** — replace a dependency with a test double (stub returns canned data, mock also verifies calls, fake is a lightweight working implementation). Mock at **boundaries you don't own** (third-party APIs, LLMs, clock, randomness); avoid mocking your own internals heavily — tests then mirror implementation and break on refactor. Tools: `unittest.mock`, `pytest` fixtures, `responses`/`respx`, VCR-style recordings.

### TDD — pros and cons.
**TDD**: red (write a failing test) → green (minimal code to pass) → refactor.

Pros:
- Forces clarity on requirements and interface before implementation.
- Produces testable, loosely-coupled design and a regression suite by default.
- Small steps, fast feedback, confidence to refactor.
- Great for bug fixes: write the reproducing test first.

Cons:
- Slower up front; awkward for exploratory/prototype work where the design is unknown (common in early FDE engagements).
- Can lead to over-mocked tests tightly coupled to implementation.
- Hard for UI, ML/LLM outputs, and non-deterministic systems (use evals instead).

Pragmatic position: TDD for well-specified logic (parsers, pricing, data transforms) and bug fixes; spike first then backfill tests for exploratory work.

### Code review — what do you look for?
In rough priority order:
1. **Correctness** — does it do what the ticket says? Edge cases (empty, null, large inputs, concurrency, time zones), error handling, off-by-ones.
2. **Security** — injection, authz checks, secrets in code, PII in logs, unsafe deserialization.
3. **Design** — fits existing architecture, right abstraction level, no needless complexity, clear boundaries.
4. **Tests** — meaningful coverage of new behaviour and edge cases; tests would fail if the code were wrong.
5. **Performance & ops** — N+1 queries, unbounded loops/memory, missing timeouts/retries, logging/metrics, migrations safe for zero-downtime, rollback path.
6. **Readability** — naming, comments explaining *why*, consistency. Leave style to linters/formatters.

Process: small PRs, review promptly, be specific and kind, distinguish blocking issues from nits ("nit:"), ask questions rather than dictate, approve when it's better than before even if not perfect.

### Git: merge vs. rebase; resolving conflicts; branching strategies.
- **Merge** — combines branches with a merge commit; preserves true history; non-destructive; history can get noisy.
- **Rebase** — replays your commits on top of the target branch; linear history, cleaner `git log`/`bisect`. Rewrites commit hashes — **never rebase shared/public branches**. `git rebase -i` to squash/fixup before review. Many teams "squash and merge" PRs.

**Resolving conflicts:** `git status` to see conflicted files; edit the `<<<<<<< ======= >>>>>>>` hunks understanding *both* intents (not just picking a side); run tests; `git add` then `git merge --continue` / `git rebase --continue`. Reduce conflicts with small, short-lived branches and rebasing often. `git rerere` remembers resolutions.

**Branching strategies:**
- **Trunk-based** — short-lived branches merged to `main` daily, feature flags for incomplete work; best with good CI/CD.
- **GitHub Flow** — feature branch → PR → main → deploy.
- **GitFlow** — `develop`, `release/*`, `hotfix/*`; suits versioned, scheduled releases (on-prem customer installs), heavier overhead.

### Technical debt — how do you manage it?
Tech debt = shortcuts whose "interest" is slower future change, bugs, or incidents. Some is deliberate and fine (shipping a POC); the problem is unmanaged debt.

How to manage:
- **Make it visible** — track items in the backlog with impact ("adds 2 days to every integration", "caused 3 incidents"), not just "code is ugly". Leave `TODO(ticket-123)` with a link.
- **Prioritize by cost of delay** — interest rate × how often that area changes. Debt in code nobody touches can wait.
- **Pay continuously** — boy-scout rule, a fixed capacity share (~10–20%) per sprint, refactor as part of features that touch the area.
- **Prevent** — CI linting, tests, code review, ADRs documenting deliberate shortcuts and the trigger to revisit.
- **Communicate in business terms** to stakeholders: risk, velocity, reliability.

FDE angle: POC shortcuts must be documented and paid down before handing over to the customer.

### How do you write maintainable code that a customer team can take over?
Design for the team that will own it, not for you:
- **Match their stack and conventions** — their language, frameworks, CI, cloud, coding standards. Boring, mainstream tech over clever choices.
- **Simple, explicit structure** — clear module boundaries, small functions, descriptive names, minimal magic/metaprogramming, config over code.
- **Tests and CI** — a test suite they can run with one command; CI pipeline in their environment.
- **Documentation** — README (what it does, how to run locally, deploy, configure), architecture diagram, ADRs explaining *why*, runbook for common failures and on-call.
- **Operability** — structured logs, metrics, alerts, health checks, sane error messages; infrastructure as code (Terraform), secrets in their secret manager.
- **Reproducible environments** — pinned dependencies, Dockerfile, `.env.example`.
- **Knowledge transfer** — pair-program with their engineers during the build, have them make changes and run deploys before handover, record walkthroughs, define a support window.

Success metric: their team ships a change without you.

## Language-specific (Python)

### List vs. tuple vs. set vs. dict — complexity of common operations.
| Operation | list | tuple | set | dict |
|---|---|---|---|---|
| Index `x[i]` | O(1) | O(1) | — | — |
| Lookup `v in x` | O(n) | O(n) | O(1) avg | O(1) avg (keys) |
| Append / add | O(1) amortized | immutable | O(1) avg | O(1) avg |
| Insert/delete at front/middle | O(n) | — | — | — |
| Pop from end | O(1) | — | — | — |
| Delete by key/value | O(n) | — | O(1) avg | O(1) avg |
| Iterate | O(n) | O(n) | O(n) | O(n) |

- **list** — ordered, mutable dynamic array. Use `collections.deque` for O(1) pops from the left.
- **tuple** — ordered, immutable, hashable if contents are (usable as dict keys), slightly lighter.
- **set** — unordered unique hashable items; hash table; fast membership and set algebra.
- **dict** — hash map; insertion-ordered since 3.7. Worst case O(n) with pathological hash collisions.

Classic interview fix: replace `if x in some_list` inside a loop (O(n²)) with a set (O(n)).

### Mutable default arguments pitfall.
Default values are evaluated **once, at function definition time**, not per call. A mutable default (list, dict, set) is shared across calls.

```python
def add(item, bucket=[]):      # bug
    bucket.append(item)
    return bucket

add(1)  # [1]
add(2)  # [1, 2]  <- same list reused

def add(item, bucket=None):    # fix
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```
Same issue with `def f(ts=datetime.now())` — the timestamp is frozen at import. For dataclasses use `field(default_factory=list)`; Pydantic handles mutable defaults safely by copying.

### Generators and `yield`; iterators.
- **Iterator protocol** — an object with `__iter__()` returning itself and `__next__()` returning the next value or raising `StopIteration`. An **iterable** (list, dict, file) has `__iter__` returning a fresh iterator. `for` loops call these under the hood.
- **Generator** — a function containing `yield`; calling it returns a generator (an iterator) that runs lazily, pausing at each `yield` and keeping its local state.

```python
def read_batches(path, size=1000):
    batch = []
    with open(path) as f:
        for line in f:
            batch.append(line)
            if len(batch) == size:
                yield batch
                batch = []
    if batch:
        yield batch
```
Benefits: **O(1) memory** streaming of large files/API pages, composable pipelines, infinite sequences. Generator expressions: `sum(x*x for x in nums)`. Gotcha: a generator can be consumed only once. `yield from` delegates to a sub-generator.

### Decorators and context managers.
**Decorator** — a callable that takes a function and returns a wrapped one; `@d` above `def f` means `f = d(f)`. Used for logging, timing, retries, caching (`functools.lru_cache`), auth checks, route registration.

```python
import functools, time
def timed(fn):
    @functools.wraps(fn)                  # preserve name/docstring
    def wrapper(*args, **kwargs):
        t = time.perf_counter()
        try:
            return fn(*args, **kwargs)
        finally:
            print(f"{fn.__name__}: {time.perf_counter() - t:.3f}s")
    return wrapper
```
**Context manager** — guarantees setup/teardown with `with`, via `__enter__`/`__exit__` (which runs even on exceptions). Used for files, locks, DB transactions, temp dirs.

```python
from contextlib import contextmanager
@contextmanager
def transaction(conn):
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
```

### `*args`, `**kwargs`.
- In a **definition**: `*args` collects extra positional arguments into a tuple; `**kwargs` collects extra keyword arguments into a dict.
- In a **call**: `*` unpacks an iterable into positional args; `**` unpacks a dict into keyword args.

```python
def f(a, *args, sep="-", **kwargs):
    print(a, args, sep, kwargs)

f(1, 2, 3, sep="+", x=9)      # 1 (2, 3) + {'x': 9}
params = {"sep": "/", "y": 1}
f(*[1, 2], **params)          # 1 (2,) / {'y': 1}
```
Common uses: decorators/wrappers that forward arbitrary arguments, extensible APIs. Related syntax: a bare `*` forces keyword-only parameters (`def f(a, *, timeout)`), `/` forces positional-only. Overusing `**kwargs` hides the real interface from readers and type checkers.

### Shallow vs. deep copy.
- **Shallow copy** (`copy.copy(x)`, `list(x)`, `x[:]`, `dict.copy()`) — new outer container, but **inner objects are shared references**.
- **Deep copy** (`copy.deepcopy(x)`) — recursively copies everything; fully independent (handles cycles via a memo dict). Slower, and some objects (sockets, locks) can't be copied.

```python
import copy
a = [[1, 2], [3]]
b = copy.copy(a);     b[0].append(99)   # a == [[1, 2, 99], [3]]  shared inner list
c = copy.deepcopy(a); c[0].append(7)    # a unchanged
```
Plain assignment (`b = a`) copies nothing — just another name. Related pitfall: `[[0] * 3] * 3` creates three references to the same inner list; use `[[0] * 3 for _ in range(3)]`.

### `asyncio` basics: event loop, `gather`, semaphores.
- **Event loop** — runs on one thread; schedules coroutines and switches between them only at `await` points while they wait on I/O. `asyncio.run(main())` starts it.
- **Coroutine** — `async def` function; calling it returns a coroutine object that does nothing until awaited or wrapped in a **Task** (`asyncio.create_task`) to run concurrently.
- **`gather`** — run many awaitables concurrently and collect results in order; `return_exceptions=True` to not fail fast. Python 3.11+ also has `TaskGroup` (structured concurrency, cancels siblings on error).
- **Semaphore** — limit concurrency so you don't overwhelm an API or exceed rate limits.

```python
import asyncio, httpx

async def fetch(client, sem, url):
    async with sem:                         # at most 10 in flight
        r = await client.get(url, timeout=10)
        return r.status_code

async def main(urls):
    sem = asyncio.Semaphore(10)
    async with httpx.AsyncClient() as client:
        return await asyncio.gather(*(fetch(client, sem, u) for u in urls),
                                    return_exceptions=True)

# asyncio.run(main(urls))
```
Pitfalls: blocking calls (`requests`, `time.sleep`, heavy CPU) freeze the loop — use `asyncio.to_thread`; forgetting `await`; un-awaited tasks being garbage-collected (keep references). `asyncio.wait_for` for timeouts.

### Type hints, dataclasses, Pydantic.
- **Type hints** — annotations (`def f(x: int) -> list[str]`, `str | None`, `Protocol`, `TypedDict`, generics). **Not enforced at runtime**; checked statically by mypy/pyright and used by IDEs. Improve readability and catch bugs in large or handed-over codebases.
- **dataclasses** — `@dataclass` auto-generates `__init__`, `__repr__`, `__eq__`; options `frozen=True`, `slots=True`, `field(default_factory=list)`. Lightweight internal data containers. **No validation**.
- **Pydantic** — models that **validate and coerce at runtime** from untrusted input (JSON, env vars, API payloads, LLM outputs), with clear errors, JSON Schema generation, serialization. Basis of FastAPI and many LLM structured-output/tool-calling libraries. v2 has a fast Rust core.

```python
from pydantic import BaseModel, Field
class Invoice(BaseModel):
    id: str
    amount: float = Field(gt=0)
    tags: list[str] = []
Invoice.model_validate({"id": "a1", "amount": "12.5"})   # amount coerced to 12.5
```
Rule: Pydantic at system boundaries, dataclasses inside.

### Virtual environments and dependency management (pip, poetry, uv).
A **virtual environment** is an isolated Python install (`python -m venv .venv`) with its own `site-packages`, so projects don't conflict with each other or the system Python.

- **pip** — the baseline installer. `requirements.txt`; for reproducibility pin everything (`pip-tools`' `pip-compile` generates a fully pinned lock file from `requirements.in`).
- **Poetry** — manages deps in `pyproject.toml`, resolves to `poetry.lock`, handles venvs, building and publishing packages.
- **uv** (Astral, Rust) — very fast drop-in for pip/pip-tools/virtualenv plus project management: `uv init`, `uv add`, `uv lock` (`uv.lock`), `uv sync`, `uv run`, and installs Python versions themselves. Increasingly the default for new projects.

Best practices: declare deps in `pyproject.toml`, **commit the lock file**, separate dev dependencies, pin the Python version, build Docker images from the lock file, scan dependencies (Dependabot, `pip-audit`). For customer environments, check whether they need an internal package mirror (Artifactory) due to egress restrictions.

## Language-specific (JavaScript/TypeScript)

### Event loop, microtasks vs. macrotasks.
JavaScript runs on a single thread with a **call stack**. Async work (timers, I/O, network) is handled by the host (browser/Node/libuv), and callbacks are queued:
- **Macrotask (task) queue** — `setTimeout`, `setInterval`, I/O callbacks, UI events, `MessageChannel`, `setImmediate` (Node).
- **Microtask queue** — Promise `.then/.catch/.finally` callbacks, `await` continuations, `queueMicrotask`, `MutationObserver`. (Node: `process.nextTick` runs even before promise microtasks.)

Loop: run the current script/task to completion → **drain the entire microtask queue** → (browser may render) → take the next macrotask → repeat.

```js
console.log("1");
setTimeout(() => console.log("timeout"), 0);
Promise.resolve().then(() => console.log("micro"));
queueMicrotask(() => console.log("micro 2"));
console.log("2");
// 1, 2, micro, micro 2, timeout
```
Consequences: CPU-heavy synchronous code blocks everything (use Web Workers / worker_threads); endlessly scheduling microtasks can starve rendering and timers.

### Promises and async/await.
A **Promise** represents a future value with states **pending → fulfilled | rejected** (settled once). Chain with `.then()`/`.catch()`/`.finally()`; each returns a new promise.

`async/await` is syntax over promises: an `async` function always returns a promise; `await` pauses that function (not the thread) until the promise settles, and the rest runs as a microtask. Errors are handled with `try/catch`.

```ts
async function load(ids: string[]) {
  try {
    // concurrent, not sequential:
    const users = await Promise.all(ids.map(id => fetch(`/api/users/${id}`).then(r => r.json())));
    return users;
  } catch (err) {
    console.error("one request failed", err);
    throw err;
  }
}
```
Combinators: `Promise.all` (fail fast), `Promise.allSettled` (all outcomes), `Promise.race` (first settled, useful for timeouts), `Promise.any` (first fulfilled). Pitfalls: `await` inside a `for` loop serializes work; unhandled rejections; forgetting to `await` or `return` a promise; `forEach` with async callbacks doesn't wait.

### Closures; `this` binding.
**Closure** — a function retains access to variables from the scope where it was **defined**, even after that scope returns. Used for data privacy, factories, memoization, callbacks.

```js
function counter() {
  let n = 0;
  return () => ++n;      // closes over n
}
const c = counter(); c(); c(); // 2
```
Classic bug: `for (var i = 0; i < 3; i++) setTimeout(() => console.log(i))` prints `3 3 3` (one shared `var`); `let` creates a new binding per iteration → `0 1 2`.

**`this`** is determined by **how a function is called** (except arrow functions):
- `obj.method()` → `obj`.
- Plain call `fn()` → `undefined` in strict mode / modules (global object in sloppy mode).
- `new Fn()` → the new instance.
- `fn.call(x)`, `fn.apply(x)`, `fn.bind(x)` → explicitly `x`.
- **Arrow functions** have no own `this`; they capture it lexically from the enclosing scope — ideal for callbacks inside methods.

Common bug: passing `obj.method` as a callback loses `this`; fix with `.bind(obj)` or an arrow function.

### `==` vs. `===`; `let`/`const`/`var`.
- **`===`** — strict equality: no type coercion; types and values must match (objects compared by reference). **Use it by default.**
- **`==`** — loose equality with type coercion and surprising results: `0 == ""` true, `"1" == 1` true, `null == undefined` true, `[] == false` true. The only common idiomatic use is `x == null` to check null-or-undefined. Note `NaN !== NaN` (use `Number.isNaN`); `Object.is` handles that and `-0`.

Declarations:
- **`var`** — function-scoped, hoisted and initialized to `undefined`, can be redeclared; leaks out of blocks. Avoid.
- **`let`** — block-scoped, reassignable; hoisted but in the **temporal dead zone** until its declaration (access throws `ReferenceError`).
- **`const`** — block-scoped, must be initialized, **binding** can't be reassigned — but the object it points to is still mutable (`Object.freeze` for shallow immutability).

Default to `const`, use `let` when reassigning, never `var`.

### TypeScript generics and utility types.
**Generics** let types be parameterized so functions/classes stay type-safe across types instead of falling back to `any`.

```ts
function first<T>(xs: T[]): T | undefined { return xs[0]; }

interface ApiResponse<T> { data: T; error?: string }

function getProp<T, K extends keyof T>(obj: T, key: K): T[K] { return obj[key]; }
```
Constraints (`extends`), defaults (`<T = string>`), and inference make them ergonomic.

**Built-in utility types:**
- `Partial<T>` / `Required<T>` — make all properties optional / required (e.g., PATCH payloads).
- `Readonly<T>` — immutable properties.
- `Pick<T, K>` / `Omit<T, K>` — select or drop keys (`Omit<User, "password">`).
- `Record<K, V>` — object type with keys K and values V.
- `Exclude<U, X>` / `Extract<U, X>` / `NonNullable<T>` — filter union types.
- `ReturnType<F>`, `Parameters<F>`, `Awaited<T>` — derive types from functions/promises.

Advanced: conditional types (`T extends U ? X : Y`), mapped types (`{ [K in keyof T]: ... }`), template literal types. Pair with runtime validation (Zod) at API boundaries, since TS types are erased at runtime — the TS counterpart of Pydantic.

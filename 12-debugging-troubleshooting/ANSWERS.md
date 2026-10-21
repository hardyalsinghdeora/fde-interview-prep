# 12 — Debugging & Troubleshooting: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts. Adapt them to your own experience and stack, and practise saying them out loud. In a real interview, narrate your hypotheses in priority order (most likely and cheapest to check first), name the exact command you would run, then state the fix and how you would prevent a recurrence.

---

## A. Live debugging round formats

### Given a repo with failing unit tests, make them pass and explain each bug.

**Approach (narrate it):**
1. Orient (2-3 min): read the README, `ls`, the entry point, and how tests run (`pytest -q`, `npm test`, `go test ./...`).
2. Run the whole suite once, then one failing test at a time: `pytest -x -q`, `pytest tests/test_cart.py::test_total -vv`, `pytest --lf` (last-failed).
3. Read the assertion diff before reading code. Expected vs actual usually points straight at the bug.
4. Form one hypothesis, make the smallest change, rerun. Never edit the test to make it pass unless the test itself is wrong, and say so explicitly if it is.
5. After each fix, rerun the full suite to catch regressions. Keep a running list: "bug, root cause, fix".

**Example bug and fix:**
```python
# Bug: discount applied as integer division, and empty cart crashes
def average_price(items):
    return sum(i["price"] for i in items) // len(items)

# Fix
def average_price(items):
    if not items:
        return 0.0
    return sum(i["price"] for i in items) / len(items)   # O(n) time, O(1) space
```
Explanation to the interviewer: "`//` floored 10.5 to 10, which is why `test_avg` expected 10.5; and `len([])` raised `ZeroDivisionError` in `test_empty`."

**Signals they want:** systematic process, small diffs, explaining *why* each fix is right, and noticing related bugs the tests do not yet cover (then adding a test).

### Given buggy code, find all bugs (off-by-one, null handling, mutation, concurrency, wrong types).

Read the code once top to bottom against a checklist: **boundaries, None/empty, shared mutable state, types/units, error paths, concurrency.**

```python
def paginate(items, page, size, seen=[]):        # 1. mutable default arg
    start = page * size                          # 2. off-by-one if pages are 1-based
    end = start + size - 1                       # 3. slice end is exclusive: drops last item
    result = items[start:end]
    for r in result:
        r["name"] = r["name"].strip()            # 4. None name -> AttributeError; 5. mutates caller's dicts
        seen.append(r["id"])
    total = len(items) / size                    # 6. float page count; should round up
    return result, total, seen
```
**Fixed:**
```python
import math

def paginate(items, page, size):
    if page < 1 or size < 1:
        raise ValueError("page and size must be >= 1")
    start = (page - 1) * size
    result = [{**r, "name": (r.get("name") or "").strip()} for r in items[start:start + size]]
    total_pages = math.ceil(len(items) / size)
    return result, total_pages
# O(size) time and extra space for the page
```
Concurrency bug pattern to mention: `counter += 1` from multiple threads is a read-modify-write, not atomic; protect with a `threading.Lock`. Wrong-types pattern: comparing `"10" > "9"` (string compare is False) or mixing `datetime` naive/aware.

### Re-engineer: improve the readability/performance of a working-but-messy program.

**Process:**
1. **Lock behaviour first.** If there are no tests, write characterization tests that capture current outputs (including odd ones). Refactoring without a safety net is guessing.
2. **Measure before optimizing.** `python -m cProfile -s cumtime app.py`, `py-spy top`, or a simple `time.perf_counter()` around suspected hot spots.
3. **Readability:** extract functions with clear names, remove duplication, replace magic numbers with constants, flatten nested ifs with early returns, add type hints.
4. **Performance:** fix the algorithm before micro-tuning: O(n^2) list membership to a set/dict, repeated I/O or DB calls in a loop to a batch, recomputation to caching.
5. Small commits, rerun tests after each, and explain trade-offs.

```python
# Before: O(n*m), unclear names
def f(a, b):
    r = []
    for x in a:
        if x["id"] in [y["id"] for y in b]:
            r.append(x)
    return r

# After: O(n + m)
def orders_with_known_customer(orders, customers):
    known_ids = {c["id"] for c in customers}
    return [o for o in orders if o["id"] in known_ids]
```
Say what you would *not* change in a time-boxed round (e.g. a large architectural rewrite) and why.

### Read a stack trace and identify the root cause.

- **Read bottom-up for the exception, then walk up to the first frame in *your* code.** In Python the last line is the exception; in Java the top line is, with `Caused by:` chains below (the deepest `Caused by` is usually the real root cause).
- Separate the **symptom** (where it crashed) from the **cause** (where bad state was created). A `KeyError` in a renderer may come from a parser that silently dropped a field.
- Note the exception type and message, file:line, the arguments involved, and thread name.

Example:
```
File "billing.py", line 42, in invoice_total
    return sum(line.amount for line in invoice.lines)
TypeError: unsupported operand type(s) for +: 'int' and 'NoneType'
```
Root cause is not line 42: some `line.amount` is `None`. Check where lines are built (e.g. the CSV loader maps empty strings to `None`). Fix at the source (validate/parse on ingest, reject or default with a logged warning), then add a guard and a test.

Tools: reproduce with the same input; `python -X faulthandler`, `pdb`/`breakpoint()` at the frame, `jstack` for Java thread dumps, Sentry for grouped traces with request context.

### Find the memory leak / performance bottleneck in this code.

**Common leak patterns:** unbounded caches/dicts, global lists that keep appending, listeners/callbacks never deregistered, closures capturing large objects, `lru_cache` on methods (holds `self`), unclosed files/connections, reference cycles with `__del__`.

```python
_cache = {}
def get_user(uid):
    if uid not in _cache:                    # Leak: grows forever with unique uids
        _cache[uid] = db.fetch(uid)
    return _cache[uid]

# Fix: bounded cache with eviction (and a TTL if data changes)
from functools import lru_cache
@lru_cache(maxsize=10_000)
def get_user(uid):
    return db.fetch(uid)
```
**Tools:** `tracemalloc` (snapshot twice, `compare_to`, sort by `lineno`), `objgraph.show_growth()`, `memray`, `py-spy dump`; Java: heap dump with `jmap -dump`, analyse in Eclipse MAT; Node: `--inspect` heap snapshots.

**Bottlenecks:** profile, do not guess. `cProfile` + `snakeviz`, `py-spy record -o flame.svg --pid N`. Typical culprits: N+1 DB queries in a loop, string concatenation in a loop, repeated JSON parsing, O(n^2) lookups, synchronous network calls in series. Fix the top item in the profile, re-measure, stop when good enough.

### Fix a race condition in this multi-threaded code.

```python
import threading

class Account:
    def __init__(self):
        self.balance = 0

    def deposit(self, amt):
        current = self.balance        # read
        current += amt                # modify
        self.balance = current        # write: another thread may have written in between

acct = Account()
threads = [threading.Thread(target=lambda: [acct.deposit(1) for _ in range(100_000)]) for _ in range(4)]
# Expected 400_000, often less: lost updates
```
**Fix:** make the read-modify-write atomic.
```python
class Account:
    def __init__(self):
        self.balance = 0
        self._lock = threading.Lock()

    def deposit(self, amt):
        with self._lock:
            self.balance += amt
```
**How to talk about it:**
- Identify shared mutable state and every access to it. Races are about *interleavings*; the GIL does not make `+=` atomic.
- Options: locks (keep critical sections small), thread-safe structures (`queue.Queue`), immutability, confinement (one owner thread), or atomic DB operations (`UPDATE ... SET balance = balance + 1`, `SELECT ... FOR UPDATE`, optimistic version columns).
- Watch for deadlocks: always acquire multiple locks in a consistent order; use timeouts.
- Check-then-act races (`if key not in d: d[key] = ...`) need the check and act under one lock, or `dict.setdefault`.
- Prove it: a stress test with many threads, and `sys.setswitchinterval(1e-6)` to force more interleavings.

### This API integration returns wrong results — find out why.

**Hypotheses, in order of likelihood:**
1. **Request is not what you think.** Log the exact outgoing request (URL, query params, headers, body). Wrong encoding (`+` vs `%20`), wrong param name, missing filter, wrong environment (sandbox vs prod base URL), wrong API version header.
2. **Pagination:** only reading page 1, or off-by-one with cursor/offset, or results reordering between pages.
3. **Response parsing:** wrong field path, ignoring nested `data` envelope, treating `null` as 0, number precision (IDs > 2^53 in JS), date/timezone (UTC vs local, epoch seconds vs ms), currency in minor units (cents).
4. **Partial success:** 200 with an `errors` array, or a 207 multi-status you treat as success.
5. **Caching/staleness:** your cache, a CDN, or eventual consistency on their side.
6. **Auth scope:** token for a different tenant/account returns a valid but different dataset.

**Commands:** reproduce outside the code with `curl -v` or Postman using identical inputs; diff against their docs' example; turn on HTTP debug logging (`http.client.HTTPConnection.debuglevel = 1`, or `requests` hooks); use `mitmproxy` to capture traffic.

**Fix and prevent:** fix at the boundary with a typed client (pydantic models validating responses), contract tests against recorded fixtures, and a reconciliation check (counts/totals vs the source).

### Code review: what would you comment on in this PR?

Review in priority order and label comments by severity (blocking / suggestion / nit):
1. **Correctness:** does it do what the ticket says? Edge cases (empty, None, large inputs, unicode, timezones), error handling, off-by-one, retries without idempotency.
2. **Security:** SQL/command injection (string-formatted queries), secrets in code or logs, missing authz checks, unsafe deserialization, SSRF in user-supplied URLs, PII logged.
3. **Data and concurrency:** migrations that lock large tables, non-backward-compatible schema changes, races, transactions.
4. **Performance:** N+1 queries, unbounded loops/queries without LIMIT, loading whole files into memory.
5. **Tests:** are there tests for the new behaviour and failure paths? Do they actually assert something?
6. **Operability:** logging, metrics, feature flag, rollback path, config defaults.
7. **Readability:** naming, function size, dead code, comments explaining *why*.

Example comment: "Blocking: `cursor.execute(f\"SELECT * FROM users WHERE email='{email}'\")` is injectable. Use a parameterized query: `cursor.execute(\"... WHERE email = %s\", (email,))`."

Tone: ask questions rather than issue verdicts, praise good parts, and keep nits non-blocking. Big PRs: ask to split.

---

## B. Production troubleshooting scenarios

For every scenario the shape is the same: **scope the blast radius, check what changed, form ranked hypotheses, verify each with a specific command or query, mitigate, then fix the root cause and prevent recurrence.**

### The customer says "the app is slow." How do you investigate?

**Clarify first:** which page/action, for whom (all users, one tenant, one region), since when, how slow (2s to 4s or to 60s), constant or spiky? Get a HAR file or a timestamped example request ID.

**Checklist (top-down):**
1. **Client vs server:** browser DevTools Network/Performance tab. Is time spent in TTFB (server) or in download/render (frontend bundle, large images, JS)?
2. **Golden signals** in APM (Datadog, New Relic, Grafana): latency p50/p95/p99, error rate, throughput, saturation. Compare to baseline and correlate with deploy markers.
3. **Distributed trace** of a slow request (OpenTelemetry/Jaeger/Datadog APM): which span dominates? DB, external API, lock wait, serialization.
4. **Database:** slow query log, `pg_stat_statements` ordered by `total_exec_time`, `EXPLAIN (ANALYZE, BUFFERS)` on the top query, lock waits in `pg_stat_activity`.
5. **Resources:** CPU, memory/GC pauses, connection pool exhaustion, thread pool queueing (`top`, `vmstat 1`, pool metrics).
6. **Network/dependencies:** third-party API latency, DNS, cross-region calls.

**Fix:** depends on finding (index, cache, batch calls, scale out, raise pool size). **Prevent:** SLOs with latency alerts on p95, synthetic checks for key journeys, performance tests in CI for critical paths.

### The website is down. Walk me through your first 15 minutes.

- **Minute 0-2: confirm and declare.** Check from outside (`curl -sv https://site/health`, a different network, status page probes). Declare an incident, open a channel, assign an incident commander and a comms owner (even if both are you initially).
- **Minute 2-5: scope.** All users or some? All endpoints? What do monitors and dashboards show (error rate, 5xx, traffic drop)? Is a dependency (cloud provider, CDN, auth) having an outage? Check their status pages.
- **Minute 5-10: what changed?** Recent deploys, config/feature-flag changes, infra changes (Terraform applies), cert expiry, DNS changes, traffic spike. `kubectl rollout history`, CI/CD logs, audit logs.
- **Minute 10-15: mitigate before root-causing.** Roll back the last deploy (`kubectl rollout undo deploy/web`), flip the flag off, fail over, scale up, or block abusive traffic at the WAF. Restore service first.
- **Throughout:** post a customer update within ~15 minutes (what is affected, that you are working on it, next update time). Keep a timeline of actions with timestamps for the post-mortem.

Layer checks if no obvious change: DNS (`dig site`), TLS (`openssl s_client`), LB health checks and target status, app pods (`kubectl get pods`, logs), DB reachable and not out of connections/disk.

### An API is returning 500 errors intermittently. How do you debug?

"Intermittent" usually means **a subset of something**: one bad instance, one code path, some inputs, a timing condition, or a flaky dependency.

1. **Find the pattern.** Group errors in logs/APM by host/pod, endpoint, customer, input shape, time of day. Query example (Datadog/Loki): `status:500 service:api | count by pod, route`. One pod? Drain and replace it, then inspect (bad disk, wrong config, stale version).
2. **Get the stack trace** from Sentry or logs using the request ID. Common causes: unhandled `None` for some inputs, timeouts to a dependency surfacing as 500, DB connection pool exhaustion under load, deadlocks.
3. **Correlate with load:** errors spike with traffic implies resource limits (pool size, file descriptors, memory). Check `pg_stat_activity` connection count vs `max_connections`.
4. **Mixed versions:** during or after a partial rollout, some pods run old code. `kubectl get pods -o jsonpath='{..image}'`.
5. **Dependency flakiness:** traces showing failing downstream calls; add timeouts, retries with backoff and jitter (idempotent calls only), circuit breakers.

**Fix:** handle the failing case, map dependency failures to correct status codes (502/503/504, not 500). **Prevent:** structured logs with request IDs, error-rate SLO alerts, a regression test with the failing input.

### An API is returning 502/504. What could cause it?

These come from a **proxy/load balancer**, not the app: 502 = the upstream gave an invalid response or closed the connection; 504 = the upstream did not respond in time.

**502 causes:**
- App crashed or restarting (OOMKilled, CrashLoop), so LB connects to nothing.
- Keep-alive mismatch: app idle timeout shorter than LB's, so the LB reuses a connection the app already closed. Fix: app keep-alive > LB idle timeout (e.g. ALB 60s, set gunicorn `--keep-alive 65`).
- Wrong upstream port/protocol (HTTP to an HTTPS backend), response headers too large for the proxy buffer.
- No healthy targets.

**504 causes:**
- Slow requests exceeding the proxy timeout (nginx `proxy_read_timeout` 60s default, ALB idle timeout): slow DB query, slow third party.
- Worker exhaustion: all gunicorn/uwsgi workers busy, requests queue past the timeout.
- Network: security group/firewall dropping packets to upstream.

**Checks:** LB access logs (ALB `elb_status_code` vs `target_status_code`, `target_processing_time`), nginx error log (`upstream prematurely closed connection`, `upstream timed out`), `kubectl get pods` restarts, target group health, app logs at the same timestamp, `curl` the backend directly bypassing the LB.

**Fix/prevent:** align timeouts: the app's own request and dependency timeouts should be shorter than the LB timeout, and the LB timeout shorter than the client's, so failures surface as clear errors at the right layer, move long work to async jobs with polling, scale workers, alert on 5xx at the LB.

### The app works locally but fails in the customer's environment.

Something differs. Enumerate the differences systematically:
1. **Configuration and secrets:** env vars missing or different (`env | sort` diff), wrong endpoints, feature flags.
2. **Network:** corporate proxy (`HTTPS_PROXY`/`NO_PROXY`), egress firewall, private DNS, TLS-intercepting proxy with a custom root CA (`SSL: CERTIFICATE_VERIFY_FAILED`). Test with `curl -v`, `nslookup`, `nc -vz host 443`.
3. **Versions:** OS, Python/Node/JDK, library versions (unpinned deps), DB version, container base image, CPU architecture (arm64 vs amd64).
4. **Permissions:** IAM roles, filesystem read-only, non-root containers, SELinux, Kubernetes PodSecurity, service account scopes.
5. **Data:** real data volume, encodings, nulls, unicode, timezones, locale (decimal comma).
6. **Resources:** smaller memory/CPU limits, ephemeral disk.

**Tools:** get the full error and logs; run a diagnostic script that prints versions, config (redacted), DNS resolution and connectivity checks; reproduce in a container matching their image.

**Fix:** make the delta explicit. **Prevent:** pin dependencies with lockfiles, ship as a container, validate config at startup (fail fast with a clear message), provide a preflight/`doctor` command, and document network requirements (hosts, ports, CAs).

### It worked yesterday and nobody changed anything. What do you check?

Something always changed; it just was not a code deploy. Check:
- **Time-based:** expired TLS certs, expired API keys/tokens/passwords, OAuth client secrets, license keys, DST or month/year boundaries, leap days, date-dependent logic.
- **Data:** volume crossed a threshold (table outgrew memory, query plan flipped), a new malformed record, a new customer with unusual data.
- **Dependencies:** a third-party API changed or deprecated a version, an auto-updated SaaS, a floating container tag (`:latest`) or unpinned package pulled a new version on restart.
- **Infra:** managed service maintenance/upgrade, auto-scaling events, node replaced, quota or rate limit reached, disk full, DNS record TTL expired to a new IP.
- **Someone else's change:** network/firewall/IdP changes by the customer's IT, rotated credentials, group membership changes.
- **Accumulated state:** memory leak hitting a limit after N days, log files filling disk, connection leak.

**Commands:** `openssl s_client -connect host:443 | openssl x509 -noout -dates`, cloud audit logs (CloudTrail, GCP Audit Logs), `docker image inspect` digests vs yesterday, `df -h`, provider status pages, the change calendar.

**Prevent:** pin images by digest and dependencies by lockfile, alert on cert/secret expiry 30 days ahead, monitor quotas.

### Database queries suddenly became 10x slower.

**Hypotheses:**
1. **Plan change:** stale statistics after a big data load, so the planner picked a sequential scan or bad join. Check `EXPLAIN (ANALYZE, BUFFERS)`; compare estimated vs actual rows. Fix: `ANALYZE table;`.
2. **Missing/dropped/invalid index**, or a new query pattern not covered. `\d table`, `pg_stat_user_indexes` (idx_scan), `pg_stat_user_tables` (seq_scan count).
3. **Lock contention:** a long transaction or migration holding locks. `SELECT pid, state, wait_event_type, query, now()-xact_start FROM pg_stat_activity ORDER BY xact_start;` and `pg_locks`.
4. **Bloat / vacuum:** dead tuples (`n_dead_tup` in `pg_stat_user_tables`), autovacuum blocked by a long-running transaction.
5. **Resource saturation:** CPU, IOPS (cloud burst credits exhausted on gp2/ burstable instances), memory (working set no longer fits in cache, buffer hit ratio drop), connections.
6. **Data growth:** table crossed a size where the query no longer fits in memory.
7. **Noisy neighbour:** a new batch job or analytics query on the primary.

**Top queries:** `SELECT query, calls, mean_exec_time, total_exec_time FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 10;` MySQL equivalents: slow query log, `SHOW PROCESSLIST`, `EXPLAIN ANALYZE`.

**Fix:** add/repair index (`CREATE INDEX CONCURRENTLY`), refresh stats, kill blocking session, move analytics to a replica. **Prevent:** query latency alerts, review `EXPLAIN` for new queries, tune autovacuum.

### A batch job that usually takes 1 hour now takes 8 hours.

1. **Input changed?** Compare input size and shape to previous runs (row counts, file sizes, skew: one key now with 50% of data). 8x data rarely means 8x time; a jump often signals something superlinear.
2. **Where is the time?** Per-stage timings from job logs; Spark UI (stage durations, task skew, spill to disk, shuffle size, GC time); Airflow task durations.
3. **Resources:** fewer executors/workers (quota, spot instances reclaimed), memory pressure causing spill or swapping, CPU throttling in containers (`container_cpu_cfs_throttled_seconds_total`).
4. **Downstream/upstream:** DB writes slower (locks, indexes added on target table, triggers), API it calls now rate-limits (look for 429 and retry backoff), network to a storage bucket in another region.
5. **Code/config change:** new library version, a join that became a cross join, a lost partition filter causing full scans, broadcast join disabled.
6. **Contention:** another job now runs at the same time.

**Fix examples:** salting skewed keys, restoring partition pruning, bulk loads (`COPY`) instead of row-by-row inserts, dropping indexes during load, increasing parallelism. **Prevent:** record duration and row counts per run and alert on deviation (e.g. > 2x median), data volume checks before run, SLA miss alerts in the orchestrator.

### Memory usage keeps growing until the service crashes.

**Confirm it is a leak, not a cache warming up:** a sawtooth that returns to baseline after GC is normal; a rising floor across GCs is a leak. Plot RSS/heap over days and correlate with traffic and deploys.

**Hypotheses:** unbounded in-memory cache or map keyed by request/user, objects held by global lists or event listeners, connection/file handle leaks, thread leaks (each with its own stack), large responses held in memory, native memory (glibc fragmentation, C extensions), JVM heap set larger than the container limit.

**Tools:**
- Python: `tracemalloc` snapshots diffed over time, `memray`, `objgraph.show_most_common_types()`, `py-spy dump`.
- JVM: `jcmd <pid> GC.heap_info`, heap dump `jmap -dump:live,format=b,file=heap.hprof <pid>`, analyse dominators in Eclipse MAT; GC logs.
- Node: `--inspect` and heap snapshots in Chrome DevTools, `--heapsnapshot-near-heap-limit`.
- Linux: `ps -o rss`, `/proc/<pid>/status`, `lsof -p <pid> | wc -l` (handle leaks), `ls /proc/<pid>/task | wc -l` (threads).
- Kubernetes: `kubectl top pod`, `kubectl describe pod` for `OOMKilled`.

**Mitigate:** scheduled restarts or `max_requests` for gunicorn workers while investigating. **Fix:** bound caches, close resources with context managers, deregister listeners. **Prevent:** memory alert on trend, soak tests, memory limits set intentionally.

### CPU is at 100% on one server.

**One server, not all:** suspect something specific to that host: load-balancer imbalance (sticky sessions, hot key), a stuck thread/infinite loop, a cron job or backup running there, a noisy co-tenant, or a compromised host (crypto miner).

**Commands:**
- `top -c` / `htop`: which process? `top -H -p <pid>`: which thread?
- `uptime` load average, `vmstat 1` (us vs sy vs wa vs st; high `st` = hypervisor steal, high `wa` = I/O bound, not CPU).
- Python: `py-spy top --pid <pid>` / `py-spy dump`; Java: `jstack <pid>` and map the hot thread ID (hex) from `top -H`; generic: `perf top -p <pid>`, `perf record -g` for flame graphs.
- GC: JVM spending most time in GC looks like high CPU (`jstat -gcutil <pid> 1000`).
- Traffic: LB metrics per target, access log request counts per host; check for regex catastrophic backtracking or a single huge request.
- Unknown processes: `ps auxf`, `crontab -l`, `systemctl list-timers`.

**Mitigate:** take the host out of the LB rotation, capture diagnostics (thread dump, profile) *before* restarting, then restart/replace. **Fix:** the hot code path, rebalance the load. **Prevent:** per-host CPU alerts, autoscaling, request timeouts, profiling in production (continuous profilers like Pyroscope/Datadog).

### Disk filled up on a production machine.

**Immediate:**
1. `df -h` (which filesystem), `df -i` (inodes can run out with free space left: many small files).
2. Find the culprit: `du -xh / --max-depth=2 | sort -h | tail -20`, or `ncdu -x /`.
3. **Deleted but still open files:** `lsof +L1` or `lsof | grep deleted`. Space is not freed until the process closes the file; restart the process or truncate via `: > /proc/<pid>/fd/<n>`.

**Usual suspects:** application logs without rotation, debug logging left on, core dumps, Docker images/containers/volumes (`docker system df`, `docker system prune`), journald (`journalctl --disk-usage`, `journalctl --vacuum-size=500M`), temp files from a failed job, DB WAL/binlogs piling up because replication or archiving is stuck (`pg_replication_slots` with an inactive slot is a classic), backups written locally.

**Safe cleanup:** truncate rather than delete open log files (`truncate -s 0 app.log`), compress old logs, never `rm` database files.

**Prevent:** logrotate with size limits, log shipping to central storage, Docker log driver `max-size`, alerts at 80% and 90% disk and on growth rate, separate volumes for logs and data, retention policies.

### Users in one region can't reach the service (DNS? CDN? firewall?).

Work up the stack from a client in the affected region (ask a user, use a VPN/cloud VM in that region, or tools like RIPE Atlas/Catchpoint).

1. **DNS:** `dig +trace app.example.com`, `dig @8.8.8.8` vs the local resolver. Geo-DNS / latency-based routing could point that region to an unhealthy endpoint. Stale records cached after a change (check TTL). Some ISPs or countries block resolvers.
2. **Connectivity:** `traceroute`/`mtr app.example.com` for where packets die; `nc -vz host 443`; `curl -v --resolve app.example.com:443:<ip> https://app.example.com/` to test a specific edge IP.
3. **CDN/edge:** a specific PoP unhealthy (check the `cf-ray`/`x-amz-cf-pop` header to identify it), CDN status page, WAF/geo-blocking rule, bot protection challenging a region's IP range.
4. **Firewall/security groups:** allow-lists that exclude a region's NAT ranges; customer corporate firewall.
5. **Regional backend:** in multi-region deployments, the region's LB/target health, a failed regional deploy, cloud regional outage.
6. **TLS:** certificate served by that PoP differs or is expired; SNI issues.

**Mitigate:** route that region to a healthy region/PoP via DNS or CDN config. **Prevent:** synthetic monitoring from multiple geographies, health-checked DNS failover.

### SSL/TLS certificate errors at the customer site.

**Get the exact error**, it tells you which hypothesis: `certificate has expired`, `unable to get local issuer certificate`, `hostname mismatch`, `self-signed certificate in certificate chain`, handshake failure.

**Hypotheses and checks:**
1. **TLS-intercepting corporate proxy** (Zscaler, Palo Alto, Bluecoat) re-signing traffic with the company's root CA, which your client/container does not trust. Very common at enterprises. Check the issuer: `openssl s_client -connect api.example.com:443 -servername api.example.com -showcerts`. Fix: add the customer's CA to the trust store (`REQUESTS_CA_BUNDLE`, `SSL_CERT_FILE`, `NODE_EXTRA_CA_CERTS`, Java `keytool -importcert` into the truststore), or ask for a proxy bypass. Never disable verification as the fix.
2. **Incomplete chain** served by the server (missing intermediate): browsers may work (AIA fetching) while curl/Python fail. Check with `openssl s_client` or SSL Labs.
3. **Expired certificate** or wrong clock on the client (`date`, NTP).
4. **Hostname mismatch:** accessing by IP or internal alias not in the SAN list. `openssl x509 -noout -text | grep -A1 "Subject Alternative Name"`.
5. **Outdated client:** old OS/Java trust store missing newer roots, TLS 1.0/1.1 only, cipher mismatch.
6. **mTLS:** client certificate missing or not trusted.

**Prevent:** automated renewal (cert-manager, ACM, Let's Encrypt), expiry alerts, and a documented "custom CA" setup in the deployment guide.

### Auth/SSO login fails only for some users.

Find what the failing users have in common: IdP group, email domain, account age, browser, region, user attributes.

**Hypotheses:**
1. **Attribute/claim mapping:** missing email or `NameID`, different format (UPN vs email), uppercase vs lowercase email mismatch with your user table, users without the required group claim. Capture the SAML assertion (SAML-tracer browser extension) or decode the OIDC ID token (`jwt.io` or `base64 -d`) for a failing and a working user and diff them.
2. **Group/role assignment:** user not assigned to the app in Okta/Entra ID, or too many groups (Entra ID drops groups claims over 200 and sends an overage link instead; SAML header size limits).
3. **Clock skew** on a server: assertions "not yet valid" or expired (`NotBefore`/`NotOnOrAfter`); fix NTP.
4. **Multiple IdPs / domains:** users from an acquired company in a different tenant; home realm discovery routing wrong.
5. **Just-in-time provisioning** fails for new users (duplicate username, required field null); existing users fine.
6. **Browser issues:** third-party cookie blocking, `SameSite` cookie settings breaking the POST back, cookie too large.
7. **Certificate rotation:** IdP signing cert rolled over and one of your instances has the old one cached.

**Tools:** IdP sign-in logs (Okta System Log, Entra sign-in logs with error codes like AADSTS50105), your app's auth logs with correlation IDs. **Prevent:** case-insensitive identity matching, clear error pages that show a correlation ID, SSO test users per group.

### Data in the dashboard doesn't match the source system.

**First, define "doesn't match":** which metric, which time range, by how much, consistently or only recently? Get one specific example (a record ID or a day) to trace end to end.

**Hypotheses:**
1. **Definitions:** different metric definitions (gross vs net, booked vs recognized), filters (test accounts, deleted/cancelled records excluded), status logic.
2. **Time:** timezone (UTC in warehouse vs local in source), date boundaries, event time vs ingestion time, late-arriving data.
3. **Freshness:** pipeline failed or lagging; dashboard cache. Check last successful run and `max(updated_at)` in the table.
4. **Join fan-out or loss:** a one-to-many join duplicating rows (inflated sums), or an inner join dropping rows with null keys.
5. **Incremental load bugs:** watermark skipped records updated in the same second, soft deletes not propagated, CDC missed updates.
6. **Duplicates** from retries/re-runs.
7. **Type issues:** float rounding, currency conversion, truncated strings.

**Method:** reconcile layer by layer: source count/sum for a day → raw/staging → transformed → dashboard query. The layer where numbers diverge contains the bug. `SELECT date, count(*), sum(amount) FROM ... GROUP BY 1` at each layer, then `EXCEPT`/anti-join to find the exact mismatching IDs.

**Prevent:** data tests (dbt `unique`, `not_null`, `relationships`, row-count reconciliation vs source), freshness alerts, a documented metric glossary.

### Duplicate records appeared after a pipeline re-run.

**Root cause:** the pipeline is not idempotent. Typical patterns: `INSERT`/append instead of upsert, no unique key, a re-run reprocessing a window that overlaps the previous one, at-least-once delivery from a queue, retries after a partial write.

**Diagnose:**
```sql
SELECT order_id, count(*) FROM orders GROUP BY order_id HAVING count(*) > 1;
```
Check whether duplicates share a load batch/run ID or `_loaded_at` timestamp, which run inserted them, and whether they are exact duplicates or differ in some field (that decides which row to keep).

**Fix the data** (in a transaction, after a backup/snapshot):
```sql
DELETE FROM orders o
USING (
  SELECT ctid, row_number() OVER (PARTITION BY order_id ORDER BY _loaded_at DESC) AS rn
  FROM orders
) d
WHERE o.ctid = d.ctid AND d.rn > 1;
```
(In a warehouse: rebuild the table with `QUALIFY row_number() OVER (...) = 1`.)

**Fix the pipeline:** make writes idempotent: `MERGE`/`INSERT ... ON CONFLICT (order_id) DO UPDATE`, or delete-then-insert the partition being reprocessed (overwrite partition by date) inside one transaction. Add a unique constraint where the store supports it. **Prevent:** a uniqueness data test that fails the run, and a re-run test in CI (run twice, assert same result).

### Kafka consumer lag keeps growing.

Lag grows when produce rate > consume rate. Find which side changed.

**Check:**
- Lag per partition: `kafka-consumer-groups.sh --bootstrap-server b:9092 --describe --group my-group` (or Burrow/Datadog). Lag on **all** partitions = throughput problem; lag on **one** partition = hot key/skew or a stuck consumer.
- Producer rate: did input volume spike (incident, backfill, new customer)?
- Consumer health: are consumers rebalancing repeatedly? Look for `Rebalance`/`max.poll.interval.ms exceeded` in logs: processing a batch takes longer than `max.poll.interval.ms`, so the consumer is kicked out, rebalances, and reprocesses. Fix: smaller `max.poll.records` or faster processing.
- Processing time per message: a slow downstream (DB, API with 429s), poison message being retried forever.
- Parallelism: number of consumers > partitions means idle consumers; you cannot scale beyond partition count.

**Fix options:** scale consumers up to the partition count; increase partitions (affects key ordering); batch downstream writes; make processing async within ordering constraints; send poison messages to a dead-letter topic; fix key skew.

**Prevent:** alert on lag and on lag *growth rate* (time-based lag is better than offset lag), autoscale consumers on lag (KEDA), load test consumers at peak input.

### Kubernetes pod is in CrashLoopBackOff / OOMKilled / ImagePullBackOff.

Start with `kubectl get pods -n ns`, `kubectl describe pod <pod> -n ns` (Events and Last State), and `kubectl get events -n ns --sort-by=.lastTimestamp`.

**CrashLoopBackOff** (container starts then exits repeatedly):
- `kubectl logs <pod> --previous` for the crashed container's logs.
- Causes: app error on startup (missing env var/secret, bad config, can't reach DB), wrong command/entrypoint, failing liveness probe killing a slow-starting app (add a `startupProbe` or raise `initialDelaySeconds`), permission errors when running as non-root.
- Debug: `kubectl debug -it <pod> --image=busybox --target=<container>`, or temporarily override command to `sleep infinity` and exec in.

**OOMKilled** (exit code 137, `Reason: OOMKilled`):
- Container exceeded `resources.limits.memory`. Check `kubectl top pod` and memory metrics over time.
- Causes: limit too low for real workload, memory leak, JVM/Node heap not container-aware (set `-XX:MaxRAMPercentage=75` or `--max-old-space-size`), too many worker processes per pod.
- Fix: right-size requests/limits based on observed usage plus headroom, fix leaks, reduce workers.

**ImagePullBackOff / ErrImagePull:**
- Events show the reason: wrong image name/tag (typo, tag not pushed), private registry without `imagePullSecrets` or expired credentials, registry rate limit (Docker Hub), node cannot reach registry (egress firewall/proxy at customer site), architecture mismatch.
- Verify: `docker pull <image>` with the same credentials; `kubectl get secret regcred -o yaml`.

**Prevent:** deploy pipeline that verifies the image exists, immutable tags/digests, readiness and startup probes, resource limits based on load testing, alerts on restart counts.

### Deploy succeeded but the new feature isn't showing.

"Deploy succeeded" often means "the pipeline was green", not "users get new code."

1. **Is the new version running?** `kubectl get deploy web -o jsonpath='{.spec.template.spec.containers[0].image}'`, `kubectl rollout status deploy/web`, a `/version` or build-info endpoint returning the git SHA. Common: image tag `:latest` reused and `imagePullPolicy: IfNotPresent` so nodes kept the old image; rollout stuck with old pods still serving.
2. **Feature flag** off, or targeted to a different segment/environment (LaunchDarkly/Unleash rules).
3. **Caching:** CDN serving old JS/HTML (check `Age`, `x-cache` headers; invalidate), browser cache without hashed asset filenames, service worker serving stale bundle, app-level cache.
4. **Wrong target:** deployed to staging, another region, or a different cluster/namespace; DNS points at an old environment (blue/green switch not flipped).
5. **Config/migration:** feature depends on a DB migration or config value that did not run; frontend deployed but backend API not (or vice versa).
6. **Multiple replicas** with mixed versions behind the LB, so it appears intermittently.

**Prevent:** immutable image tags (git SHA), version endpoint and version in logs, post-deploy smoke tests that hit the new feature, cache-busting asset hashes, deploy notifications that state version + environment.

### A webhook from a third party isn't arriving.

Trace it hop by hop from the sender to your handler.

1. **Sender side:** the provider's dashboard delivery log (Stripe, GitHub, Shopify all have one): was it sent? What response code did they get? Is the endpoint configured with the right URL (staging vs prod, http vs https, trailing slash), and subscribed to the right event types? Some providers disable endpoints after repeated failures.
2. **Did it reach your edge?** LB/API gateway/WAF access logs for the path. Absent means DNS, firewall, IP allow-list (the provider's IPs not allowed), or WAF blocking (bot rule, body size).
3. **TLS:** provider rejects your cert (incomplete chain, self-signed).
4. **Reached the app but failed:** 4xx/5xx in app logs. Signature verification failing (wrong secret, verifying a re-serialized body instead of raw bytes, clock tolerance), auth middleware requiring a session, CSRF protection rejecting the POST, timeout because you do heavy work synchronously (provider times out and retries).
5. **Arrived and was "processed" but nothing happened:** event type ignored, deduplication dropped it, async queue consumer down.

**Test:** send a test event from the provider; use `stripe listen`/`ngrok` or a request bin to confirm the provider sends; replay from their dashboard.

**Prevent:** return 2xx fast and process async, store raw events with event IDs (idempotent handling), alert on "no webhooks received in N hours", reconcile periodically by polling their API for missed events.

### A cron job didn't run.

Separate "didn't start" from "started and failed silently."

**Didn't start:**
- Scheduler running? `systemctl status cron`, `grep CRON /var/log/syslog` or `journalctl -u cron`. Kubernetes: `kubectl get cronjob`, `kubectl get jobs`, check `suspend: true`, `startingDeadlineSeconds` missed, `concurrencyPolicy: Forbid` with the previous run still going.
- Schedule wrong: timezone (cron uses server TZ, often UTC; DST shifts), syntax (`0 2 * * 1-5`), day-of-month and day-of-week OR semantics. Verify with crontab.guru.
- Host was down/rebooted or the job is on a node that was replaced; the crontab lives on one server that was rebuilt.
- Airflow: DAG paused, `start_date`/`catchup` semantics (runs at end of interval), scheduler down, pool/slots full.

**Started but failed:**
- Cron's minimal environment: no `PATH`, no env vars, different shell, different working directory. Use absolute paths and source env explicitly.
- Permissions, missing `+x`, `%` in crontab needing escaping.
- Output went to local mail and nobody saw it. Redirect: `>> /var/log/job.log 2>&1`.

**Prevent:** dead-man's-switch monitoring (Healthchecks.io, Cronitor, Datadog check-ins) that alerts when a heartbeat is *missing*, logs shipped centrally, idempotent jobs that can be safely re-run, run on a managed scheduler rather than one pet server.

---

## C. LLM-specific troubleshooting

General rule for LLM issues: **log every call** (model ID, full prompt after templating, parameters, retrieved context, tool calls, raw output, tokens, latency, request ID) and keep an **eval set** of real examples. Without those, LLM debugging is guesswork.

### The LLM suddenly gives worse answers. How do you investigate?

**Clarify:** worse how (wrong facts, format, tone, refusals, shorter)? For which inputs? Since when? Get 5-10 concrete bad examples with request IDs.

**Hypotheses, in order:**
1. **Our change:** prompt template edit, new few-shot examples, a code change that altered how the prompt is assembled (missing variable rendering as empty, truncated context). Diff the *rendered* prompt for a bad request against an older good one.
2. **Model change:** an alias like `-latest` pointing to a new snapshot, a provider model update or deprecation, a different deployment/region, a fallback model kicking in on errors. Check the model ID in the response metadata. Pin exact model versions.
3. **Parameters:** temperature, max tokens, top_p changed; a new default in an SDK upgrade.
4. **Context/RAG:** retrieval returning worse chunks (index rebuilt, new embedding model, data changed, chunking change), context window overflow truncating instructions.
5. **Input drift:** users asking different kinds of questions (new customer, new feature), longer conversations.
6. **Perception:** a few anecdotes vs a real regression. Measure.

**Method:** rerun the eval set (golden questions with expected answers or rubric) against current and previous configurations; A/B the suspect change. Use LLM-as-judge plus human spot checks. Tools: Langfuse, LangSmith, Braintrust, Arize, or plain logs in a warehouse.

**Prevent:** version prompts in git, pin model snapshots, run evals in CI on any prompt/model/retrieval change, monitor online quality signals (thumbs down, retries, escalation rate).

### Responses are getting cut off mid-answer (max tokens? stop sequences?).

**Check the stop/finish reason in the API response first**, it tells you which hypothesis is right:
- `stop_reason: "max_tokens"` (Anthropic) / `finish_reason: "length"` (OpenAI): output hit `max_tokens`. Raise it, ask for more concise output, or continue generation by sending the partial output back and asking it to continue. Also check that input + output fits in the context window; a long prompt leaves less room.
- `stop_reason: "stop_sequence"` / `finish_reason: "stop"` with a matched sequence: a stop sequence like `"\n\n"`, `"###"` or `"</answer>"` appears naturally in the content. Make stop sequences more specific.
- `finish_reason: "content_filter"` or a refusal: safety filtering.
- Normal end turn but text truncated in the UI: **our bug**: streaming client dropped the final chunks (connection closed early, not handling the last event, proxy timeout at 60s), the DB column is `VARCHAR(4000)`, or the UI truncates.

**Commands:** log the raw API response including `usage.output_tokens` and the stop reason; compare the stored text length to `output_tokens`; replay the same request with `curl` and no streaming.

**Prevent:** alert on the rate of `max_tokens` stops, handle truncation explicitly in code (retry/continue, or show "response truncated"), size `max_tokens` from observed output distributions.

### The model ignores part of the system prompt.

**Hypotheses and checks:**
1. **It is not actually there.** Log the final rendered request: the system prompt may be missing, truncated by context management, overridden by a framework, or sent as a user message. Very common.
2. **Conflicting instructions:** two rules contradict each other, or few-shot examples demonstrate behaviour that contradicts the rule (models follow examples over instructions). Retrieved documents or user input containing instructions (including prompt injection) compete with yours.
3. **Buried instruction:** the rule is one line in a 3,000-token prompt, or lost in a long context ("lost in the middle"). Move critical constraints to a clear, dedicated section, and restate key format rules near the end or in the user turn.
4. **Vague or negative phrasing:** "don't be verbose" vs "answer in at most 3 sentences". Explain *why* the rule exists; give positive examples.
5. **Model capability/settings:** a smaller model, high temperature, or a model version change.

**Method:** build a minimal reproduction, then ablate: remove sections of the prompt until the behaviour changes. Test on 20-50 cases, not one.

**Fix:** clarify and structure the prompt (XML tags/sections), add examples that show the rule followed, enforce hard constraints in code (validation, post-processing, structured outputs) rather than prompting alone. **Prevent:** eval cases specifically for each critical instruction.

### The model returns invalid JSON 5% of the time.

**Look at the failures first** and categorize: wrapped in markdown fences, leading prose ("Here is the JSON:"), truncated (check stop reason = max tokens), trailing commas/single quotes, wrong schema (missing fields, wrong types) vs syntactically invalid.

**Fixes, strongest first:**
1. **Use native structured outputs / constrained decoding** (OpenAI `response_format` with a JSON schema in strict mode, Anthropic structured outputs or tool use with an `input_schema`, Gemini `response_schema`). This removes most syntax failures.
2. **Raise `max_tokens`** if truncation is the cause.
3. **Prompt:** give the exact schema and an example, ask for JSON only. Prefill the assistant turn with `{` where supported.
4. **Validate and repair in code:**
```python
import json
from pydantic import BaseModel, ValidationError

class Result(BaseModel):
    label: str
    confidence: float

def parse(text: str) -> Result | None:
    start, end = text.find("{"), text.rfind("}")
    if start == -1 or end == -1:
        return None
    try:
        return Result.model_validate(json.loads(text[start:end + 1]))
    except (json.JSONDecodeError, ValidationError):
        return None
```
5. **Retry** on failure, feeding back the validation error ("Your output failed validation: ... return corrected JSON"), with a cap of 1-2 retries, then a fallback path.
6. Lower temperature for extraction tasks; simplify the schema (fewer nesting levels, enums instead of free text).

**Prevent:** track parse-failure rate as a metric, add failing cases to the eval set.

### The RAG system answers from the wrong document.

Split the pipeline and find which stage fails: **retrieval** (wrong chunks retrieved) or **generation** (right chunks, wrong use).

**Check retrieval** for the failing query: log the top-k chunks with scores and source IDs.
1. **Right doc not retrieved:** chunking split the answer across chunks or merged unrelated content; embedding model weak on domain terms, IDs, product codes (add hybrid search with BM25); query too short or ambiguous (query rewriting); right doc not indexed at all (ingestion failure, stale index).
2. **Similar-looking wrong docs rank higher:** multiple versions of a policy, docs for other customers/products/regions. Add metadata filters (tenant, product, version, date) and prefer latest versions; add a reranker (cross-encoder, Cohere Rerank).
3. **Permission/tenant leak:** a document from the wrong tenant is a security incident; enforce filters at query time, not in the prompt.

**Check generation:** if the right chunk is in context but the answer uses another, the prompt may not tell the model how to choose; include titles/dates/source metadata with each chunk, ask it to cite sources, and reduce k so fewer distractors.

**Measure:** a labelled set of query → correct doc pairs; compute recall@k and MRR for retrieval, faithfulness/citation accuracy for answers (Ragas, custom LLM judge). Change one thing at a time and re-measure.

### Latency jumped from 2s to 15s.

**Break the request into spans** (tracing with OpenTelemetry, Langfuse, or timestamps in logs): pre-processing, retrieval/vector search, each LLM call (time to first token vs total), tool calls, post-processing.

**Hypotheses:**
1. **Output got longer:** latency scales with output tokens. Check `usage.output_tokens` before vs after. A prompt change asking for reasoning or a bigger `max_tokens` can do this.
2. **Input got bigger:** more retrieved chunks, longer conversation history, larger system prompt; prompt caching stopped hitting (a dynamic value like a timestamp placed at the top of the prompt breaks the cache prefix). Check cache-read token counts.
3. **More calls:** an agent now takes more steps, a new chain step, retries on 429/5xx with backoff (check retry logs), sequential calls that could be parallel.
4. **Model change:** larger model, reasoning/extended thinking enabled, fallback to a slower model or region.
5. **Provider-side:** status page, high load; compare time-to-first-token across regions/providers.
6. **Our infra:** vector DB slow, cold starts, connection pool exhaustion, no HTTP connection reuse.

**Fixes:** stream responses for perceived latency, cap output length, prompt caching with stable prefixes, parallelize independent calls, use a smaller model for simple steps, reduce context. **Prevent:** p50/p95 latency and TTFT dashboards per step with alerts.

### Getting lots of 429 rate-limit errors in production.

**Identify which limit:** read the error body and headers (`retry-after`, `x-ratelimit-remaining-requests`, `x-ratelimit-remaining-tokens`, Anthropic `anthropic-ratelimit-*`). Requests per minute, input tokens per minute, output tokens per minute, or concurrent requests? Org-wide or per-key? Some 429/529 errors are provider overload rather than your quota.

**Causes:** traffic growth, a batch job or backfill sharing the same key as production, a retry storm (immediate retries without backoff multiply load), an agent loop, larger prompts consuming the token budget, multiple services sharing one key.

**Fixes:**
- Retries with exponential backoff and jitter, honouring `retry-after`; cap retries.
- Client-side rate limiting/token bucket and a concurrency limit (semaphore) per key, sized to your quota.
- Separate keys/projects for batch vs interactive traffic; move offline work to the provider's batch API (cheaper, separate limits).
- Reduce tokens: prompt caching, smaller context, shorter outputs.
- Queue requests and smooth bursts; degrade gracefully (fallback model, cached answers).
- Request a higher tier/limit increase, or provision throughput (e.g. via Bedrock/Vertex/Azure).

**Prevent:** dashboards of usage vs limits, alerts at 80% of quota, load tests at expected peak.

### Costs doubled this month. Find out why.

**Break down spend** by dimension until the jump is isolated: by model, by API key/project/workspace, by service/feature, by customer/tenant, by day. Provider usage dashboards and cost exports (Anthropic/OpenAI usage pages, AWS Cost Explorer for Bedrock) plus your own per-call logs with token counts and tags.

**Then decompose cost = calls × tokens per call × price per token:**
1. **More calls:** traffic growth (legitimate), a new feature, an agent looping, retry storms, a batch job re-run, abuse or a leaked API key (check key usage by IP/project; rotate if suspicious).
2. **More tokens per call:** bigger prompts (more RAG chunks, full conversation history), longer outputs, prompt caching stopped hitting (cache-read tokens dropped), extended thinking/reasoning enabled.
3. **Higher price per token:** switched to a larger model, a fallback routing to an expensive model, lost batch discount.

**Query example on your logs:**
```sql
SELECT date_trunc('day', ts) d, model, feature,
       count(*) calls, avg(input_tokens) avg_in, avg(output_tokens) avg_out, sum(cost_usd) cost
FROM llm_calls WHERE ts >= now() - interval '60 days'
GROUP BY 1,2,3 ORDER BY 1, cost DESC;
```

**Fixes:** route easy tasks to smaller models, prompt caching, trim context, cap output, batch API for offline work, fix loops. **Prevent:** per-feature and per-tenant cost attribution, daily budget alerts and anomaly detection, hard spend limits per key.

### The agent gets stuck in a loop calling the same tool.

**Look at the trace:** the sequence of tool calls, arguments and results. Is it the same call with identical arguments (not seeing results) or slightly varying ones (not making progress)?

**Hypotheses:**
1. **Tool result is unhelpful:** an error message that does not say what to change, an empty result, or a truncated result, so the model retries. Return actionable errors ("No customer with id 123. Valid ids look like CUST-000123.") and explicit empty results ("0 results").
2. **Tool result not fed back correctly:** a bug where the result is not appended to the conversation, the `tool_use_id` mismatches, or context truncation drops it. The model never sees the answer.
3. **No clear completion criterion:** the prompt does not say when to stop or what "done" looks like.
4. **Ambiguous tools:** overlapping tools or descriptions make the model unsure which to call next.
5. **Context overflow:** long histories get truncated, so the agent forgets it already called the tool.

**Fixes:**
- Hard guardrails in code: max iterations/steps, max identical calls (detect repeated `(tool, args)` hashes), a timeout and token budget; on hitting a limit, stop and return a partial answer or escalate.
- Better tool results and descriptions; tell the model what to do on failure.
- Summarize or compact history for long runs.

**Prevent:** agent eval cases that assert step counts, monitoring of steps-per-task and cost-per-task distributions.

### The agent calls the wrong tool or invents tool arguments.

**Inspect the traces** for failing cases: which tool was chosen, what arguments, and what information was available in context at that point.

**Causes and fixes:**
1. **Poor tool descriptions:** vague names (`get_data`), overlapping tools, no guidance on when to use which. Write descriptions like docs for a new engineer: what it does, when to use it and when not to, parameter meanings and formats, an example.
2. **Too many tools:** dozens of tools degrade selection. Consolidate, or load a relevant subset per task (tool search/routing).
3. **Missing information:** the model invents an ID because it does not have one. Provide a lookup tool (e.g. `search_customers` before `get_customer`), and instruct it to ask the user when required info is missing.
4. **Weak schemas:** use JSON Schema with `enum`s, formats, `required` fields, and strict mode/structured tool calling so arguments must validate.
5. **Validate in code:** reject invalid arguments with an actionable error the model can correct from; never execute destructive actions on unvalidated arguments; require confirmation for writes.
6. **Model/settings:** a smaller model or high temperature; try a stronger model for planning steps.

**Measure:** a tool-selection eval set (input → expected tool and key args), track accuracy as you change descriptions. Prevent with these evals in CI.

### Output differs between staging and production with the same prompt.

**First, confirm "same prompt":** diff the fully rendered requests from both environments (system prompt, messages, tools, parameters). Usually they are not the same.

**Hypotheses:**
1. **Non-determinism:** temperature > 0 means outputs vary by design; even at temperature 0 outputs can differ slightly due to batching/hardware. Compare distributions over many runs, not single outputs.
2. **Different model:** different model ID or version alias, different provider (direct API vs Bedrock/Vertex/Azure deployment with different versions), different region, a fallback active in one environment.
3. **Different parameters:** `max_tokens`, `temperature`, `top_p`, stop sequences set via env-specific config or SDK defaults from different library versions.
4. **Different context:** RAG index differs (staging indexed older/fewer docs), different user/tenant data, conversation history, feature flags enabling different tools.
5. **Prompt template versions:** prod running an older build; prompt pulled from a prompt-management service with env-specific labels.
6. **Post-processing:** different parsing/validation code.

**Fix:** align config via a single source of truth; log model ID and prompt version with each response. **Prevent:** config parity checks in deploy, evals run in both environments, pinned model snapshots.

### Streaming breaks behind the customer's proxy/load balancer.

Symptom patterns: the whole response arrives at once at the end (buffering), the stream stops after ~30-60s (idle timeout), or the connection is reset.

**Hypotheses and fixes:**
1. **Proxy buffering:** nginx buffers upstream responses by default. Set `proxy_buffering off;` for the SSE location, or send header `X-Accel-Buffering: no`. Also disable gzip on `text/event-stream` (compression buffers).
2. **Idle/read timeouts:** ALB idle timeout (60s default), nginx `proxy_read_timeout`, corporate proxy timeouts. Raise them for streaming routes and send periodic heartbeat comments (`: ping\n\n`) every ~15s.
3. **HTTP version / protocol handling:** some corporate proxies and older LBs do not handle chunked transfer encoding or HTTP/2 streaming well; WebSockets may be blocked entirely (missing `Upgrade` headers in nginx: `proxy_set_header Upgrade $http_upgrade; proxy_set_header Connection "upgrade";`).
4. **TLS-inspecting security proxies** (Zscaler etc.) that buffer the full response to scan it. Ask for a bypass rule for your domain, or fall back.
5. **CDN** caching or buffering the response: mark as `Cache-Control: no-cache`, disable buffering on the route.

**Debug:** `curl -N -v https://host/stream` from inside and outside the customer network to see whether chunks arrive incrementally; compare with hitting the backend directly; check proxy logs and response headers added by intermediaries (`Via`, `X-Cache`).

**Prevent:** document network requirements for streaming, implement graceful fallback to non-streaming, and test streaming through a proxy in your integration environment.

---

## D. Debugging process questions

### Walk me through your general debugging process.

1. **Understand the symptom precisely.** Expected vs actual, exact error, who is affected, since when, how often. Get a concrete example (request ID, input, timestamp).
2. **Assess impact and mitigate first** if it is production: rollback, flag off, failover. Root cause can wait an hour; customers cannot.
3. **Reproduce.** Ideally a minimal, fast, reliable reproduction (a failing test). A reproduction turns debugging into a search problem.
4. **Ask "what changed?"** Deploys, config, data, dependencies, traffic, time.
5. **Form ranked hypotheses.** Rank by likelihood and by cost to check. Write them down.
6. **Test one hypothesis at a time** with evidence: logs, metrics, traces, a debugger, adding a log line, `git bisect`. Prefer experiments that eliminate half the search space (binary search through code, time, data or layers).
7. **Find the root cause, not just the trigger.** Ask "why" until you reach something you can fix structurally.
8. **Fix, then verify:** the reproduction now passes, nothing else broke, and production metrics confirm.
9. **Prevent:** a regression test, monitoring/alerting that would have caught it earlier, and sharing the learning (post-mortem or a short write-up).

Throughout: keep notes with timestamps, communicate status, and resist changing several things at once.

### How do you debug something you can't reproduce?

- **Gather every artifact from occurrences:** logs, traces, request IDs, timestamps, user/tenant, inputs, versions, host. Look for what failing cases share (same pod, time of day, input size, browser, concurrent requests).
- **Reason about classes of non-determinism:** concurrency/races, timing and timeouts, caches and state, environment differences, specific data, resource exhaustion under load, clock/timezone, external dependencies.
- **Increase observability where it happens:** add targeted structured logging around the suspect path (inputs, branch taken, durations), enable debug logging for one tenant via flag, add tracing, capture core/heap dumps on failure (`-XX:+HeapDumpOnOutOfMemoryError`, `faulthandler`).
- **Recreate conditions:** replay production traffic or the exact payload, use production-like data volume, run under load (`locust`, `k6`), stress concurrency (many threads, injected delays), chaos/fault injection for dependency failures.
- **Read the code** for paths that could produce the observed state; reasoning from the evidence backwards often finds it.
- **Ship a safe hypothesis fix plus instrumentation** if needed, and confirm by the failure rate dropping to zero over a meaningful period.

Be honest with stakeholders: "we have not reproduced it yet; here is what we have ruled out and what we added to catch it next time."

### How do you debug in a customer environment where you have limited access?

**Typical constraints:** no direct shell, no production data access, air-gapped networks, change-control windows, screen-share only, every command goes through the customer's admin.

**Tactics:**
- **Prepare before you need it:** ship the product with good diagnostics: structured logs with correlation IDs, a `/health` and `/version` endpoint, a "support bundle" command that collects logs, config (secrets redacted), versions, and connectivity checks into one archive.
- **Make requests precise and batched:** send the customer a short script or a numbered list of exact commands with expected output, so one round trip answers many hypotheses (`kubectl describe`, `kubectl logs --previous`, `curl -v`, `nslookup`, `openssl s_client`, `env | grep -i proxy`).
- **Screen-share and drive verbally;** ask them to paste output into the ticket for a written record.
- **Reproduce on your side:** mirror their setup (versions, proxy, custom CA, OS, Kubernetes distribution like OpenShift) in a lab environment; ask for sanitized sample data.
- **Respect security:** never ask for credentials in chat, use their approved channels, follow change-control, and get approval before running anything that writes.
- **Build trust:** explain what each command does and why.

Prevent: document environment requirements and a troubleshooting guide; add any diagnostic you wished you had to the support bundle.

### What tools do you use (debuggers, profilers, logs, tracing, `strace`, `tcpdump`)?

Group them by question:
- **What is the code doing?** Debuggers: `pdb`/`breakpoint()`, `debugpy` with VS Code, IntelliJ debugger, `node --inspect`. Remote attach when needed.
- **Where is the time/CPU going?** Profilers: `cProfile` + `snakeviz`, `py-spy` (attach to a live process without restart, flame graphs), `perf`, async-profiler for JVM, Chrome DevTools Performance; continuous profilers (Pyroscope, Datadog).
- **Where is the memory going?** `tracemalloc`, `memray`, heap dumps with Eclipse MAT, Chrome heap snapshots.
- **What happened across services?** Centralized logs (ELK/OpenSearch, Loki, Datadog, CloudWatch Logs Insights) with request IDs; distributed tracing (OpenTelemetry, Jaeger, Honeycomb); metrics and dashboards (Prometheus/Grafana); error tracking (Sentry).
- **What is the process doing at the OS level?** `strace -f -p <pid>` (syscalls: stuck on `connect`, `read` from a socket, missing files via `ENOENT`, permission `EACCES`), `ltrace`, `lsof -p <pid>` (open files and sockets), `top`/`htop`, `vmstat`, `iostat -x`, `ss -tanp` (connections and states).
- **What is on the wire?** `curl -v`, `dig`, `mtr`, `nc`, `openssl s_client`, `tcpdump -i any -nn host 10.0.0.5 and port 5432 -w cap.pcap` analysed in Wireshark, `mitmproxy` for HTTP(S).
- **Containers/K8s:** `kubectl describe/logs/events/top/debug`, `k9s`, `docker inspect`.
- **History:** `git log -S`, `git blame`, `git bisect`.

Point to make: pick the tool by the question you need answered, and prefer non-invasive tools in production.

### How do you use `git bisect`?

`git bisect` binary-searches commit history to find the commit that introduced a regression: about log2(N) steps (1,000 commits in ~10).

```bash
git bisect start
git bisect bad                 # current HEAD is broken
git bisect good v2.3.0         # last known good tag or commit
# git checks out a midpoint; test it, then mark:
git bisect good                # or: git bisect bad
# ...repeat until it prints "<sha> is the first bad commit"
git bisect reset               # return to where you started
```

**Automate it** with a script that exits 0 for good, 1-127 for bad, and 125 to skip an untestable commit:
```bash
git bisect run pytest -x tests/test_invoice.py::test_total
```

**Tips:**
- You need a reliable, fast test for "bad"; flaky tests mislead bisect.
- Use `git bisect skip` for commits that do not build.
- `git bisect log` / `git bisect replay` to record and redo a session.
- Works on any repeatable check, including performance ("bad if runtime > 2s") or output differences.
- Once found, read the commit diff to understand *why* it broke, then fix forward or revert.

### How do you decide between a quick fix and a root-cause fix during an incident?

During an active incident, **restore service first, root-cause second.** The decision factors:
- **Customer impact and duration:** if users are down, take the fastest safe mitigation (rollback, feature flag off, scale up, failover, restart, block bad traffic).
- **Risk of the fix:** prefer reversible, well-understood actions. A rollback is lower risk than writing new code under pressure. A "quick fix" that is a novel code change at 3 a.m. can make things worse.
- **Confidence in diagnosis:** if the root cause is clear and the fix is small, tested and low-risk, shipping it directly can be fine.
- **Data integrity:** if data is being corrupted, stop the bleeding immediately (pause the job, read-only mode) even at the cost of availability.

**Then, always:**
- Capture evidence before mitigation destroys it (thread dumps, logs, heap dumps).
- Track the quick fix as temporary: a ticket with an owner and a date for the root-cause fix, discussed in the post-mortem.
- Do the root-cause fix through the normal process (review, tests, staged rollout).

Phrase for the interview: "Mitigate fast, fix properly, and never let the workaround become the permanent solution by accident."

### What goes into a good post-mortem?

A good post-mortem is **blameless** (focus on systems and processes, not people) and produces **actionable follow-ups**.

**Structure:**
1. **Summary:** what happened, in two or three sentences.
2. **Impact:** who and what was affected, duration, quantified (requests failed, customers affected, revenue, SLA/SLO breach, data loss).
3. **Timeline** (UTC timestamps): trigger, detection, key decisions, mitigation, resolution. Highlight time-to-detect and time-to-mitigate.
4. **Root cause and contributing factors:** use "5 whys" or a causal analysis; usually several factors combine (a bug plus a missing alert plus an unclear runbook).
5. **Detection:** how we found out (alert vs customer report) and how we could have found out sooner.
6. **What went well / what went poorly / where we got lucky.**
7. **Action items:** specific, owned, prioritized, with due dates and tickets. Mix of prevent (fix the bug class), detect (alerts), and mitigate (runbooks, faster rollback).
8. **Lessons learned** shared broadly.

**Process:** draft within a few days while memory is fresh, review in a meeting with participants, track action items to completion, and share a customer-facing version (RCA) when customers were affected: what happened, impact, what we are doing so it does not recur, without internal blame or jargon.

### How do you communicate with the customer during an outage?

**Principles:** fast, honest, regular, and plain language.
- **Acknowledge early:** first message within ~15 minutes of confirmation, even with little information. Silence is worse than "we are investigating."
- **Each update covers:** what is affected (in customer terms, e.g. "dashboard exports fail"), what is not affected, what we are doing, any workaround, and **when the next update will come**. Then keep that promise even if there is no news.
- **One channel and one voice:** a status page plus the agreed channel (shared Slack, email, call bridge) with a single comms owner, so engineers can focus on fixing.
- **Do not speculate** about root cause or give ETAs you cannot back. Say "we expect an update by 14:30 UTC" rather than "fixed in 10 minutes."
- **Tailor depth:** executives want impact and ETA; their engineers may want technical detail and can help (e.g. confirm from their side).
- **Resolution message:** confirm recovery, ask them to verify on their side, and commit to an RCA by a specific date.
- **Follow-up:** deliver the RCA, explain prevention steps, and handle SLA credits if applicable.

Example update: "14:05 UTC: Report generation is failing for all users since 13:40 UTC. Data ingestion is unaffected. We have identified a faulty release and are rolling it back. Next update by 14:35 UTC or sooner."

### How do you prevent the same bug from happening again?

Fix the **class** of bug, not just the instance:
1. **Regression test** that reproduces the exact failure and now passes; add it to CI.
2. **Fix the root cause and look for siblings:** grep the codebase for the same pattern (e.g. other unparameterized queries, other caches without bounds).
3. **Make the bug impossible or loud:** types and validation at boundaries (pydantic schemas, DB constraints like `UNIQUE`/`NOT NULL`/foreign keys), linters and static analysis rules (Semgrep, Ruff, mypy), safer library defaults, idempotent operations.
4. **Detection:** add the metric or alert that would have caught it earlier (SLO burn-rate alerts, data quality tests, dead-man's switches, cert expiry alerts).
5. **Process:** update the checklist, runbook or PR template; add a canary/staged rollout or feature flags so future regressions affect fewer users; automate a manual step that went wrong.
6. **Share the knowledge:** post-mortem, a short internal write-up, onboarding docs.
7. **Track follow-ups to completion:** action items with owners and dates, reviewed in a regular ops meeting.

The interview point: "A bug that happened once is an event; a bug that happens twice is a process failure."

# 02 — Coding: Model Answers

Questions: [README.md](README.md)

Section B (classic DS&A topic checklist) is answered separately in [ANSWERS-dsa-topics.md](ANSWERS-dsa-topics.md). This file covers sections A, C and D.

**How to use these answers:** these are reference answers, not scripts. Re-type the code yourself, adapt the explanations to your own experience, and practise saying the approach, complexity and trade-offs out loud before you write any code.

---

## A. Reported FDE coding problems

### Check whether a string is a palindrome (and variants: ignore punctuation, valid palindrome II).

**Approach:** two pointers from both ends moving inward. For the "ignore punctuation" variant, skip non-alphanumeric characters and compare lowercased. For Valid Palindrome II (at most one deletion), on the first mismatch try skipping either the left or the right character and check the remainder.

```python
def is_palindrome(s: str) -> bool:
    i, j = 0, len(s) - 1
    while i < j:
        if not s[i].isalnum():
            i += 1
        elif not s[j].isalnum():
            j -= 1
        elif s[i].lower() != s[j].lower():
            return False
        else:
            i += 1; j -= 1
    return True

def valid_palindrome_ii(s: str) -> bool:
    def is_pal(i, j):
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1; j -= 1
        return True
    i, j = 0, len(s) - 1
    while i < j:
        if s[i] != s[j]:
            return is_pal(i + 1, j) or is_pal(i, j - 1)
        i += 1; j -= 1
    return True
```

**Complexity:** O(n) time, O(1) extra space for both.

**Edge cases:** empty string and single char (true); string of only punctuation (true); unicode (`isalnum` and `lower` handle most; use `casefold()` for German ß-style cases).

**Follow-ups:** at most k deletions (DP on longest palindromic subsequence, O(n^2)); longest palindromic substring (expand around centre); palindrome linked list (find middle, reverse second half).

### Find a path on a grid where 1s are walls (BFS/DFS shortest path; with k wall removals).

**Approach:** BFS gives the shortest path in an unweighted grid. DFS finds *a* path but not the shortest. With k removals, the state becomes `(row, col, removals_left)`; BFS over that expanded state space.

```python
from collections import deque

def shortest_path(grid, k=0):
    """Min steps from top-left to bottom-right; may remove up to k walls. -1 if impossible."""
    if not grid or not grid[0]:
        return -1
    R, C = len(grid), len(grid[0])
    if k >= R + C - 1:                      # enough to clear every cell on a shortest path
        return R + C - 2
    start_k = k - grid[0][0]
    if start_k < 0:
        return -1
    q = deque([(0, 0, start_k, 0)])
    best = {(0, 0): start_k}                # max removals left seen at each cell
    while q:
        r, c, rem, d = q.popleft()
        if (r, c) == (R - 1, C - 1):
            return d
        for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C:
                nrem = rem - grid[nr][nc]
                if nrem >= 0 and nrem > best.get((nr, nc), -1):
                    best[(nr, nc)] = nrem
                    q.append((nr, nc, nrem, d + 1))
    return -1
```

**Complexity:** O(R·C·k) time and space in the worst case (each cell can be visited once per removal budget). With k=0 it is O(R·C).

**Edge cases:** start or end is a wall; 1x1 grid (answer 0); no path; empty grid.

**Follow-ups:** return the actual path (store parent pointers); weighted cells (Dijkstra); 8-directional movement; very large grids (A* with Manhattan heuristic, bidirectional BFS).

### Find time intervals where multiple people are all free (Scale AI OA, "employee free time").

**Approach:** flatten all busy intervals, sort by start, merge them. Gaps between merged intervals are times when everyone is free. Variant "find slots where at least m people are free / meeting of duration d within working hours": use a sweep line over +1/-1 events.

```python
def employee_free_time(schedules):
    """schedules: list per person of [start, end) busy intervals. Returns common free gaps."""
    busy = sorted(iv for person in schedules for iv in person)
    free, end = [], None
    for s, e in busy:
        if end is not None and s > end:
            free.append((end, s))
        end = e if end is None else max(end, e)
    return free

def common_free_slots(schedules, day_start, day_end, duration):
    """Variant: free slots inside working hours that are at least `duration` long."""
    busy = sorted(iv for p in schedules for iv in p)
    out, cur = [], day_start
    for s, e in busy:
        if s >= day_end:
            break
        if s - cur >= duration:
            out.append((cur, s))
        cur = max(cur, e)
    if day_end - cur >= duration:
        out.append((cur, day_end))
    return out
```

**Complexity:** O(N log N) for the sort, N = total intervals; O(N) space. A heap-based k-way merge gives O(N log P) if each person's list is already sorted (P = people).

**Edge cases:** touching intervals `[1,3]` and `[3,5]` (no gap; clarify open/closed ends); empty schedules; one person; times as strings like "09:30" (convert to minutes); time zones.

**Follow-ups:** "at least m of n people free" (sweep line counting busy people); pick the earliest slot; streaming calendars; recurring events.

### Rate limiter supporting per-user **and** global limits (token bucket / sliding window).

**Approach:** a token bucket per user plus one global bucket. A request is allowed only if both buckets have a token; check both first, then consume both, so a rejected request does not burn a token. Use a lock for thread safety and an injectable clock for tests.

```python
import threading, time

class TokenBucket:
    def __init__(self, capacity: float, refill_per_sec: float, now: float):
        self.capacity, self.rate = capacity, refill_per_sec
        self.tokens, self.last = capacity, now

    def _refill(self, now):
        self.tokens = min(self.capacity, self.tokens + (now - self.last) * self.rate)
        self.last = now

class RateLimiter:
    def __init__(self, user_cap, user_rate, global_cap, global_rate, clock=time.monotonic):
        self.clock = clock
        self.user_cfg = (user_cap, user_rate)
        self.global_bucket = TokenBucket(global_cap, global_rate, clock())
        self.users: dict[str, TokenBucket] = {}
        self.lock = threading.Lock()

    def allow(self, user_id: str, cost: float = 1.0) -> bool:
        with self.lock:
            now = self.clock()
            ub = self.users.get(user_id)
            if ub is None:
                ub = self.users[user_id] = TokenBucket(*self.user_cfg, now)
            ub._refill(now); self.global_bucket._refill(now)
            if ub.tokens >= cost and self.global_bucket.tokens >= cost:
                ub.tokens -= cost; self.global_bucket.tokens -= cost
                return True
            return False
```

Sliding-window-log alternative (exact, more memory): keep a deque of timestamps per user; drop those older than the window; allow if `len < limit`.

**Complexity:** token bucket O(1) time per call, O(U) space for U users. Sliding log is O(limit) memory per user.

**Edge cases:** clock going backwards (use `monotonic`); burst at start (bucket starts full); cost > capacity (always rejected, so validate); unbounded user map (evict idle users with an LRU/TTL).

**Follow-ups:** distributed version (Redis with a Lua script doing refill+consume atomically, or a sliding window with sorted sets); per-tier limits from config; return `Retry-After` (time until enough tokens: `(cost - tokens)/rate`); fixed-window vs sliding-window-counter trade-offs.

### LRU cache (and LFU cache, TTL cache).

**Approach:**
- **LRU:** hash map + doubly linked list; in Python, `OrderedDict` gives both. `get` moves to the end; `put` evicts from the front.
- **LFU:** map key -> (value, freq), map freq -> OrderedDict of keys, track `min_freq`. Evict the least-recently-used key in the `min_freq` bucket.
- **TTL:** store an expiry per key; expire lazily on read, plus an optional heap or periodic sweep to reclaim memory.

```python
from collections import OrderedDict, defaultdict
import time

class LRUCache:
    def __init__(self, capacity: int):
        self.cap, self.d = capacity, OrderedDict()
    def get(self, key):
        if key not in self.d:
            return -1
        self.d.move_to_end(key)
        return self.d[key]
    def put(self, key, value):
        if key in self.d:
            self.d.move_to_end(key)
        self.d[key] = value
        if len(self.d) > self.cap:
            self.d.popitem(last=False)

class LFUCache:
    def __init__(self, capacity: int):
        self.cap, self.min_freq = capacity, 0
        self.kv = {}                                  # key -> (value, freq)
        self.buckets = defaultdict(OrderedDict)       # freq -> keys in LRU order
    def _touch(self, key):
        val, f = self.kv[key]
        del self.buckets[f][key]
        if not self.buckets[f]:
            del self.buckets[f]
            if self.min_freq == f:
                self.min_freq = f + 1
        self.kv[key] = (val, f + 1)
        self.buckets[f + 1][key] = None
    def get(self, key):
        if key not in self.kv:
            return -1
        self._touch(key)
        return self.kv[key][0]
    def put(self, key, value):
        if self.cap <= 0:
            return
        if key in self.kv:
            self.kv[key] = (value, self.kv[key][1]); self._touch(key); return
        if len(self.kv) >= self.cap:
            old, _ = self.buckets[self.min_freq].popitem(last=False)
            if not self.buckets[self.min_freq]:
                del self.buckets[self.min_freq]
            del self.kv[old]
        self.kv[key] = (value, 1); self.buckets[1][key] = None; self.min_freq = 1

class TTLCache:
    def __init__(self, clock=time.monotonic):
        self.d, self.clock = {}, clock
    def set(self, key, value, ttl: float):
        self.d[key] = (value, self.clock() + ttl)
    def get(self, key, default=None):
        item = self.d.get(key)
        if item is None:
            return default
        value, exp = item
        if self.clock() >= exp:
            del self.d[key]
            return default
        return value
```

**Complexity:** O(1) per operation for LRU and LFU; TTL O(1) per op with lazy expiry (O(log n) if you add an expiry heap). Space O(capacity).

**Edge cases:** capacity 0; `put` on existing key updates value and recency; LFU tie-break by recency; TTL of 0 or negative.

**Follow-ups:** thread safety (one lock, or lock striping); write the doubly linked list yourself (interviewers often ask); size-bounded by bytes, not count; distributed cache (Redis, consistent hashing).

### In-memory key-value store with transactions (`BEGIN`, `ROLLBACK`, `COMMIT`) and TTLs.

**Approach:** keep a base dict plus a stack of transaction layers. Writes go to the top layer; reads walk layers top-down, then the base. A delete is a tombstone in the layer. `ROLLBACK` pops the top layer; `COMMIT` merges all layers into the base (or merges the top into the one below, for nested commit semantics; clarify). TTL: store `(value, expires_at)` and treat expired as missing.

```python
import time
_TOMB = object()

class KVStore:
    def __init__(self, clock=time.monotonic):
        self.base, self.tx, self.clock = {}, [], clock

    def _lookup(self, key):
        for layer in reversed(self.tx):
            if key in layer:
                return layer[key]
        return self.base.get(key)

    def get(self, key):
        item = self._lookup(key)
        if item is None or item is _TOMB:
            return None
        value, exp = item
        if exp is not None and self.clock() >= exp:
            return None
        return value

    def set(self, key, value, ttl=None):
        exp = self.clock() + ttl if ttl is not None else None
        (self.tx[-1] if self.tx else self.base)[key] = (value, exp)

    def delete(self, key):
        if self.tx:
            self.tx[-1][key] = _TOMB
        else:
            self.base.pop(key, None)

    def begin(self):
        self.tx.append({})

    def rollback(self):
        if not self.tx:
            raise RuntimeError("NO TRANSACTION")
        self.tx.pop()

    def commit(self):
        if not self.tx:
            raise RuntimeError("NO TRANSACTION")
        for layer in self.tx:                      # apply oldest first
            for k, v in layer.items():
                if v is _TOMB:
                    self.base.pop(k, None)
                else:
                    self.base[k] = v
        self.tx.clear()
```

**Complexity:** get O(d) where d = transaction depth (usually tiny); set/delete O(1); commit O(total writes in open transactions). Space O(keys + writes in transactions).

**Edge cases:** rollback/commit with no transaction; delete of a missing key; nested transactions; TTL expiring mid-transaction; setting a key with TTL then overwriting without TTL.

**Follow-ups:** `COUNT value` in O(1) (maintain a value -> count map, adjusted per layer); snapshot isolation for concurrent clients (MVCC: versioned values); persistence (write-ahead log + periodic snapshot); background expiry sweep.

### In-memory database / bank system with progressive levels (deposit, transfer, top spenders, scheduled payments, merge accounts).

This is the classic CodeSignal "progressive" format: each level adds requirements, so design for extension from level 1. All operations take a `timestamp`; process scheduled work before handling each call.

**Approach:**
- `Account` holds balance, total outgoing, and balance history (for "balance at time t").
- Level 1: create, deposit, transfer (validate existence, distinct accounts, funds).
- Level 2: top-n spenders by outgoing total, ties by id: sort, or a heap for O(A log n).
- Level 3: scheduled payments in a min-heap keyed by `(execute_at, seq)`; process due ones at the start of every operation; cancel by marking.
- Level 4: merge accounts: move balance, outgoing totals and pending payments. If `balance_at` must still answer for the absorbed account's past, keep its history (e.g. a `merged_into` map plus the merge timestamp) instead of deleting it; the code below simply drops it.

```python
import heapq
from dataclasses import dataclass, field

@dataclass
class Account:
    id: str
    balance: int = 0
    outgoing: int = 0
    history: list = field(default_factory=list)   # (timestamp, balance)

class Bank:
    def __init__(self):
        self.accounts: dict[str, Account] = {}
        self.pending = []              # heap of (execute_at, seq, payment_id)
        self.payments = {}             # payment_id -> dict
        self.seq = 0

    # --- helpers
    def _record(self, acc, ts):
        acc.history.append((ts, acc.balance))

    def _process_due(self, ts):
        while self.pending and self.pending[0][0] <= ts:
            _, _, pid = heapq.heappop(self.pending)
            p = self.payments[pid]
            if p["status"] != "pending":
                continue
            acc = self.accounts.get(p["account"])
            if acc and acc.balance >= p["amount"]:
                acc.balance -= p["amount"]; acc.outgoing += p["amount"]
                self._record(acc, p["execute_at"]); p["status"] = "done"
            else:
                p["status"] = "failed"

    # --- level 1
    def create_account(self, ts, acc_id) -> bool:
        self._process_due(ts)
        if acc_id in self.accounts:
            return False
        self.accounts[acc_id] = Account(acc_id); self._record(self.accounts[acc_id], ts)
        return True

    def deposit(self, ts, acc_id, amount):
        self._process_due(ts)
        acc = self.accounts.get(acc_id)
        if acc is None or amount <= 0:
            return None
        acc.balance += amount; self._record(acc, ts)
        return acc.balance

    def transfer(self, ts, src, dst, amount):
        self._process_due(ts)
        a, b = self.accounts.get(src), self.accounts.get(dst)
        if not a or not b or src == dst or amount <= 0 or a.balance < amount:
            return None
        a.balance -= amount; a.outgoing += amount; b.balance += amount
        self._record(a, ts); self._record(b, ts)
        return a.balance

    # --- level 2
    def top_spenders(self, ts, n):
        self._process_due(ts)
        top = heapq.nsmallest(n, self.accounts.values(), key=lambda a: (-a.outgoing, a.id))
        return [f"{a.id}({a.outgoing})" for a in top]

    # --- level 3
    def schedule_payment(self, ts, acc_id, amount, delay):
        self._process_due(ts)
        if acc_id not in self.accounts:
            return None
        self.seq += 1
        pid = f"payment{self.seq}"
        self.payments[pid] = {"account": acc_id, "amount": amount,
                              "execute_at": ts + delay, "status": "pending"}
        heapq.heappush(self.pending, (ts + delay, self.seq, pid))
        return pid

    def cancel_payment(self, ts, acc_id, pid) -> bool:
        self._process_due(ts)
        p = self.payments.get(pid)
        if not p or p["account"] != acc_id or p["status"] != "pending":
            return False
        p["status"] = "cancelled"
        return True

    # --- level 4
    def merge_accounts(self, ts, keep, absorb) -> bool:
        self._process_due(ts)
        a, b = self.accounts.get(keep), self.accounts.get(absorb)
        if not a or not b or keep == absorb:
            return False
        a.balance += b.balance; a.outgoing += b.outgoing
        for p in self.payments.values():
            if p["account"] == absorb and p["status"] == "pending":
                p["account"] = keep
        del self.accounts[absorb]; self._record(a, ts)
        return True

    def balance_at(self, ts, acc_id, time_at):
        self._process_due(ts)
        acc = self.accounts.get(acc_id)
        if not acc:
            return None
        import bisect
        i = bisect.bisect_right(acc.history, (time_at, float("inf"))) - 1
        return acc.history[i][1] if i >= 0 else None
```

**Complexity:** deposit/transfer O(1) amortised plus due-payment processing O(log P) each; top spenders O(A log n); balance_at O(log H) via binary search on history.

**Edge cases:** same timestamp for scheduled payment and query (process payments first, ordered by creation); insufficient funds at execution time; transfer to self; merged account ids reused later; integer vs float money (use integer cents or `Decimal`).

**Follow-ups and tips:** keep each level's tests passing as you go; write helpers (`_get_or_none`, `_process_due`) early; clarify tie-break rules and return formats exactly, because the graders compare strings.

### File system: `mkdir`, `ls`, `addContentToFile`, `readContentFromFile`.

**Approach:** a trie of nodes. Each node has `children` (sorted on output) and `content` (non-None means it is a file). Walk the path split by `/`, creating nodes as needed for `mkdir` and `addContentToFile`.

```python
class Node:
    __slots__ = ("children", "content")
    def __init__(self):
        self.children: dict[str, "Node"] = {}
        self.content: str | None = None      # None = directory

class FileSystem:
    def __init__(self):
        self.root = Node()

    def _walk(self, path: str, create: bool = False) -> Node:
        node = self.root
        for part in filter(None, path.split("/")):
            if part not in node.children:
                if not create:
                    raise FileNotFoundError(path)
                node.children[part] = Node()
            node = node.children[part]
        return node

    def ls(self, path: str) -> list[str]:
        node = self._walk(path)
        if node.content is not None:                # a file: return its name
            return [path.rstrip("/").split("/")[-1]]
        return sorted(node.children)

    def mkdir(self, path: str) -> None:
        self._walk(path, create=True)

    def addContentToFile(self, path: str, content: str) -> None:
        node = self._walk(path, create=True)
        node.content = (node.content or "") + content

    def readContentFromFile(self, path: str) -> str:
        node = self._walk(path)
        if node.content is None:
            raise IsADirectoryError(path)
        return node.content
```

**Complexity:** each op O(L) for path length L, plus O(k log k) to sort k children in `ls`. Space O(total path components + content).

**Edge cases:** root `/`; trailing slashes; `ls` on a file; creating a file where a directory exists; empty content appends.

**Follow-ups:** `rm`, `mv` (detach and reattach subtree), size of directory (cache sizes and update along the path), permissions, wildcard search, keeping children in a sorted container to avoid sorting each `ls`.

### Web crawler (single-threaded → multi-threaded, same domain only).

**Approach:** BFS from the start URL with a `visited` set; only enqueue links whose hostname matches. For concurrency, use a thread pool with a lock-protected `visited` set and a work queue (or asyncio with a semaphore). Fetching is I/O-bound, so threads or async help despite the GIL.

```python
from urllib.parse import urlparse, urljoin, urldefrag
from concurrent.futures import ThreadPoolExecutor, FIRST_COMPLETED, wait
import threading

def _norm(base, href):
    return urldefrag(urljoin(base, href))[0]

def crawl(start_url, get_urls, max_workers=8, max_pages=10_000):
    """get_urls(url) -> list of hrefs on that page (injected, e.g. HtmlParser.getUrls)."""
    host = urlparse(start_url).hostname
    visited = {start_url}
    lock = threading.Lock()

    def fetch(url):
        try:
            return [_norm(url, h) for h in get_urls(url)]
        except Exception:
            return []                                # log and continue

    with ThreadPoolExecutor(max_workers) as pool:
        futures = {pool.submit(fetch, start_url)}
        while futures:
            done, futures = wait(futures, return_when=FIRST_COMPLETED)
            for f in done:
                for link in f.result():
                    if urlparse(link).hostname != host:
                        continue
                    with lock:
                        if link in visited or len(visited) >= max_pages:
                            continue
                        visited.add(link)
                    futures.add(pool.submit(fetch, link))
    return visited
```

The single-threaded version is the same loop with a `deque` instead of futures. Here the lock is technically unnecessary because only the main thread touches `visited`; keep it if workers enqueue directly.

**Complexity:** O(V + E) pages and links; wall time roughly (V / workers) × latency. Space O(V).

**Edge cases:** relative links, fragments (`#a`), trailing slashes, query parameters producing infinite URL spaces (cap pages and depth), redirects to another domain, `http` vs `https`, non-HTML responses.

**Follow-ups:** politeness (robots.txt, per-host rate limit, backoff on 429); asyncio + aiohttp version; distributed crawler (shared frontier in Kafka/Redis, visited set as a Bloom filter, partition by host); content dedup via hashing.

### Parse a log file and return top-K errors / endpoints / users.

**Approach:** stream the file line by line (never load it all), parse with a regex, count with a `Counter`, then take top-K with a heap (`most_common(k)` uses `heapq.nlargest`). Count and skip malformed lines.

```python
import re, heapq
from collections import Counter

LINE = re.compile(
    r'(?P<ts>\S+ \S+) (?P<level>[A-Z]+) (?P<user>\S+) (?P<method>[A-Z]+) (?P<path>\S+) (?P<status>\d{3})'
)

def top_k(path, k=10, field="path", only_errors=False):
    counts, bad = Counter(), 0
    with open(path, encoding="utf-8", errors="replace") as f:
        for line in f:
            m = LINE.match(line)
            if not m:
                bad += 1
                continue
            if only_errors and int(m["status"]) < 500:
                continue
            key = m[field]
            if field == "path":
                key = re.sub(r"/\d+", "/{id}", key.split("?")[0])   # normalise ids
            counts[key] += 1
    top = heapq.nlargest(k, counts.items(), key=lambda kv: (kv[1], kv[0]))
    return top, bad
```

**Complexity:** O(N) to scan N lines, O(U log k) for top-K over U unique keys. Space O(U).

**Edge cases:** malformed lines, multi-line entries (stack traces), mixed encodings, gzip files (`gzip.open`), ties (define order), path normalisation (`/users/123` vs `/users/456`).

**Follow-ups:** file too big for memory of unique keys (Count-Min Sketch + heap, or MapReduce: hash-partition by key, count per partition, merge top-K); time-windowed top-K (bucket counts per minute); real-time version (Kafka + Flink); JSON logs (`json.loads` per line).

### Stack-trace / log deduplication.

**Approach:** compute a *fingerprint* per error by normalising away the parts that vary between occurrences, then group by fingerprint (this is what Sentry does).
- Normalise the message: replace numbers, hex addresses, UUIDs, quoted strings, timestamps with placeholders.
- For stack traces, fingerprint on exception type + the top N frames (file:function), ignoring line numbers (optional) and framework frames.
- Group with a dict: fingerprint -> count, first_seen, last_seen, one sample.

```python
import re, hashlib
from collections import defaultdict

PATTERNS = [
    (re.compile(r"0x[0-9a-fA-F]+"), "<HEX>"),
    (re.compile(r"\b[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}\b"), "<UUID>"),
    (re.compile(r"'[^']*'|\"[^\"]*\""), "<STR>"),
    (re.compile(r"\d+"), "<N>"),
]
FRAME = re.compile(r'File "(?P<file>[^"]+)", line \d+, in (?P<func>\S+)')

def normalise(msg: str) -> str:
    for pat, rep in PATTERNS:
        msg = pat.sub(rep, msg)
    return msg

def fingerprint(trace: str, top_frames: int = 5) -> str:
    lines = trace.strip().splitlines()
    exc = normalise(lines[-1]) if lines else ""
    frames = [f"{m['file']}:{m['func']}" for m in FRAME.finditer(trace)][-top_frames:]
    return hashlib.sha1("|".join([exc, *frames]).encode()).hexdigest()

def group_traces(traces):
    groups = defaultdict(lambda: {"count": 0, "sample": None})
    for t in traces:
        g = groups[fingerprint(t)]
        g["count"] += 1
        g["sample"] = g["sample"] or t
    return sorted(groups.values(), key=lambda g: -g["count"])
```

Splitting a raw log into separate traces: start a new record on lines that match the log-line prefix (timestamp/level); append indented or `Traceback`/`at ...` lines to the current record.

**Complexity:** O(total characters) to normalise and hash; O(G) space for G groups.

**Edge cases:** chained exceptions ("During handling of the above..."), recursion (collapse repeated frames), truncated traces, Java `at com.foo...` format, different line numbers after a deploy (which is why you may drop line numbers).

**Follow-ups:** near-duplicates (MinHash/SimHash on token shingles, or embeddings + clustering); streaming with a time window; alert when a new fingerprint appears.

### Design and implement a task scheduler with dependencies (topological sort).

**Approach:** model tasks as a DAG; Kahn's algorithm. Indegree counts; start with indegree 0 tasks; when a task finishes, decrement dependents. If not all tasks are scheduled, there is a cycle. For parallel execution, everything in the ready queue can run concurrently; with durations, the earliest finish time per task = max(finish of deps) + duration.

```python
from collections import defaultdict, deque

def schedule(tasks, deps):
    """tasks: iterable of ids; deps: list of (before, after). Returns execution order in 'waves'."""
    indeg = {t: 0 for t in tasks}
    out = defaultdict(list)
    for a, b in deps:
        out[a].append(b); indeg[b] += 1
    ready = deque(sorted(t for t, d in indeg.items() if d == 0))
    waves, done = [], 0
    while ready:
        wave = list(ready); ready.clear()
        waves.append(wave)
        for t in wave:
            done += 1
            for nxt in out[t]:
                indeg[nxt] -= 1
                if indeg[nxt] == 0:
                    ready.append(nxt)
    if done != len(indeg):
        cyclic = [t for t, d in indeg.items() if d > 0]
        raise ValueError(f"cycle among: {cyclic}")
    return waves

def min_total_time(durations: dict, deps):
    """Critical path length with unlimited workers."""
    finish = {}
    for wave in schedule(durations, deps):
        for t in wave:
            finish[t] = durations[t] + max((finish[a] for a, b in deps if b == t), default=0)
    return max(finish.values(), default=0)
```

(`min_total_time` is O(V·E) as written; precompute a reverse adjacency list to make it O(V+E).)

**Complexity:** O(V + E) time and space.

**Edge cases:** dependency on an unknown task (validate or auto-add); self-dependency; duplicate edges; empty input; disconnected components.

**Follow-ups:** limited workers (simulate with a min-heap of finish times); retries and failure propagation (mark dependents as skipped); priorities (ready queue becomes a heap); dynamic task addition; persistence (this is basically Airflow/Temporal).

### Merge overlapping intervals; insert interval; meeting rooms I & II.

```python
import heapq

def merge(intervals):
    out = []
    for s, e in sorted(intervals):
        if out and s <= out[-1][1]:
            out[-1][1] = max(out[-1][1], e)
        else:
            out.append([s, e])
    return out

def insert(intervals, new):          # intervals already sorted and non-overlapping
    out, i, n = [], 0, len(intervals)
    s, e = new
    while i < n and intervals[i][1] < s:
        out.append(intervals[i]); i += 1
    while i < n and intervals[i][0] <= e:
        s, e = min(s, intervals[i][0]), max(e, intervals[i][1]); i += 1
    out.append([s, e])
    return out + intervals[i:]

def can_attend_all(intervals):       # meeting rooms I
    iv = sorted(intervals)
    return all(iv[i][0] >= iv[i - 1][1] for i in range(1, len(iv)))

def min_rooms(intervals):            # meeting rooms II
    heap = []                        # end times of rooms in use
    for s, e in sorted(intervals):
        if heap and heap[0] <= s:
            heapq.heapreplace(heap, e)
        else:
            heapq.heappush(heap, e)
    return len(heap)
```

**Complexity:** merge O(n log n); insert O(n) (already sorted); rooms I O(n log n); rooms II O(n log n) time, O(n) heap.

**Edge cases:** touching intervals (`[1,2]`,`[2,3]`: merge or not? meetings ending at 2 free the room for a 2 start); empty input; single interval; unsorted input; intervals with start == end.

**Follow-ups:** return which room each meeting gets (heap of `(end, room_id)`); sweep-line alternative for rooms II (sort starts and ends separately); non-overlapping intervals (greedy by end time, count removals); streaming intervals (balanced tree / `sortedcontainers`).

### Two-sum / three-sum variants.

```python
def two_sum(nums, target):                 # indices, unsorted input
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen:
            return seen[target - x], i
        seen[x] = i
    return None

def two_sum_sorted(nums, target):          # two pointers, O(1) space
    i, j = 0, len(nums) - 1
    while i < j:
        s = nums[i] + nums[j]
        if s == target: return i, j
        if s < target: i += 1
        else: j -= 1
    return None

def three_sum(nums):                       # unique triplets summing to 0
    nums.sort(); res = []
    for i in range(len(nums) - 2):
        if i and nums[i] == nums[i - 1]:
            continue
        if nums[i] > 0:
            break
        lo, hi = i + 1, len(nums) - 1
        while lo < hi:
            s = nums[i] + nums[lo] + nums[hi]
            if s < 0: lo += 1
            elif s > 0: hi -= 1
            else:
                res.append([nums[i], nums[lo], nums[hi]])
                lo += 1; hi -= 1
                while lo < hi and nums[lo] == nums[lo - 1]:
                    lo += 1
    return res
```

**Complexity:** two-sum O(n) time/space with a hash map; sorted two-sum O(n) time, O(1) space; three-sum O(n^2) time, O(1) extra (ignoring sort/output).

**Edge cases:** duplicates (skip them for unique triplets); same element used twice (the hash-map check before inserting prevents it); negative numbers; no solution; overflow is not an issue in Python.

**Follow-ups:** 3Sum closest (track best diff); 4Sum / kSum (recursion down to two-sum, O(n^(k-1))); count pairs instead of finding one (Counter); two-sum over a stream (`add`/`find` design: hash map of counts); data too big (sort externally or hash-partition by value).

### Group anagrams.

**Approach:** anagrams share a canonical key. Use the sorted string, or a 26-length count tuple for O(L) keys on lowercase ASCII.

```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        groups["".join(sorted(w))].append(w)     # or: tuple of letter counts
    return list(groups.values())
```

**Complexity:** O(N · L log L) with sorting (O(N · L) with count keys); O(N · L) space.

**Edge cases:** empty strings (group together); case and spaces ("Dormitory" / "dirty room": normalise first); unicode (sorted key works for any characters; the 26-count key does not).

**Follow-ups:** stream of words (same dict, incremental); find all anagrams of p in s (sliding window with counts, O(n)); distributed (map: key -> word; reduce by key).

### Top-K frequent elements / words.

```python
import heapq
from collections import Counter

def top_k_frequent(nums, k):                    # bucket sort, O(n)
    count = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for x, c in count.items():
        buckets[c].append(x)
    out = []
    for c in range(len(buckets) - 1, 0, -1):
        for x in buckets[c]:
            out.append(x)
            if len(out) == k:
                return out
    return out

def top_k_words(words, k):                      # ties broken alphabetically
    count = Counter(words)
    return heapq.nsmallest(k, count, key=lambda w: (-count[w], w))
```

**Complexity:** Counter O(n); bucket sort O(n); heap O(U log k) for U unique items. Space O(U).

**Edge cases:** k > number of unique items; ties (define rule); empty input; case normalisation for words.

**Follow-ups:** streaming top-K (Count-Min Sketch + min-heap of size k, or Space-Saving algorithm); sliding time window; distributed (local top-K is not enough for exact global top-K: send full counts per partition, or use partition by key then merge).

### Number of islands.

**Approach:** scan cells; for each unvisited land cell, increment count and flood-fill (BFS/DFS) to mark the island. Iterative BFS avoids recursion limits on large grids.

```python
from collections import deque

def num_islands(grid):
    if not grid:
        return 0
    R, C = len(grid), len(grid[0])
    seen, count = set(), 0
    for r in range(R):
        for c in range(C):
            if grid[r][c] == "1" and (r, c) not in seen:
                count += 1
                q = deque([(r, c)]); seen.add((r, c))
                while q:
                    x, y = q.popleft()
                    for nx, ny in ((x+1, y), (x-1, y), (x, y+1), (x, y-1)):
                        if 0 <= nx < R and 0 <= ny < C and grid[nx][ny] == "1" and (nx, ny) not in seen:
                            seen.add((nx, ny)); q.append((nx, ny))
    return count
```

**Complexity:** O(R·C) time and space.

**Edge cases:** empty grid; all water; all land; `"1"` vs `1` types; recursion depth with DFS on a 1000x1000 grid.

**Follow-ups:** max island area; number of distinct island shapes (normalise path signature); islands added dynamically (Number of Islands II: union-find, near O(1) per add); grid too large for memory (process row strips with union-find across boundaries).

### Word search / word ladder.

**Word search (word in a grid):** DFS backtracking from each cell, marking visited in place.

```python
def exist(board, word):
    R, C = len(board), len(board[0])
    def dfs(r, c, i):
        if i == len(word):
            return True
        if not (0 <= r < R and 0 <= c < C) or board[r][c] != word[i]:
            return False
        tmp, board[r][c] = board[r][c], "#"
        found = any(dfs(r + dr, c + dc, i + 1) for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)))
        board[r][c] = tmp
        return found
    return any(dfs(r, c, 0) for r in range(R) for c in range(C))
```

Complexity O(R·C·4^L) worst case, O(L) recursion. For many words (Word Search II), build a trie of words and DFS once, pruning trie nodes as words are found.

**Word ladder (shortest transformation length):** BFS over words; neighbours via wildcard patterns (`h*t`) to avoid O(N^2) comparisons.

```python
from collections import defaultdict, deque

def ladder_length(begin, end, word_list):
    words = set(word_list)
    if end not in words:
        return 0
    patterns = defaultdict(list)
    for w in words | {begin}:
        for i in range(len(w)):
            patterns[w[:i] + "*" + w[i+1:]].append(w)
    q, seen = deque([(begin, 1)]), {begin}
    while q:
        w, d = q.popleft()
        if w == end:
            return d
        for i in range(len(w)):
            p = w[:i] + "*" + w[i+1:]
            for nxt in patterns[p]:
                if nxt not in seen:
                    seen.add(nxt); q.append((nxt, d + 1))
            patterns[p] = []                 # each pattern expanded once
    return 0
```

Complexity O(N · L^2) time and space (N words, length L). Bidirectional BFS cuts the explored frontier significantly. Follow-up: return all shortest paths (Word Ladder II: BFS to build parent map, then DFS to reconstruct).

**Edge cases:** end word missing from the dictionary; begin == end; single-letter words; case sensitivity.

### Course schedule (cycle detection).

**Approach:** can all courses be finished = is the prerequisite graph acyclic. Kahn's algorithm (count processed nodes) or DFS with three colours (white/grey/black; a grey neighbour means a back edge = cycle). Course Schedule II returns the order.

```python
from collections import deque

def find_order(n, prereqs):
    """prereqs: [course, prereq] pairs. Returns an order, or [] if a cycle exists."""
    adj = [[] for _ in range(n)]
    indeg = [0] * n
    for course, pre in prereqs:
        adj[pre].append(course); indeg[course] += 1
    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []
    while q:
        u = q.popleft(); order.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)
    return order if len(order) == n else []

def can_finish(n, prereqs):
    return len(find_order(n, prereqs)) == n
```

**Complexity:** O(V + E) time and space.

**Edge cases:** no prerequisites; self-loop `[a, a]`; duplicate edges (indegree counts them twice, and they are decremented twice, so still correct); disconnected graph.

**Follow-ups:** report the actual cycle (DFS with a parent stack); minimum number of semesters (BFS levels); with a max courses per semester (harder; bitmask DP for small n); lexicographically smallest order (heap instead of deque).

### Implement a tokenizer / simple expression evaluator (basic calculator I/II).

**Approach:** tokenise into numbers, operators and parentheses; then either use a stack (Calculator II: `+ - * /` without parens) or a recursive-descent parser for full precedence plus parentheses and unary minus. Recursive descent is the most extensible answer.

Grammar: `expr := term (('+'|'-') term)*`, `term := factor (('*'|'/') factor)*`, `factor := NUMBER | '(' expr ')' | ('+'|'-') factor`.

```python
import re

TOKEN = re.compile(r"\s*(?:(\d+\.?\d*)|(.))")

def tokenize(s):
    out = []
    for num, op in TOKEN.findall(s):
        if num:
            out.append(float(num) if "." in num else int(num))
        elif op.strip():
            if op not in "+-*/()":
                raise SyntaxError(f"bad char {op!r}")
            out.append(op)
    return out

def evaluate(s: str):
    toks, pos = tokenize(s), 0
    def peek():
        return toks[pos] if pos < len(toks) else None
    def take():
        nonlocal pos
        pos += 1
        return toks[pos - 1]
    def expr():
        v = term()
        while peek() in ("+", "-"):
            v = v + term() if take() == "+" else v - term()
        return v
    def term():
        v = factor()
        while peek() in ("*", "/"):
            if take() == "*":
                v *= factor()
            else:
                d = factor()
                if d == 0:
                    raise ZeroDivisionError("division by zero")
                v = int(v / d) if isinstance(v, int) and isinstance(d, int) else v / d
        return v
    def factor():
        t = take() if peek() is not None else None
        if isinstance(t, (int, float)):
            return t
        if t == "(":
            v = expr()
            if peek() != ")":
                raise SyntaxError("expected )")
            take()
            return v
        if t in ("+", "-"):
            v = factor()
            return v if t == "+" else -v
        raise SyntaxError(f"unexpected {t!r}")
    v = expr()
    if pos != len(toks):
        raise SyntaxError("trailing tokens")
    return v

assert evaluate("3+2*2") == 7 and evaluate(" -(1+(4+5+2)-3)+(6+8) ") == 5 and evaluate("14/3*2") == 8
```

Integer division truncates toward zero to match the LeetCode spec (`int(v / d)`); be careful that `//` floors for negatives.

**Complexity:** O(n) time; O(n) space for tokens and recursion depth (nested parens).

**Edge cases:** whitespace, unary minus (`-3`, `2*-3`), division by zero, empty input, mismatched parens, very deep nesting (recursion limit; switch to shunting-yard with explicit stacks).

**Follow-ups:** variables (`x = 3`, environment dict); functions (`max(a,b)`); exponent with right associativity; build an AST and evaluate separately (enables caching/printing); never use `eval()` on user input.

### Flatten nested JSON / nested list iterator.

**Flatten JSON:** recursive (or explicit-stack) walk producing dotted keys. Decide how to represent list indices (`a.0.b` or `a[0].b`) and empty containers.

```python
def flatten(obj, prefix="", sep="."):
    out = {}
    stack = [(prefix, obj)]
    while stack:
        key, val = stack.pop()
        if isinstance(val, dict) and val:
            stack.extend((f"{key}{sep}{k}" if key else str(k), v) for k, v in val.items())
        elif isinstance(val, list) and val:
            stack.extend((f"{key}{sep}{i}" if key else str(i), v) for i, v in enumerate(val))
        else:
            out[key] = val            # scalars and empty {} / []
    return out

# flatten({"a": {"b": 1, "c": [1, {"d": 2}]}}) -> {"a.b": 1, "a.c.0": 1, "a.c.1.d": 2}
```

**Nested list iterator:** lazy, using a stack of iterators so memory is O(depth), not O(n).

```python
class NestedIterator:
    def __init__(self, nested):
        self.stack = [iter(nested)]
        self._next = None
    def _advance(self):
        while self.stack and self._next is None:
            try:
                x = next(self.stack[-1])
            except StopIteration:
                self.stack.pop(); continue
            if isinstance(x, list):
                self.stack.append(iter(x))
            else:
                self._next = x
    def has_next(self):
        self._advance()
        return self._next is not None
    def next(self):
        if not self.has_next():
            raise StopIteration
        x, self._next = self._next, None
        return x
```

(If `None` can be a legitimate value, use a sentinel object instead of `None`.)

**Complexity:** flatten O(total nodes) time and space; iterator O(1) amortised per element, O(depth) space.

**Edge cases:** keys that contain the separator (escape or choose another separator); empty dicts/lists; very deep nesting (explicit stack avoids recursion limits); non-string keys.

**Follow-ups:** unflatten (split keys, rebuild; decide whether numeric segments mean lists); flatten into CSV columns (union of keys across records); generator version `yield from`.

### Diff two JSON objects / two lists of records.

**JSON diff:** recursive compare returning a list of `(path, kind, old, new)` where kind is added / removed / changed.

```python
def json_diff(a, b, path=""):
    if isinstance(a, dict) and isinstance(b, dict):
        out = []
        for k in a.keys() | b.keys():
            p = f"{path}.{k}" if path else str(k)
            if k not in b:
                out.append((p, "removed", a[k], None))
            elif k not in a:
                out.append((p, "added", None, b[k]))
            else:
                out += json_diff(a[k], b[k], p)
        return sorted(out, key=lambda d: d[0])
    if isinstance(a, list) and isinstance(b, list):
        out = []
        for i in range(max(len(a), len(b))):
            p = f"{path}[{i}]"
            if i >= len(b):
                out.append((p, "removed", a[i], None))
            elif i >= len(a):
                out.append((p, "added", None, b[i]))
            else:
                out += json_diff(a[i], b[i], p)
        return out
    return [] if a == b else [(path, "changed", a, b)]
```

**Lists of records:** key both lists by a primary key, then set operations.

```python
def diff_records(old, new, key="id"):
    o = {r[key]: r for r in old}
    n = {r[key]: r for r in new}
    added = [n[k] for k in n.keys() - o.keys()]
    removed = [o[k] for k in o.keys() - n.keys()]
    changed = {k: {f: (o[k].get(f), n[k].get(f))
                   for f in o[k].keys() | n[k].keys() if o[k].get(f) != n[k].get(f)}
               for k in o.keys() & n.keys() if o[k] != n[k]}
    return added, removed, changed
```

**Complexity:** JSON diff O(size of both documents); record diff O(n + m) time and space.

**Edge cases:** lists where order should not matter (compare as multisets, or match by id); type changes (`1` vs `"1"`, `1` vs `1.0`, `True == 1` in Python); duplicate primary keys; missing key field; floats (tolerance).

**Follow-ups:** files too big for memory (sort both by key externally and do a merge-join, or hash-partition by key); emit RFC 6902 JSON Patch; list diff with minimal edits (LCS / Myers diff).

### Implement `retry with exponential backoff and jitter`.

**Approach:** retry only retryable errors (timeouts, connection errors, 429, 5xx), with delay `min(cap, base * 2^attempt)` and "full jitter" (`random.uniform(0, delay)`) to avoid thundering herds. Respect `Retry-After` if present. Cap attempts and total time. Make the clock and sleep injectable for tests.

```python
import random, time, functools

class RetryableError(Exception):
    def __init__(self, msg="", retry_after: float | None = None):
        super().__init__(msg); self.retry_after = retry_after

def retry(max_attempts=5, base=0.2, cap=10.0, retry_on=(RetryableError, TimeoutError, ConnectionError),
          sleep=time.sleep, rng=random.random):
    def deco(fn):
        @functools.wraps(fn)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return fn(*args, **kwargs)
                except retry_on as e:
                    if attempt == max_attempts - 1:
                        raise
                    delay = rng() * min(cap, base * 2 ** attempt)          # full jitter
                    ra = getattr(e, "retry_after", None)
                    sleep(max(delay, ra) if ra else delay)
        return wrapper
    return deco

@retry(max_attempts=4)
def call_api():
    ...
```

**Complexity:** O(max_attempts) calls; worst-case wait is bounded by roughly `sum(min(cap, base*2^i))`.

**Edge cases:** non-idempotent operations (only retry POSTs with an idempotency key); 4xx errors other than 408/429 must not be retried; exceptions inside `sleep`; the last error must propagate with its original traceback.

**Follow-ups:** async version (`await asyncio.sleep`); retry budget / circuit breaker so retries do not amplify an outage; decorrelated jitter (`delay = min(cap, uniform(base, prev*3))`); logging each attempt with a correlation id; libraries: `tenacity`, `backoff`.

### Implement a simple pub/sub or event bus.

**Approach:** map topic -> list of handlers. `subscribe` returns an unsubscribe function. `publish` calls handlers, isolating failures so one bad subscriber does not break others. Add wildcard topics and async delivery as extensions.

```python
import threading, logging, fnmatch
from collections import defaultdict
from concurrent.futures import ThreadPoolExecutor

log = logging.getLogger(__name__)

class EventBus:
    def __init__(self, async_workers: int = 0):
        self._subs = defaultdict(list)            # pattern -> [handler]
        self._lock = threading.RLock()
        self._pool = ThreadPoolExecutor(async_workers) if async_workers else None

    def subscribe(self, pattern: str, handler):
        with self._lock:
            self._subs[pattern].append(handler)
        def unsubscribe():
            with self._lock:
                if handler in self._subs[pattern]:
                    self._subs[pattern].remove(handler)
        return unsubscribe

    def publish(self, topic: str, event) -> int:
        with self._lock:                          # snapshot so handlers may (un)subscribe
            handlers = [h for p, hs in self._subs.items() if fnmatch.fnmatch(topic, p) for h in hs]
        for h in handlers:
            if self._pool:
                self._pool.submit(self._safe_call, h, topic, event)
            else:
                self._safe_call(h, topic, event)
        return len(handlers)

    @staticmethod
    def _safe_call(h, topic, event):
        try:
            h(event)
        except Exception:
            log.exception("handler failed for topic %s", topic)

bus = EventBus()
unsub = bus.subscribe("order.*", lambda e: print("got", e))
bus.publish("order.created", {"id": 1})
```

**Complexity:** subscribe O(1); publish O(P + H) for P patterns and H matching handlers (exact-match only would be O(H)).

**Edge cases:** handler throws; handler unsubscribes during publish (snapshot handles it); publishing to a topic with no subscribers; ordering guarantees (synchronous = in order; async = not guaranteed).

**Follow-ups:** durable delivery and replay (persist events; consumers track offsets, like Kafka); at-least-once vs exactly-once (idempotent handlers); back-pressure with bounded queues; dead-letter queue for failing handlers; cross-process: Redis pub/sub, SNS/SQS, Kafka.

### Paginate through an API and aggregate results.

**Approach:** a generator that follows the API's pagination scheme (cursor/`next` token, page number, or `Link` header), with retries on 429/5xx, a timeout, and a page cap. Aggregation consumes the generator, so memory stays bounded.

```python
import time, requests

def iter_items(session: requests.Session, url: str, params=None, max_pages=10_000, timeout=10):
    params = dict(params or {})
    for _ in range(max_pages):
        for attempt in range(5):
            resp = session.get(url, params=params, timeout=timeout)
            if resp.status_code == 429 or resp.status_code >= 500:
                time.sleep(float(resp.headers.get("Retry-After", 2 ** attempt)))
                continue
            resp.raise_for_status()
            break
        else:
            raise RuntimeError(f"giving up on {url} {params}")
        body = resp.json()
        yield from body.get("data", [])
        cursor = body.get("next_cursor")
        if not cursor:
            return
        params["cursor"] = cursor
    raise RuntimeError("max_pages exceeded; possible pagination loop")

def total_by_status(session, url):
    totals = {}
    for item in iter_items(session, url, {"limit": 100}):
        totals[item["status"]] = totals.get(item["status"], 0) + item.get("amount", 0)
    return totals
```

**Complexity:** O(N) items, O(N / page_size) requests; memory O(page_size + aggregate size).

**Edge cases:** empty first page; the same cursor returned twice (loop guard); offset pagination with inserts during iteration (duplicates/skips: prefer cursor, dedupe by id); auth token expiring mid-run (refresh); missing fields.

**Follow-ups:** parallelise when pages are addressable by number (thread pool with a concurrency cap); resumable sync (persist the last cursor / `updated_since` watermark); respecting rate limits proactively; async with `httpx`.

### Batch requests: given a stream of items, send in batches of N or every T ms.

**Approach:** buffer items; flush when the buffer reaches N, or when T ms have passed since the first item in the buffer. A background thread waits on a queue with a timeout so time-based flushes happen even if no new items arrive. Flush the remainder on close.

```python
import queue, threading, time

class Batcher:
    def __init__(self, send, max_size=100, max_wait_ms=200):
        self.send, self.max_size, self.max_wait = send, max_size, max_wait_ms / 1000
        self.q: queue.Queue = queue.Queue(maxsize=10 * max_size)   # back-pressure
        self._stop = object()
        self.t = threading.Thread(target=self._run, daemon=True); self.t.start()

    def add(self, item):
        self.q.put(item)

    def close(self):
        self.q.put(self._stop); self.t.join()

    def _run(self):
        buf, deadline = [], None
        while True:
            timeout = None if deadline is None else max(0, deadline - time.monotonic())
            try:
                item = self.q.get(timeout=timeout)
            except queue.Empty:
                item = None                                 # deadline hit
            if item is self._stop:
                if buf: self._flush(buf)
                return
            if item is not None:
                if not buf:
                    deadline = time.monotonic() + self.max_wait
                buf.append(item)
            if buf and (len(buf) >= self.max_size or time.monotonic() >= deadline):
                self._flush(buf); buf, deadline = [], None

    def _flush(self, buf):
        try:
            self.send(list(buf))
        except Exception as e:
            print("batch failed", e)            # in production: retry, then dead-letter
```

(If `None` can be a real item, use a separate sentinel for "timeout".)

**Complexity:** O(1) amortised per item; memory O(max queue + N).

**Edge cases:** shutdown with a partial batch (flush in `close`); send failing (retry/DLQ, do not lose items); slow sender (bounded queue blocks producers = back-pressure); batch size limits in bytes as well as count.

**Follow-ups:** asyncio version (`asyncio.wait_for(queue.get(), timeout)`); per-key batching; ordering guarantees; this is how Kafka producers (`linger.ms`, `batch.size`) and embedding batch APIs work.

### Deduplicate records with fuzzy matching (entity resolution).

**Approach:** comparing every pair is O(n^2), so:
1. **Normalise:** lowercase, strip punctuation, expand abbreviations (`inc`, `st.`), standardise phones/emails/addresses.
2. **Blocking:** only compare records sharing a cheap key (e.g. first 3 letters of normalised name + zip, or phonetic code). Union multiple blocking keys for recall.
3. **Score pairs:** weighted similarity per field (Jaro-Winkler / token-set ratio for names, exact for email).
4. **Cluster:** union-find over pairs above a threshold (transitive closure).
5. **Survivorship:** choose the golden record per cluster (most complete / most recent).

```python
import re
from difflib import SequenceMatcher
from collections import defaultdict
from itertools import combinations

def norm(s):
    return re.sub(r"[^a-z0-9 ]", "", (s or "").lower()).strip()

def sim(a, b):
    return SequenceMatcher(None, a, b).ratio()

def dedupe(records, threshold=0.85):
    parent = list(range(len(records)))
    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]; x = parent[x]
        return x
    blocks = defaultdict(list)
    for i, r in enumerate(records):
        blocks[(norm(r["name"])[:3], r.get("zip"))].append(i)
        if r.get("email"):
            blocks[("email", r["email"].lower())].append(i)
    for idx in blocks.values():
        for i, j in combinations(idx, 2):
            a, b = records[i], records[j]
            if a.get("email") and a.get("email", "").lower() == b.get("email", "").lower():
                score = 1.0
            else:
                score = 0.7 * sim(norm(a["name"]), norm(b["name"])) + \
                        0.3 * sim(norm(a.get("address")), norm(b.get("address")))
            if score >= threshold:
                parent[find(i)] = find(j)
    clusters = defaultdict(list)
    for i in range(len(records)):
        clusters[find(i)].append(records[i])
    return list(clusters.values())
```

**Complexity:** O(n) to block, O(sum of block_size^2 × field length) to compare, near-linear union-find. Big blocks dominate, so cap block size or sub-block.

**Edge cases:** transitive chains merging different entities (A~B, B~C but A not ~ C; consider cluster-level checks); missing fields; nicknames (Bob/Robert); company suffixes; non-Latin scripts.

**Follow-ups:** how to choose the threshold (label a sample, measure precision/recall); ML matcher (logistic regression on field similarities, or `dedupe`/`splink` libraries); embeddings + ANN for blocking; human review queue for borderline scores; incremental matching for new records.

### Implement a cosine-similarity search over embeddings (brute force, then top-k with heap).

**Approach:** cosine(a, b) = a·b / (|a||b|). Pre-normalise all vectors once so search is just a dot product. Brute force scores every vector; keep top-k with a size-k min-heap (O(n log k)) rather than sorting (O(n log n)). With NumPy, `argpartition` is the fast version.

```python
import heapq, math

def normalize(v):
    n = math.sqrt(sum(x * x for x in v))
    return [x / n for x in v] if n else v

def top_k_cosine(query, vectors, k=5):
    """vectors: list of (id, normalized_vector). Returns [(score, id)] best first."""
    q = normalize(query)
    heap = []                                       # min-heap of (score, id)
    for vid, v in vectors:
        score = sum(a * b for a, b in zip(q, v))
        if len(heap) < k:
            heapq.heappush(heap, (score, vid))
        elif score > heap[0][0]:
            heapq.heapreplace(heap, (score, vid))
    return sorted(heap, reverse=True)

# NumPy version: M is (n, d) with normalised rows
import numpy as np
def top_k_np(M, q, k=5):
    q = q / np.linalg.norm(q)
    scores = M @ q
    idx = np.argpartition(-scores, min(k, len(scores) - 1))[:k]
    idx = idx[np.argsort(-scores[idx])]
    return list(zip(scores[idx].tolist(), idx.tolist()))
```

**Complexity:** O(n·d) to score, O(n log k) heap; O(k) extra space. NumPy is the same big-O but vectorised (roughly 100x faster).

**Edge cases:** zero vectors (division by zero); dimension mismatch (validate); k > n; ties; ids that are not comparable when scores tie in the heap (add a counter as tiebreaker).

**Follow-ups:** scale to millions (ANN: HNSW, IVF-PQ via FAISS, or pgvector/Pinecone/Weaviate); metadata filtering (pre-filter vs post-filter trade-off); batch queries as a matrix multiply; float16/int8 quantisation to save memory.

### Chunk a document for RAG with overlap; respect sentence boundaries.

**Approach:** split into sentences, then greedily pack sentences into chunks up to `max_tokens`. For overlap, start the next chunk with the last few sentences of the previous one (up to `overlap_tokens`). Hard-split any sentence longer than the max. Use the real tokenizer for counting where possible; whitespace words are an approximation.

```python
import re

SENT = re.compile(r"(?<=[.!?])\s+(?=[A-Z0-9\"'(])")

def count_tokens(s):            # swap in tiktoken / the model's tokenizer
    return len(s.split())

def chunk(text, max_tokens=200, overlap_tokens=40):
    sents = []
    for para in re.split(r"\n\s*\n", text.strip()):
        for s in SENT.split(para.strip()):
            if not s:
                continue
            words = s.split()
            while len(words) > max_tokens:              # hard-split giant sentences
                sents.append(" ".join(words[:max_tokens])); words = words[max_tokens:]
            sents.append(" ".join(words))
    chunks, cur, cur_tok = [], [], 0
    for s in sents:
        t = count_tokens(s)
        if cur and cur_tok + t > max_tokens:
            chunks.append(" ".join(cur))
            keep, kt = [], 0                            # carry trailing sentences as overlap
            for prev in reversed(cur):
                pt = count_tokens(prev)
                if kt + pt > overlap_tokens or kt + pt + t > max_tokens:
                    break                                # overlap must leave room for s
                keep.insert(0, prev); kt += pt
            cur, cur_tok = keep, kt
        cur.append(s); cur_tok += t
    if cur:
        chunks.append(" ".join(cur))
    return chunks
```

**Complexity:** O(n) in text length (each sentence is carried into at most a few overlaps). Space O(n).

**Edge cases:** abbreviations ("Dr. Smith", "e.g.") falsely splitting sentences (use nltk/spaCy/pysbd in production); no punctuation at all; code blocks and tables (do not split inside); empty text; overlap >= max (validate).

**Follow-ups:** structure-aware chunking (split on Markdown headings, keep the heading path as metadata); attach metadata (doc id, page, offsets) for citations; semantic chunking (split where embedding similarity between neighbouring sentences drops); how to choose chunk size (evaluate retrieval recall on a labelled question set).

### Stream tokens from an LLM API and handle partial JSON.

**Approach:** consume the server-sent-events stream, append text deltas to a buffer, and display incrementally. For JSON output, you cannot `json.loads` until the end, so either (a) parse only on completion and validate, or (b) for progressive UI, "close" the partial JSON (track open strings/brackets and append the missing closers) and attempt a parse on each delta.

```python
import json

def close_partial_json(s: str) -> str:
    """Best-effort completion of a truncated JSON prefix so it can be parsed."""
    stack, in_str, esc = [], False, False
    for ch in s:
        if in_str:
            if esc: esc = False
            elif ch == "\\": esc = True
            elif ch == '"': in_str = False
        elif ch == '"': in_str = True
        elif ch in "{[": stack.append("}" if ch == "{" else "]")
        elif ch in "}]" and stack: stack.pop()
    out = s + ('"' if in_str else "")
    out = out.rstrip().rstrip(",")
    if out.endswith(":"):
        out += " null"
    return out + "".join(reversed(stack))

def stream_json(chunks):
    """chunks: iterator of text deltas. Yields progressively more complete objects."""
    buf, last = "", None
    for delta in chunks:
        buf += delta
        try:
            obj = json.loads(close_partial_json(buf))
        except json.JSONDecodeError:
            continue                                  # e.g. cut mid-number or mid-key
        if obj != last:
            last = obj
            yield obj
    final = json.loads(buf)                           # strict parse of the full output
    if final != last:
        yield final

# list(stream_json(['{"name": "Al', 'ice", "tags": ["a",', ' "b"]}']))
```

With the Anthropic SDK: `with client.messages.stream(...) as stream: for text in stream.text_stream: ...`; tool-use input arrives as `input_json_delta` events carrying `partial_json` fragments, which you concatenate and parse at `content_block_stop`.

**Complexity:** re-parsing on each delta is O(n) per delta, O(n^2) total; fine for typical outputs. For long outputs, throttle parsing (every k ms) or use an incremental parser.

**Edge cases:** deltas splitting a multi-byte UTF-8 character (decode the byte stream incrementally, which the SDKs do for you); deltas splitting an escape sequence; the model wrapping JSON in prose or ```` ```json ```` fences; stream disconnects (retry the whole request; you cannot resume mid-generation); `stop_reason == "max_tokens"` leaving truncated JSON.

**Follow-ups:** use structured outputs / tool use with a JSON schema to guarantee valid JSON; cancel the stream on client disconnect; measure time-to-first-token.

### Parse and validate tool-call JSON from an LLM; repair malformed JSON.

**Approach:** a layered pipeline: extract -> parse -> repair -> validate -> (re-prompt on failure).
1. **Extract:** strip code fences; take the substring from the first `{` to its matching `}`.
2. **Parse** with `json.loads`. If it fails, apply cheap repairs: remove trailing commas, convert Python literals (`True/None`), single quotes to double (careful), close truncated brackets.
3. **Validate** against the tool's JSON Schema (`jsonschema` or Pydantic): required fields, types, enums. Check the tool name exists.
4. **On failure:** send the validation error back to the model as a tool result with `is_error`, and let it retry (bounded retries). Never `eval` model output.

```python
import json, re
from jsonschema import Draft202012Validator     # pip install jsonschema

def extract_json(text: str) -> str:
    text = re.sub(r"^`{3}(?:json)?\s*|\s*`{3}$", "", text.strip())   # strip code fences
    start = text.find("{")
    if start < 0:
        raise ValueError("no JSON object found")
    depth, in_str, esc = 0, False, False
    for i in range(start, len(text)):
        ch = text[i]
        if in_str:
            if esc: esc = False
            elif ch == "\\": esc = True
            elif ch == '"': in_str = False
        elif ch == '"': in_str = True
        elif ch == "{": depth += 1
        elif ch == "}":
            depth -= 1
            if depth == 0:
                return text[start:i + 1]
    return text[start:]                              # truncated; repair will try to close

def repair(s: str) -> str:
    s = re.sub(r",\s*([}\]])", r"\1", s)            # trailing commas
    s = re.sub(r"\bTrue\b", "true", s)
    s = re.sub(r"\bFalse\b", "false", s)
    s = re.sub(r"\bNone\b", "null", s)
    if '"' not in s:
        s = s.replace("'", '"')                      # only when clearly single-quoted
    return close_partial_json(s)                     # from the streaming answer above

def parse_tool_call(text: str, tools: dict):
    """tools: name -> JSON schema for input. Returns (name, args) or raises ValueError with a message for the model."""
    raw = extract_json(text)
    try:
        obj = json.loads(raw)
    except json.JSONDecodeError:
        obj = json.loads(repair(raw))                # raises if still broken
    name, args = obj.get("name"), obj.get("arguments", obj.get("input", {}))
    if name not in tools:
        raise ValueError(f"unknown tool {name!r}; valid: {sorted(tools)}")
    errors = sorted(Draft202012Validator(tools[name]).iter_errors(args), key=lambda e: e.path)
    if errors:
        raise ValueError("; ".join(f"{'/'.join(map(str, e.path)) or '<root>'}: {e.message}" for e in errors))
    return name, args
```

**Complexity:** O(n) in the length of the output for each stage.

**Edge cases:** multiple JSON objects in one reply; braces inside strings (handled by the string-aware scanner); arguments double-encoded as a JSON string (`"arguments": "{\"x\":1}"`: detect and `json.loads` again); extra fields (reject or strip, depending on `additionalProperties`); numbers as strings (coerce with Pydantic if safe).

**Follow-ups:** use native tool use / structured outputs (constrained decoding) so repair is rarely needed; log every repair to measure how often the model fails; semantic validation beyond schema (does this customer id exist? is this a destructive action needing confirmation?).

### Count tokens / truncate conversation history to fit a context window.

**Approach:** budget = context window − max output tokens − safety margin. Always keep the system prompt and the latest user message. Then add messages from newest to oldest until the budget is exhausted. Keep tool-use / tool-result pairs together (dropping one half breaks the API). Optionally replace the dropped prefix with a summary.

```python
def message_tokens(msg) -> int:
    # Approximation (~4 chars/token in English). In production, use the provider's
    # token counting endpoint (e.g. Anthropic messages.count_tokens) or its tokenizer.
    return len(msg.get("content") or "") // 4 + 4     # + per-message overhead

def truncate_history(system, messages, context_window, max_output, margin=500):
    budget = context_window - max_output - margin - message_tokens({"content": system})
    if not messages:
        return []
    # group messages so tool calls and their results are kept or dropped together
    groups, i = [], 0
    while i < len(messages):
        g = [messages[i]]
        if messages[i].get("tool_calls"):                # assistant message that made tool calls
            while i + 1 < len(messages) and messages[i + 1].get("role") == "tool":
                i += 1; g.append(messages[i])
        groups.append(g); i += 1
    kept, used = [], 0
    for g in reversed(groups):
        cost = sum(message_tokens(m) for m in g)
        if used + cost > budget:
            if not kept:                                 # latest turn alone is too big
                raise ValueError("latest message exceeds the context budget")
            break
        kept[:0] = g; used += cost
    while kept and kept[0]["role"] != "user":            # API requires starting with a user turn
        kept.pop(0)
    return kept
```

**Complexity:** O(n) messages (each counted once); O(n) space.

**Edge cases:** single huge message (truncate its content, e.g. keep head and tail of a pasted log, or chunk and summarise); images/attachments have their own token cost; conversation must start with a user turn and alternate roles; budget must account for tool definitions too (they consume tokens).

**Follow-ups:** summarise dropped turns ("rolling summary") instead of discarding; retrieve relevant old turns with embeddings; prompt caching (keep the prefix stable so cache hits stay valid, which argues for dropping from the middle in big blocks rather than one message at a time); exact counts via the API rather than heuristics.

### Simple in-memory vector store with add/search/delete.

**Approach:** store vectors in a NumPy matrix with an `id -> row` map. Normalise on insert so search is a matrix-vector product. Delete by swapping the last row into the deleted slot (O(d)), keeping the matrix dense. Support metadata filters.

```python
import numpy as np

class VectorStore:
    def __init__(self, dim: int):
        self.dim = dim
        self.M = np.empty((0, dim), dtype=np.float32)
        self.ids: list[str] = []
        self.row: dict[str, int] = {}
        self.meta: dict[str, dict] = {}

    def add(self, vid: str, vec, metadata=None):
        v = np.asarray(vec, dtype=np.float32)
        if v.shape != (self.dim,):
            raise ValueError("dimension mismatch")
        n = np.linalg.norm(v)
        if n == 0:
            raise ValueError("zero vector")
        v = v / n
        if vid in self.row:                                  # upsert
            self.M[self.row[vid]] = v
        else:
            self.row[vid] = len(self.ids); self.ids.append(vid)
            self.M = np.vstack([self.M, v])                  # amortise with capacity doubling in prod
        self.meta[vid] = metadata or {}

    def delete(self, vid: str) -> bool:
        i = self.row.pop(vid, None)
        if i is None:
            return False
        last = len(self.ids) - 1
        if i != last:                                        # move last row into the hole
            self.M[i] = self.M[last]
            self.ids[i] = self.ids[last]; self.row[self.ids[i]] = i
        self.M = self.M[:last]; self.ids.pop(); self.meta.pop(vid, None)
        return True

    def search(self, query, k=5, where: dict | None = None):
        if not self.ids:
            return []
        q = np.asarray(query, dtype=np.float32); q = q / np.linalg.norm(q)
        scores = self.M @ q
        if where:
            mask = np.array([all(self.meta[i].get(f) == v for f, v in where.items()) for i in self.ids])
            scores = np.where(mask, scores, -np.inf)
        k = min(k, len(self.ids))
        top = np.argpartition(-scores, k - 1)[:k]
        top = top[np.argsort(-scores[top])]
        return [(self.ids[i], float(scores[i])) for i in top if scores[i] != -np.inf]
```

**Complexity:** add O(n·d) as written due to `vstack` (O(d) amortised with a pre-allocated, doubling buffer); delete O(d); search O(n·d + n log k) plus O(n·f) for filters.

**Edge cases:** duplicate id (upsert); search on empty store; k > n; zero vectors; filter that matches nothing.

**Follow-ups:** persistence (save `M` with `np.save` plus a JSON sidecar; or SQLite + pgvector); ANN index (HNSW via `hnswlib`/FAISS) once n is in the millions; thread safety (readers-writer lock; copy-on-write matrix); hybrid search (BM25 + vectors, combined with reciprocal rank fusion).

### Concurrent requests with a max concurrency limit (semaphore / async).

**Approach:** asyncio + `asyncio.Semaphore(limit)`; each task acquires before making the call. Use `gather(return_exceptions=True)` so one failure does not cancel the rest, and keep results in input order. For huge inputs, use a fixed pool of N workers pulling from a queue so you do not create millions of coroutines at once.

```python
import asyncio
import httpx

async def fetch_all(urls, limit=10, timeout=10.0):
    sem = asyncio.Semaphore(limit)
    async with httpx.AsyncClient(timeout=timeout) as client:
        async def one(url):
            async with sem:
                r = await client.get(url)
                r.raise_for_status()
                return r.json()
        return await asyncio.gather(*(one(u) for u in urls), return_exceptions=True)

# Worker-pool variant: bounded memory for very large inputs
async def run_bounded(items, worker_fn, limit=10):
    q: asyncio.Queue = asyncio.Queue(maxsize=limit * 2)
    results = {}
    async def worker():
        while True:
            i, item = await q.get()
            try:
                results[i] = await worker_fn(item)
            except Exception as e:
                results[i] = e
            finally:
                q.task_done()
    workers = [asyncio.create_task(worker()) for _ in range(limit)]
    for i, item in enumerate(items):
        await q.put((i, item))
    await q.join()
    for w in workers:
        w.cancel()
    return [results[i] for i in range(len(results))]

# Threaded equivalent: ThreadPoolExecutor(max_workers=limit).map(fn, items)
```

**Complexity:** O(n) work; at most `limit` in flight; total time roughly ceil(n / limit) × latency.

**Edge cases:** exceptions (return them per item, or fail fast with `asyncio.TaskGroup`); timeouts per request; cancellation; rate limit (429) is different from a concurrency limit, so add a token bucket if the API limits requests per second.

**Follow-ups:** retries with backoff inside `one`; adaptive concurrency (AIMD: reduce limit on 429s); progress reporting; distributed limit across processes (Redis semaphore).

### Circuit breaker implementation.

**Approach:** three states. **Closed:** calls pass; count consecutive failures; on reaching a threshold, open. **Open:** fail fast without calling, until `reset_timeout` passes, then half-open. **Half-open:** let a limited number of trial calls through; success closes, failure re-opens.

```python
import threading, time

class CircuitOpenError(Exception):
    pass

class CircuitBreaker:
    def __init__(self, failure_threshold=5, reset_timeout=30.0, half_open_max=1,
                 expected=(Exception,), clock=time.monotonic):
        self.threshold, self.reset_timeout, self.half_open_max = failure_threshold, reset_timeout, half_open_max
        self.expected, self.clock = expected, clock
        self.state, self.failures, self.opened_at, self.trials = "closed", 0, 0.0, 0
        self.lock = threading.Lock()

    def _before(self):
        with self.lock:
            if self.state == "open":
                if self.clock() - self.opened_at < self.reset_timeout:
                    raise CircuitOpenError("circuit open")
                self.state, self.trials = "half_open", 0
            if self.state == "half_open":
                if self.trials >= self.half_open_max:
                    raise CircuitOpenError("half-open trial in progress")
                self.trials += 1

    def _on_success(self):
        with self.lock:
            self.state, self.failures = "closed", 0

    def _on_failure(self):
        with self.lock:
            self.failures += 1
            if self.state == "half_open" or self.failures >= self.threshold:
                self.state, self.opened_at = "open", self.clock()

    def call(self, fn, *args, **kwargs):
        self._before()
        try:
            result = fn(*args, **kwargs)
        except self.expected:
            self._on_failure()
            raise
        except BaseException:
            with self.lock:                          # not a dependency failure: free the trial slot
                if self.state == "half_open":
                    self.trials -= 1
            raise
        self._on_success()
        return result
```

**Complexity:** O(1) per call; O(1) space.

**Edge cases:** which errors count (timeouts and 5xx yes; 4xx caused by bad input no); concurrent calls in half-open (limit trials); clock injection for tests; failures counted consecutively vs a failure rate over a rolling window.

**Follow-ups:** rolling-window failure rate with a minimum request volume (like Hystrix/resilience4j); fallback responses (cached value, degraded mode); metrics on state transitions and alerting; per-endpoint breakers; combine with retries (retries inside the breaker, not around it, or retries will hammer an open circuit).

### Debounce / throttle function (JS).

- **Debounce:** run only after calls have stopped for `wait` ms (search-as-you-type, resize). Each call resets the timer.
- **Throttle:** run at most once per `wait` ms (scroll handlers). Leading call fires immediately; a trailing call captures the last arguments.

```javascript
function debounce(fn, wait, { leading = false } = {}) {
  let timer = null;
  function debounced(...args) {
    const callNow = leading && timer === null;
    clearTimeout(timer);
    timer = setTimeout(() => {
      timer = null;
      if (!leading) fn.apply(this, args);
    }, wait);
    if (callNow) fn.apply(this, args);
  }
  debounced.cancel = () => { clearTimeout(timer); timer = null; };
  return debounced;
}

function throttle(fn, wait) {
  let last = 0, timer = null, pendingArgs = null, pendingThis = null;
  function throttled(...args) {
    const now = Date.now();
    const remaining = wait - (now - last);
    if (remaining <= 0) {
      clearTimeout(timer); timer = null;
      last = now;
      fn.apply(this, args);
    } else {
      pendingArgs = args; pendingThis = this;            // remember latest call for trailing edge
      if (!timer) {
        timer = setTimeout(() => {
          last = Date.now(); timer = null;
          fn.apply(pendingThis, pendingArgs);
          pendingArgs = pendingThis = null;
        }, remaining);
      }
    }
  }
  throttled.cancel = () => { clearTimeout(timer); timer = null; pendingArgs = null; };
  return throttled;
}
```

**Complexity:** O(1) per call; one timer.

**Edge cases:** preserving `this` and arguments (use a `function`, not an arrow, for the wrapper); cancel on component unmount; leading + trailing both enabled; system clock changes (`performance.now()` is monotonic).

**Follow-ups:** `flush()` to run the pending call immediately; return a Promise of the result; React hook version (`useMemo(() => debounce(...), [])` plus cleanup in `useEffect`); Python equivalent with `threading.Timer` or asyncio.

### Unix `tail -f` / `grep` style utility.

**Approach:**
- **tail -n N:** read from the end in blocks backwards until N newlines are found (do not read the whole file), or use `deque(f, maxlen=N)` for simple cases.
- **-f (follow):** after printing, loop: read new lines; if none, sleep briefly. Detect rotation/truncation by checking the inode and file size.
- **grep:** compile the regex once and filter lines lazily, so it composes with follow.

```python
import os, re, sys, time
from collections import deque

def last_lines(path, n=10, block=4096):
    if n <= 0:
        return []
    with open(path, "rb") as f:
        f.seek(0, os.SEEK_END)
        pos, data = f.tell(), b""
        while pos > 0 and data.count(b"\n") <= n:
            step = min(block, pos); pos -= step
            f.seek(pos); data = f.read(step) + data
    return [l.decode("utf-8", "replace") for l in data.splitlines()[-n:]]

def follow(path, poll=0.5):
    f = open(path, "r", encoding="utf-8", errors="replace")
    f.seek(0, os.SEEK_END)
    inode = os.fstat(f.fileno()).st_ino
    buf = ""
    while True:
        chunk = f.readline()
        if chunk:
            buf += chunk
            if buf.endswith("\n"):                 # only emit complete lines
                yield buf.rstrip("\n"); buf = ""
            continue
        time.sleep(poll)
        try:
            st = os.stat(path)
        except FileNotFoundError:
            continue                               # mid-rotation
        if st.st_ino != inode or st.st_size < f.tell():   # rotated or truncated
            f.close()
            f = open(path, "r", encoding="utf-8", errors="replace")
            inode = os.fstat(f.fileno()).st_ino

def grep(lines, pattern, invert=False, ignore_case=False):
    rx = re.compile(pattern, re.I if ignore_case else 0)
    return (l for l in lines if bool(rx.search(l)) != invert)

if __name__ == "__main__":
    path, pat = sys.argv[1], sys.argv[2]
    for line in grep(last_lines(path, 10), pat):
        print(line)
    for line in grep(follow(path), pat):
        print(line, flush=True)
```

**Complexity:** `last_lines` O(size of the last N lines), not O(file); follow O(1) per new line; grep O(line length) per line; memory O(N).

**Edge cases:** last line without a trailing newline (partial line buffering); log rotation and truncation; binary/invalid UTF-8; very long lines; file not existing yet.

**Follow-ups:** use inotify/`watchdog` instead of polling; context lines (`-A/-B` with a deque); multiple files; counting matches per minute; this is the core of log shippers like Fluent Bit and Filebeat.

### CSV ingestion with bad rows: parse, validate, report errors.

**Approach:** stream with `csv.DictReader` (handles quoting and embedded commas/newlines). Validate the header first. Validate each row against a schema (type converters + rules). Good rows go to the sink in batches; bad rows go to an error report with the line number, field and reason, and the raw row. Never fail the whole file for one bad row, but do fail if the error rate exceeds a threshold.

```python
import csv
from dataclasses import dataclass
from datetime import date
from decimal import Decimal, InvalidOperation

def email(s: str) -> str:
    if "@" not in s or " " in s:
        raise ValueError("invalid email")
    return s.lower()

SCHEMA = {                       # field -> (converter, required)
    "id": (int, True),
    "email": (email, True),
    "amount": (Decimal, True),
    "signup_date": (date.fromisoformat, False),
}

@dataclass
class RowError:
    line: int
    field: str | None
    reason: str
    raw: dict

def ingest(path, sink, batch_size=1000, max_error_rate=0.05):
    good, errors, seen_ids, total = [], [], set(), 0
    with open(path, newline="", encoding="utf-8-sig") as f:          # -sig strips a BOM
        reader = csv.DictReader(f)
        missing = set(SCHEMA) - set(reader.fieldnames or [])
        if missing:
            raise ValueError(f"missing columns: {sorted(missing)}")
        for row in reader:
            total += 1
            line = reader.line_num
            if None in row:                                           # too many fields
                errors.append(RowError(line, None, "extra columns", row)); continue
            clean, err = {}, None
            for field, (conv, required) in SCHEMA.items():
                val = (row.get(field) or "").strip()
                if not val:
                    if required:
                        err = RowError(line, field, "required", row); break
                    clean[field] = None; continue
                try:
                    clean[field] = conv(val)
                except (ValueError, InvalidOperation) as e:
                    err = RowError(line, field, f"bad value {val!r}: {e}", row); break
            if err is None and clean["id"] in seen_ids:
                err = RowError(line, "id", "duplicate id", row)
            if err:
                errors.append(err); continue
            seen_ids.add(clean["id"]); good.append(clean)
            if len(good) >= batch_size:
                sink(good); good = []
    if good:
        sink(good)
    if total and len(errors) / total > max_error_rate:
        raise RuntimeError(f"{len(errors)}/{total} bad rows; aborting (data already sent should be in a staging table)")
    return {"total": total, "loaded": total - len(errors), "errors": errors}
```

**Complexity:** O(rows × fields) time; memory O(batch_size + errors + unique ids).

**Edge cases:** BOM, different delimiters (`csv.Sniffer`), quoted newlines, short rows (`DictReader` fills `None` values), extra columns, encodings (latin-1 exports from Excel), thousands separators and currency symbols, date formats, duplicate ids, empty file.

**Follow-ups:** load into a staging table and swap atomically so a failed file never half-loads; write errors to a CSV the customer can fix and re-upload; collect all errors per row instead of the first; Pydantic models for the schema; pandas/Polars for speed when rows are trusted; idempotent reloads (upsert by key).

---

## C. Object-oriented / low-level design (code it)

General approach for every LLD question (say this structure out loud):
1. **Clarify requirements and scope** (5 min): actors, core use cases, what is out of scope.
2. **Identify entities and relationships** (nouns -> classes, verbs -> methods).
3. **Define interfaces** for the parts likely to vary (pricing, strategy, notification channel). This is where extension points live.
4. **Code the core flow** end to end, then handle edge cases and concurrency.
5. **Discuss extensions** and which design patterns you used (Strategy, State, Observer, Factory) and why.

### Parking lot

**Clarify:** multiple floors; spot sizes (motorcycle, compact, large); vehicles fit in a spot of their size or larger; tickets on entry; fee on exit by duration.

**Classes:** `VehicleType`/`SpotSize` enums, `Vehicle`, `Spot`, `Floor`, `Ticket`, `PricingStrategy` (interface), `SpotAllocationStrategy`, `ParkingLot` (facade).

```python
import heapq, itertools, math, threading, time
from dataclasses import dataclass, field
from enum import IntEnum
from abc import ABC, abstractmethod

class Size(IntEnum):
    SMALL = 1; MEDIUM = 2; LARGE = 3

@dataclass
class Vehicle:
    plate: str
    size: Size

@dataclass
class Spot:
    id: str
    floor: int
    size: Size
    vehicle: Vehicle | None = None

@dataclass
class Ticket:
    id: int
    vehicle: Vehicle
    spot: Spot
    entry: float
    exit: float | None = None
    fee: float | None = None

class PricingStrategy(ABC):
    @abstractmethod
    def fee(self, ticket: Ticket, exit_time: float) -> float: ...

class HourlyPricing(PricingStrategy):
    RATES = {Size.SMALL: 1.0, Size.MEDIUM: 2.0, Size.LARGE: 4.0}
    def fee(self, ticket, exit_time):
        hours = max(1, math.ceil((exit_time - ticket.entry) / 3600))
        return hours * self.RATES[ticket.spot.size]

class ParkingLot:
    def __init__(self, spots: list[Spot], pricing: PricingStrategy, clock=time.time):
        self.pricing, self.clock = pricing, clock
        # free spots per size, ordered (floor, id) so we fill nearest first
        self.free = {s: [] for s in Size}
        for sp in spots:
            heapq.heappush(self.free[sp.size], (sp.floor, sp.id, sp))
        self.active: dict[str, Ticket] = {}          # plate -> ticket
        self.ids = itertools.count(1)
        self.lock = threading.Lock()

    def park(self, v: Vehicle) -> Ticket:
        with self.lock:
            if v.plate in self.active:
                raise ValueError("already parked")
            for size in Size:
                if size >= v.size and self.free[size]:
                    _, _, spot = heapq.heappop(self.free[size])
                    spot.vehicle = v
                    t = Ticket(next(self.ids), v, spot, self.clock())
                    self.active[v.plate] = t
                    return t
            raise RuntimeError("lot full")

    def leave(self, plate: str) -> Ticket:
        with self.lock:
            t = self.active.pop(plate)
            t.exit = self.clock()
            t.fee = self.pricing.fee(t, t.exit)
            t.spot.vehicle = None
            heapq.heappush(self.free[t.spot.size], (t.spot.floor, t.spot.id, t.spot))
            return t

    def available(self) -> dict:
        return {s.name: len(h) for s, h in self.free.items()}
```

**Complexity:** park/leave O(log S); availability O(1) per size.

**Extension points:** new `PricingStrategy` (weekend, flat daily max, EV charging surcharge); allocation strategy (nearest to entrance, by floor); new spot types (EV, disabled) as extra enum values plus fit rules; payment interface; multiple entry gates (lock already makes allocation safe; in a distributed system, allocate via DB row locks or `SELECT ... FOR UPDATE SKIP LOCKED`); display boards subscribe to availability events (Observer).

### Elevator system

**Clarify:** N elevators, M floors; hall calls (floor + direction) and car calls (destination inside the car); goal: minimise wait time; capacity optional.

**Classes:** `Direction` enum, `Elevator` (state: floor, direction, stop sets), `Request`, `DispatchStrategy` (interface), `ElevatorController` (assigns hall calls, ticks the simulation).

**Algorithm:** each elevator runs the **SCAN/LOOK** algorithm: keep moving in the current direction while there are stops ahead, then reverse. Dispatcher picks the elevator with the lowest estimated cost (idle or moving toward the floor in the same direction is cheap).

```python
from enum import Enum
from abc import ABC, abstractmethod

class Dir(Enum):
    UP = 1; DOWN = -1; IDLE = 0

class Elevator:
    def __init__(self, eid: int, floor: int = 0):
        self.id, self.floor, self.dir = eid, floor, Dir.IDLE
        self.up_stops: set[int] = set()
        self.down_stops: set[int] = set()

    def add_stop(self, floor: int):
        if floor == self.floor:                         # already here: doors open now
            return
        (self.up_stops if floor > self.floor else self.down_stops).add(floor)

    def has_work(self):
        return bool(self.up_stops or self.down_stops)

    def step(self) -> list[str]:
        """Advance one tick (one floor). Returns events."""
        events = []
        if self.dir == Dir.IDLE:
            if self.up_stops: self.dir = Dir.UP
            elif self.down_stops: self.dir = Dir.DOWN
            else: return events
        ahead = self.up_stops if self.dir == Dir.UP else self.down_stops
        if not ahead:                                   # LOOK: reverse when nothing ahead
            self.dir = Dir.DOWN if self.dir == Dir.UP else Dir.UP
            if not self.has_work():
                self.dir = Dir.IDLE
            return events
        self.floor += self.dir.value
        if self.floor in ahead:
            ahead.discard(self.floor)
            events.append(f"elevator {self.id} opens at {self.floor}")
        # stops behind us are re-bucketed so reversal picks them up
        for s in list(self.up_stops):
            if s < self.floor: self.up_stops.discard(s); self.down_stops.add(s)
        for s in list(self.down_stops):
            if s > self.floor: self.down_stops.discard(s); self.up_stops.add(s)
        if not self.has_work():
            self.dir = Dir.IDLE
        return events

class DispatchStrategy(ABC):
    @abstractmethod
    def choose(self, elevators: list[Elevator], floor: int, direction: Dir) -> Elevator: ...

class NearestSuitable(DispatchStrategy):
    def choose(self, elevators, floor, direction):
        def cost(e):
            d = abs(e.floor - floor)
            if e.dir == Dir.IDLE:
                return d
            moving_toward = (floor - e.floor) * e.dir.value >= 0
            return d if moving_toward and e.dir == direction else d + 1000     # heavy penalty
        return min(elevators, key=cost)

class ElevatorController:
    def __init__(self, n: int, strategy: DispatchStrategy = NearestSuitable()):
        self.elevators = [Elevator(i) for i in range(n)]
        self.strategy = strategy

    def hall_call(self, floor: int, direction: Dir) -> int:
        e = self.strategy.choose(self.elevators, floor, direction)
        e.add_stop(floor)
        return e.id

    def car_call(self, eid: int, floor: int):
        self.elevators[eid].add_stop(floor)

    def tick(self):
        return [ev for e in self.elevators for ev in e.step()]
```

**Complexity:** dispatch O(N) elevators; step O(stops) per elevator.

**Extension points:** dispatch strategies (zoning for peak hours, destination dispatch); capacity/weight limits; maintenance mode (State pattern per elevator); express elevators serving subsets of floors; door timing and events to a UI (Observer). Mention that the real system is event-driven with a hardware controller; the `tick` simulation is for testability.

### Library management system

**Clarify:** books (title, ISBN) vs physical copies; members borrow and return; due dates and fines; reservations (holds) when no copy is available; borrow limits.

**Classes:** `Book` (metadata), `BookCopy` (barcode, status), `Member`, `Loan`, `Library` (service), `FinePolicy` (interface), holds queue per ISBN.

```python
from collections import defaultdict, deque
from dataclasses import dataclass, field
from datetime import date, timedelta
from enum import Enum

class CopyStatus(Enum):
    AVAILABLE = "available"; LOANED = "loaned"; ON_HOLD = "on_hold"; LOST = "lost"

@dataclass
class Book:
    isbn: str; title: str; author: str

@dataclass
class BookCopy:
    barcode: str; isbn: str; status: CopyStatus = CopyStatus.AVAILABLE
    held_for: str | None = None

@dataclass
class Member:
    id: str; name: str; max_loans: int = 5
    loans: set = field(default_factory=set)       # barcodes

@dataclass
class Loan:
    barcode: str; member_id: str; due: date; returned: date | None = None

class FinePolicy:
    def fine(self, loan: Loan, on: date) -> float:
        return max(0, (on - loan.due).days) * 0.25

class Library:
    LOAN_DAYS = 14
    def __init__(self, fine_policy: FinePolicy = FinePolicy()):
        self.books: dict[str, Book] = {}
        self.copies: dict[str, BookCopy] = {}
        self.members: dict[str, Member] = {}
        self.loans: dict[str, Loan] = {}              # active loans by barcode
        self.holds: dict[str, deque] = defaultdict(deque)   # isbn -> member ids
        self.fines: dict[str, float] = defaultdict(float)
        self.fine_policy = fine_policy

    def add_copy(self, book: Book, barcode: str):
        self.books.setdefault(book.isbn, book)
        self.copies[barcode] = BookCopy(barcode, book.isbn)

    def search(self, text: str) -> list[Book]:
        t = text.lower()
        return [b for b in self.books.values() if t in b.title.lower() or t in b.author.lower()]

    def checkout(self, member_id: str, barcode: str, today: date) -> Loan:
        m, c = self.members[member_id], self.copies[barcode]
        if len(m.loans) >= m.max_loans:
            raise ValueError("loan limit reached")
        if self.fines[member_id] > 10:
            raise ValueError("outstanding fines")
        if c.status == CopyStatus.ON_HOLD and c.held_for != member_id:
            raise ValueError("copy is held for another member")
        if c.status not in (CopyStatus.AVAILABLE, CopyStatus.ON_HOLD):
            raise ValueError("copy not available")
        c.status, c.held_for = CopyStatus.LOANED, None
        loan = Loan(barcode, member_id, today + timedelta(days=self.LOAN_DAYS))
        self.loans[barcode] = loan; m.loans.add(barcode)
        return loan

    def return_copy(self, barcode: str, today: date) -> float:
        loan = self.loans.pop(barcode)
        loan.returned = today
        fine = self.fine_policy.fine(loan, today)
        self.fines[loan.member_id] += fine
        self.members[loan.member_id].loans.discard(barcode)
        c = self.copies[barcode]
        q = self.holds[c.isbn]
        if q:                                          # hand to the next person in the queue
            c.status, c.held_for = CopyStatus.ON_HOLD, q.popleft()
            # notify(c.held_for, ...)  -> Observer / notification service
        else:
            c.status = CopyStatus.AVAILABLE
        return fine

    def place_hold(self, member_id: str, isbn: str):
        if member_id not in self.holds[isbn]:
            self.holds[isbn].append(member_id)
```

**Extension points:** `FinePolicy` per member type (students, staff); notifications (due-soon reminders, hold available) via an observer/notification service; renewals (blocked when holds exist); hold expiry; multiple branches (copies get a `branch_id`, transfers); persistence via a repository layer so the domain logic stays testable.

### Vending machine

**Clarify:** select product, insert money, dispense, return change; out-of-stock; cancel and refund; admin restock.

**Pattern:** **State** pattern (Idle -> HasMoney -> Dispensing) keeps transitions explicit. Change-making with a greedy algorithm over available coin counts (greedy is correct for canonical coin systems; otherwise use DP).

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass

@dataclass
class Slot:
    code: str; name: str; price: int; qty: int          # price in cents

class State(ABC):
    def __init__(self, m: "VendingMachine"): self.m = m
    def insert(self, coin: int): raise RuntimeError(f"cannot insert in {type(self).__name__}")
    def select(self, code: str): raise RuntimeError(f"cannot select in {type(self).__name__}")
    def cancel(self) -> dict: return {}

class Idle(State):
    def insert(self, coin):
        self.m._accept(coin); self.m.state = HasMoney(self.m)

class HasMoney(State):
    def insert(self, coin):
        self.m._accept(coin)
    def select(self, code):
        slot = self.m.slots.get(code)
        if not slot or slot.qty == 0:
            raise ValueError("unavailable")
        if self.m.balance < slot.price:
            raise ValueError(f"insert {slot.price - self.m.balance} more")
        change = self.m._make_change(self.m.balance - slot.price)   # raises if impossible
        slot.qty -= 1
        self.m.balance = 0; self.m.state = Idle(self.m)
        return slot.name, change
    def cancel(self):
        refund = self.m._make_change(self.m.balance)
        self.m.balance = 0; self.m.state = Idle(self.m)
        return refund

class VendingMachine:
    COINS = (100, 25, 10, 5)
    def __init__(self, slots: list[Slot], coins: dict[int, int]):
        self.slots = {s.code: s for s in slots}
        self.coins = dict(coins)                         # denomination -> count in machine
        self.balance = 0
        self.state: State = Idle(self)

    def insert(self, coin): self.state.insert(coin)
    def select(self, code): return self.state.select(code)
    def cancel(self): return self.state.cancel()

    def _accept(self, coin):
        if coin not in self.COINS:
            raise ValueError("rejected coin")
        self.coins[coin] = self.coins.get(coin, 0) + 1
        self.balance += coin

    def _make_change(self, amount: int) -> dict:
        out, rem = {}, amount
        for c in sorted(self.coins, reverse=True):
            n = min(rem // c, self.coins[c])
            if n:
                out[c] = n; rem -= n * c
        if rem:
            raise RuntimeError("exact change only")
        for c, n in out.items():
            self.coins[c] -= n
        return out

    def restock(self, code: str, qty: int):
        self.slots[code].qty += qty
```

**Extension points:** new payment methods (card, mobile) behind a `PaymentProcessor` interface; new states (Maintenance, OutOfService); DP change-making for non-canonical coin sets; telemetry (low-stock events to an operator); pricing promotions. Concurrency is usually not an issue (single user at a time), but transactions should be atomic: compute change before decrementing stock, as above.

### Chess / tic-tac-toe game

**Tic-tac-toe (code it fully):** O(1) win check per move by tracking row, column and diagonal sums (+1 for X, −1 for O).

```python
class TicTacToe:
    def __init__(self, n: int = 3):
        self.n = n
        self.board = [[None] * n for _ in range(n)]
        self.rows, self.cols = [0] * n, [0] * n
        self.diag = self.anti = 0
        self.turn, self.moves, self.winner = "X", 0, None

    def move(self, r: int, c: int) -> str | None:
        """Returns 'X'/'O' on win, 'draw', or None if the game continues."""
        if self.winner:
            raise ValueError("game over")
        if not (0 <= r < self.n and 0 <= c < self.n) or self.board[r][c]:
            raise ValueError("invalid move")
        p, v = self.turn, (1 if self.turn == "X" else -1)
        self.board[r][c] = p; self.moves += 1
        self.rows[r] += v; self.cols[c] += v
        if r == c: self.diag += v
        if r + c == self.n - 1: self.anti += v
        if self.n in (abs(self.rows[r]), abs(self.cols[c]), abs(self.diag), abs(self.anti)):
            self.winner = p
            return p
        if self.moves == self.n * self.n:
            self.winner = "draw"
            return "draw"
        self.turn = "O" if p == "X" else "X"
        return None
```

**Chess (design + key code):** `Game` (players, board, turn, move history, status), `Board` (8x8 grid of `Piece | None`), abstract `Piece` with `color` and `candidate_moves(board, pos)`, subclasses per piece type, `Move` (from, to, piece, captured, promotion, flags). Legal move = candidate move that does not leave your own king in check (simulate the move, test `is_attacked(king_pos)`, undo).

```python
from abc import ABC, abstractmethod

class Piece(ABC):
    def __init__(self, color: str): self.color = color
    @abstractmethod
    def candidate_moves(self, board, pos): ...

class SlidingPiece(Piece):
    DIRS: tuple = ()
    def candidate_moves(self, board, pos):
        r, c = pos
        for dr, dc in self.DIRS:
            nr, nc = r + dr, c + dc
            while 0 <= nr < 8 and 0 <= nc < 8:
                target = board.grid[nr][nc]
                if target is None:
                    yield (nr, nc)
                else:
                    if target.color != self.color:
                        yield (nr, nc)
                    break
                nr, nc = nr + dr, nc + dc

class Rook(SlidingPiece):   DIRS = ((1, 0), (-1, 0), (0, 1), (0, -1))
class Bishop(SlidingPiece): DIRS = ((1, 1), (1, -1), (-1, 1), (-1, -1))
class Queen(SlidingPiece):  DIRS = Rook.DIRS + Bishop.DIRS

class Knight(Piece):
    def candidate_moves(self, board, pos):
        r, c = pos
        for dr, dc in ((1, 2), (2, 1), (-1, 2), (-2, 1), (1, -2), (2, -1), (-1, -2), (-2, -1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < 8 and 0 <= nc < 8 and (board.grid[nr][nc] is None or board.grid[nr][nc].color != self.color):
                yield (nr, nc)

class Game:
    def legal_moves(self, pos):
        piece = self.board.grid[pos[0]][pos[1]]
        for to in piece.candidate_moves(self.board, pos):
            captured = self.board.apply(pos, to)          # make move
            if not self.board.in_check(piece.color):
                yield to
            self.board.undo(pos, to, captured)            # unmake move
```

**Extension points:** NxN board and k-in-a-row (tic-tac-toe generalises cleanly); `Player` interface with `HumanPlayer` / `AIPlayer` (minimax with alpha-beta); special chess rules (castling, en passant, promotion) as flags on `Move` plus state on `Game`; move history enables undo, PGN export and threefold-repetition detection; timers (chess clock); networking (game server owns the authoritative state, clients send moves).

### Snake game

**Clarify:** grid W×H; snake moves one cell per tick; eating food grows it by one; game over when hitting a wall or itself.

**Data structures:** `deque` of body cells (head at the front) plus a `set` for O(1) collision checks. Moving = push new head, pop tail (unless eating). Remove the tail *before* the self-collision check, because moving into the cell the tail is vacating is legal.

```python
from collections import deque
import random

class SnakeGame:
    DIRS = {"U": (-1, 0), "D": (1, 0), "L": (0, -1), "R": (0, 1)}
    OPPOSITE = {"U": "D", "D": "U", "L": "R", "R": "L"}

    def __init__(self, width: int, height: int, food: list[tuple[int, int]] | None = None, seed=None):
        self.w, self.h = width, height
        self.body = deque([(0, 0)]); self.cells = {(0, 0)}
        self.food = deque(food) if food is not None else None   # fixed sequence (LeetCode) or random
        self.rng = random.Random(seed)
        self.cur_food = self._next_food()
        self.dir, self.score, self.over = "R", 0, False

    def _next_food(self):
        if self.food is not None:
            return self.food.popleft() if self.food else None
        free = [(r, c) for r in range(self.h) for c in range(self.w) if (r, c) not in self.cells]
        return self.rng.choice(free) if free else None

    def move(self, direction: str | None = None) -> int:
        """Advance one tick. Returns score, or -1 if game over."""
        if self.over:
            return -1
        if direction and direction != self.OPPOSITE[self.dir]:   # can't reverse into yourself
            self.dir = direction
        dr, dc = self.DIRS[self.dir]
        hr, hc = self.body[0]
        head = (hr + dr, hc + dc)
        if not (0 <= head[0] < self.h and 0 <= head[1] < self.w):
            self.over = True; return -1
        eating = head == self.cur_food
        if not eating:
            tail = self.body.pop(); self.cells.discard(tail)
        if head in self.cells:
            self.over = True; return -1
        self.body.appendleft(head); self.cells.add(head)
        if eating:
            self.score += 1
            self.cur_food = self._next_food()
        return self.score
```

**Complexity:** O(1) per move (random food placement is O(W·H) as written; keep a set of free cells to make it O(1)).

**Extension points:** wrap-around walls (modulo); obstacles; speed increasing with score; multiplayer (multiple snakes; resolve head-on collisions per tick); rendering separated from game logic (curses/pygame view calls `move` on a timer), which also makes the logic unit-testable.

### Spreadsheet with formula cells (dependency graph, cycle detection)

**Clarify:** cells hold a number or a formula like `=A1+B2*2` or `=SUM(A1:A3)`; reading a cell returns its computed value; updating a cell updates dependents; reject cycles.

**Design:** each cell stores its raw input, parsed dependencies and cached value. Maintain `deps[cell]` (cells it reads) and `rdeps[cell]` (cells that read it). On `set`: parse, check that no dependency can reach this cell (DFS through `deps`) — that would be a cycle; update edges; then recompute this cell and its transitive dependents in topological order.

```python
import re
from collections import defaultdict, deque

REF = re.compile(r"\b([A-Z]+)(\d+)\b")
RANGE = re.compile(r"\b([A-Z])(\d+):([A-Z])(\d+)\b")

class CycleError(Exception):
    pass

class Spreadsheet:
    def __init__(self):
        self.raw: dict[str, str] = {}
        self.val: dict[str, float] = {}
        self.deps: dict[str, set] = defaultdict(set)
        self.rdeps: dict[str, set] = defaultdict(set)

    @staticmethod
    def _expand_ranges(expr: str) -> str:
        def rng(m):
            c1, r1, c2, r2 = m[1], int(m[2]), m[3], int(m[4])
            cells = [f"{chr(c)}{r}" for c in range(ord(c1), ord(c2) + 1) for r in range(r1, r2 + 1)]
            return ",".join(cells)
        return RANGE.sub(rng, expr)

    def _parse(self, raw: str):
        if not raw.startswith("="):
            return None, set()
        expr = self._expand_ranges(raw[1:])
        return expr, {f"{c}{r}" for c, r in REF.findall(expr)}

    def _reaches(self, start: set, target: str) -> bool:
        stack, seen = list(start), set()
        while stack:
            x = stack.pop()
            if x == target:
                return True
            if x not in seen:
                seen.add(x); stack.extend(self.deps[x])
        return False

    def set(self, cell: str, raw: str):
        _, new_deps = self._parse(raw)
        if self._reaches(new_deps, cell):
            raise CycleError(f"{cell} would create a cycle")
        for d in self.deps[cell]:
            self.rdeps[d].discard(cell)
        self.deps[cell] = new_deps
        for d in new_deps:
            self.rdeps[d].add(cell)
        self.raw[cell] = raw
        self._recompute_from(cell)

    def get(self, cell: str) -> float:
        return self.val.get(cell, 0)

    def _eval(self, cell: str) -> float:
        raw = self.raw.get(cell, "")
        expr, _ = self._parse(raw)
        if expr is None:
            return float(raw) if raw else 0.0
        expr = REF.sub(lambda m: repr(self.get(m[0])), expr)
        # only numbers, operators and SUM may remain; use a real parser in production
        if not re.fullmatch(r"[\d\s.+\-*/(),eE]*", expr.replace("SUM", "")):
            raise ValueError("bad formula")
        return eval(expr, {"__builtins__": {}}, {"SUM": lambda *a: sum(a)})

    def _recompute_from(self, cell: str):
        # collect affected cells, then Kahn's topological order over that subgraph
        affected, stack = set(), [cell]
        while stack:
            x = stack.pop()
            if x not in affected:
                affected.add(x); stack.extend(self.rdeps[x])
        indeg = {x: sum(1 for d in self.deps[x] if d in affected) for x in affected}
        q = deque(x for x, d in indeg.items() if d == 0)
        while q:
            x = q.popleft()
            self.val[x] = self._eval(x)
            for y in self.rdeps[x]:
                if y in affected:
                    indeg[y] -= 1
                    if indeg[y] == 0:
                        q.append(y)

s = Spreadsheet()
s.set("A1", "2"); s.set("A2", "3"); s.set("B1", "=SUM(A1:A2)*10")
assert s.get("B1") == 50
s.set("A1", "5"); assert s.get("B1") == 80
```

In the interview, say explicitly: "I'm using a restricted `eval` for speed; in production I would reuse the recursive-descent parser from the calculator problem and add functions to its grammar." That is the honest trade-off.

**Complexity:** `set` is O(V + E) worst case for the cycle check plus recomputation of affected cells only; `get` O(1).

**Extension points:** more functions (`AVG`, `IF`) in the parser's function table; error values (`#DIV/0!`, `#REF!`) instead of exceptions; lazy evaluation (mark dirty, compute on read); undo/redo (Command pattern storing old raw values); collaborative editing (operational transforms / CRDTs, server-side recalculation).

### Inventory management system

**Clarify:** products (SKU) stocked across warehouses; receive stock, reserve for orders, fulfil (ship), release on cancel; low-stock alerts; audit trail.

**Key idea:** separate **on hand** from **reserved**; `available = on_hand − reserved`. Every change is recorded as an immutable movement (ledger), which gives auditability and lets you rebuild state.

**Classes:** `Product`, `Warehouse`, `StockLevel` (per SKU per warehouse), `Reservation`, `Movement` (ledger entry), `InventoryService`, `AllocationStrategy`, `StockObserver` for alerts.

```python
import threading, uuid
from collections import defaultdict
from dataclasses import dataclass, field
from datetime import datetime, timezone

@dataclass
class StockLevel:
    on_hand: int = 0
    reserved: int = 0
    @property
    def available(self): return self.on_hand - self.reserved

@dataclass
class Reservation:
    id: str; order_id: str; sku: str; warehouse: str; qty: int; status: str = "active"

@dataclass(frozen=True)
class Movement:
    sku: str; warehouse: str; delta_on_hand: int; delta_reserved: int; reason: str; ref: str
    at: datetime = field(default_factory=lambda: datetime.now(timezone.utc))

class InventoryService:
    def __init__(self, low_stock_threshold: dict[str, int] | None = None):
        self.stock = defaultdict(StockLevel)              # (sku, warehouse) -> StockLevel
        self.reservations: dict[str, Reservation] = {}
        self.ledger: list[Movement] = []
        self.thresholds = low_stock_threshold or {}
        self.observers = []                               # callables(sku, warehouse, available)
        self.lock = threading.Lock()

    def _apply(self, sku, wh, d_on, d_res, reason, ref):
        lvl = self.stock[(sku, wh)]
        if lvl.on_hand + d_on < 0 or lvl.reserved + d_res < 0 or lvl.available + d_on - d_res < 0:
            raise ValueError("insufficient stock")
        lvl.on_hand += d_on; lvl.reserved += d_res
        self.ledger.append(Movement(sku, wh, d_on, d_res, reason, ref))
        if lvl.available <= self.thresholds.get(sku, -1):
            for obs in self.observers:
                obs(sku, wh, lvl.available)

    def receive(self, sku, wh, qty, po_id):
        with self.lock:
            self._apply(sku, wh, qty, 0, "receive", po_id)

    def reserve(self, order_id, sku, qty) -> Reservation:
        with self.lock:
            # allocation strategy: warehouse with the most available stock (swap for nearest-to-customer)
            candidates = [(lvl.available, wh) for (s, wh), lvl in self.stock.items() if s == sku]
            avail, wh = max(candidates, default=(0, None))
            if avail < qty:
                raise ValueError("insufficient stock")
            r = Reservation(str(uuid.uuid4()), order_id, sku, wh, qty)
            self._apply(sku, wh, 0, qty, "reserve", r.id)
            self.reservations[r.id] = r
            return r

    def release(self, res_id):
        with self.lock:
            r = self.reservations[res_id]
            if r.status != "active": return
            self._apply(r.sku, r.warehouse, 0, -r.qty, "release", r.id); r.status = "released"

    def fulfil(self, res_id):
        with self.lock:
            r = self.reservations[res_id]
            if r.status != "active": raise ValueError("not active")
            self._apply(r.sku, r.warehouse, -r.qty, -r.qty, "ship", r.id); r.status = "fulfilled"

    def adjust(self, sku, wh, delta, reason):            # cycle count, damage, shrinkage
        with self.lock:
            self._apply(sku, wh, delta, 0, f"adjust:{reason}", "")
```

**Extension points:** `AllocationStrategy` (nearest warehouse, split shipments across warehouses, FIFO/FEFO by lot and expiry date); reservation expiry (TTL so abandoned carts release stock); lot/serial tracking; event publishing (Kafka) for downstream systems. In a database, enforce `available >= 0` with a conditional update (`UPDATE ... SET reserved = reserved + :q WHERE sku=:s AND on_hand - reserved >= :q`) to avoid overselling under concurrency, and make operations idempotent by request id.

### Hotel / restaurant reservation system

**Clarify (hotel):** room types; search availability for a date range; book, cancel; no double booking; holds during checkout; pricing per night. (Restaurant is the same shape with time slots and party size instead of nights and room types.)

**Classes:** `Room` (id, type, capacity), `Booking` (id, room, guest, check_in, check_out, status), `ReservationService`, `PricingStrategy`, `AssignmentStrategy`.

**Key logic:** two bookings overlap if `a.start < b.end and b.start < a.end` (half-open dates: check-out day is free for the next guest).

```python
import threading, uuid
from dataclasses import dataclass
from datetime import date, timedelta

@dataclass
class Room:
    id: str; type: str; capacity: int; base_rate: float

@dataclass
class Booking:
    id: str; room_id: str; guest: str; start: date; end: date; status: str = "confirmed"

class PricingStrategy:
    def price(self, room: Room, start: date, end: date) -> float:
        total, d = 0.0, start
        while d < end:
            total += room.base_rate * (1.2 if d.weekday() in (4, 5) else 1.0)   # Fri/Sat nights uplift
            d += timedelta(days=1)
        return round(total, 2)

class ReservationService:
    def __init__(self, rooms: list[Room], pricing: PricingStrategy = PricingStrategy()):
        self.rooms = {r.id: r for r in rooms}
        self.bookings: dict[str, list[Booking]] = {r.id: [] for r in rooms}
        self.by_id: dict[str, Booking] = {}
        self.pricing = pricing
        self.lock = threading.Lock()

    def _free(self, room_id, start, end) -> bool:
        return all(not (b.start < end and start < b.end)
                   for b in self.bookings[room_id] if b.status == "confirmed")

    def search(self, room_type: str, start: date, end: date, guests: int = 1):
        if start >= end:
            raise ValueError("invalid range")
        return [(r, self.pricing.price(r, start, end)) for r in self.rooms.values()
                if r.type == room_type and r.capacity >= guests and self._free(r.id, start, end)]

    def book(self, guest: str, room_type: str, start: date, end: date, guests: int = 1) -> Booking:
        with self.lock:                                 # check-then-act must be atomic
            options = self.search(room_type, start, end, guests)
            if not options:
                raise ValueError("no availability")
            room, _ = min(options, key=lambda o: o[0].capacity)   # assignment: smallest room that fits
            b = Booking(str(uuid.uuid4()), room.id, guest, start, end)
            self.bookings[room.id].append(b); self.by_id[b.id] = b
            return b

    def cancel(self, booking_id: str):
        with self.lock:
            self.by_id[booking_id].status = "cancelled"
```

**Complexity:** `_free` is O(bookings per room) as written. For scale, keep each room's bookings sorted and binary-search (O(log n)), or store a per-date inventory count per room type (`room_type, date -> booked`), which is how real hotel systems do it: availability = all dates in the range have `booked < total`.

**Extension points:** pricing (seasonal, dynamic, member discounts); overbooking policy; holds with expiry during payment (status `held` with TTL); cancellation policies and refunds; restaurant variant: tables combinable for big parties, slot duration, turn times. In a database, prevent double booking with an exclusion constraint (PostgreSQL `EXCLUDE USING gist (room_id WITH =, daterange(start, end) WITH &&)`) or row locks on the date-inventory rows.

### Ride-sharing matching (simplified)

**Clarify:** riders request a ride from a location; nearby available drivers; match the best driver (nearest, or best ETA); driver accepts/declines with a timeout; trip lifecycle.

**Classes:** `Location`, `Driver` (id, location, status), `Rider`, `RideRequest`, `Trip` (state machine: REQUESTED -> MATCHED -> PICKED_UP -> COMPLETED / CANCELLED), `DriverIndex` (spatial index), `MatchingStrategy`, `MatchingService`.

**Spatial index:** bucket drivers into grid cells (geohash / H3 in production); search the rider's cell and expanding rings of neighbours until enough candidates.

```python
import math, threading
from collections import defaultdict
from dataclasses import dataclass
from enum import Enum

class DriverStatus(Enum):
    AVAILABLE = 1; OFFERED = 2; ON_TRIP = 3; OFFLINE = 4

@dataclass
class Driver:
    id: str; lat: float; lng: float; status: DriverStatus = DriverStatus.AVAILABLE; rating: float = 5.0

def haversine_km(a_lat, a_lng, b_lat, b_lng):
    p1, p2 = math.radians(a_lat), math.radians(b_lat)
    dphi, dl = p2 - p1, math.radians(b_lng - a_lng)
    h = math.sin(dphi / 2) ** 2 + math.cos(p1) * math.cos(p2) * math.sin(dl / 2) ** 2
    return 2 * 6371 * math.asin(math.sqrt(h))

class GridIndex:
    def __init__(self, cell_deg=0.01):                   # ~1 km cells
        self.cell, self.cells, self.where = cell_deg, defaultdict(set), {}
    def _key(self, lat, lng):
        return (int(lat // self.cell), int(lng // self.cell))
    def upsert(self, d: Driver):
        k = self._key(d.lat, d.lng)
        old = self.where.get(d.id)
        if old != k:
            if old: self.cells[old].discard(d.id)
            self.cells[k].add(d.id); self.where[d.id] = k
    def nearby(self, lat, lng, rings=3):
        ci, cj = self._key(lat, lng)
        for r in range(rings + 1):                       # expanding rings
            found = {d for i in range(ci - r, ci + r + 1) for j in range(cj - r, cj + r + 1)
                     if max(abs(i - ci), abs(j - cj)) == r for d in self.cells.get((i, j), ())}
            yield from found

class MatchingService:
    def __init__(self):
        self.drivers: dict[str, Driver] = {}
        self.index = GridIndex()
        self.lock = threading.Lock()

    def update_location(self, driver_id, lat, lng):
        with self.lock:
            d = self.drivers.setdefault(driver_id, Driver(driver_id, lat, lng))
            d.lat, d.lng = lat, lng
            self.index.upsert(d)

    def match(self, lat, lng, k=5, exclude=frozenset()) -> Driver | None:
        """Pick the best available driver and atomically mark them OFFERED."""
        with self.lock:
            cands = []
            for did in self.index.nearby(lat, lng):
                d = self.drivers[did]
                if d.status == DriverStatus.AVAILABLE and did not in exclude:
                    cands.append((haversine_km(lat, lng, d.lat, d.lng) - 0.1 * d.rating, did))
                    if len(cands) >= 3 * k:
                        break
            if not cands:
                return None
            _, best = min(cands)
            self.drivers[best].status = DriverStatus.OFFERED
            return self.drivers[best]

    def respond(self, driver_id, accepted: bool):
        with self.lock:
            self.drivers[driver_id].status = DriverStatus.ON_TRIP if accepted else DriverStatus.AVAILABLE
```

On decline or a 15-second timeout, call `match` again with the declined driver in `exclude`.

**Extension points:** `MatchingStrategy` (ETA from a routing service instead of straight-line distance; batch matching every few seconds solved as an assignment problem with the Hungarian algorithm; pooling); surge pricing per cell; driver preferences. At scale: location updates go to an in-memory geo store (Redis GEO, sharded by region), and matching is partitioned by city.

### Logger with rate limiting ("print message only if not printed in last 10s")

**Approach:** map message -> last printed timestamp. Allow if never seen or `now - last >= 10`. To bound memory, also keep a queue of (timestamp, message) and evict entries older than 10s.

```python
from collections import deque

class Logger:
    def __init__(self, window: int = 10):
        self.window = window
        self.last: dict[str, int] = {}
        self.q: deque = deque()                       # (ts, msg) in print order

    def should_print(self, timestamp: int, message: str) -> bool:
        while self.q and self.q[0][0] <= timestamp - self.window:   # evict expired
            ts, msg = self.q.popleft()
            if self.last.get(msg) == ts:
                del self.last[msg]
        if message in self.last:
            return False
        self.last[message] = timestamp
        self.q.append((timestamp, message))
        return True
```

**Complexity:** O(1) amortised per call; space O(distinct messages in the last window).

**Edge cases:** timestamps assumed non-decreasing (clarify; if not, use the simple dict version); the boundary (exactly 10s later is allowed); identical messages at the same timestamp.

**Extension points:** per-message window or severity-based windows; count suppressed messages and emit "message X repeated N times" (like syslog); thread safety (lock); key by normalised message (strip ids) to rate-limit "similar" messages; plug into Python `logging` as a `logging.Filter`.

### Hit counter (hits in last 5 minutes)

**Approach:** circular buffer of 300 buckets (one per second), each storing `(timestamp, count)`. A hit at time t goes into bucket `t % 300`; if the bucket holds an old timestamp, reset it. `get_hits(t)` sums buckets whose timestamp is within the last 300 seconds. Fixed memory regardless of traffic.

```python
import threading

class HitCounter:
    def __init__(self, window: int = 300):
        self.window = window
        self.times = [-window] * window           # so t=0 is not mistaken for a hit
        self.counts = [0] * window
        self.lock = threading.Lock()

    def hit(self, timestamp: int) -> None:
        i = timestamp % self.window
        with self.lock:
            if self.times[i] != timestamp:
                self.times[i], self.counts[i] = timestamp, 1
            else:
                self.counts[i] += 1

    def get_hits(self, timestamp: int) -> int:
        with self.lock:
            return sum(c for t, c in zip(self.times, self.counts) if timestamp - t < self.window)
```

**Complexity:** hit O(1); get_hits O(W) with W = 300 (constant); space O(W). Alternative: a deque of timestamps, O(1) amortised but memory grows with hits per window.

**Edge cases:** out-of-order timestamps (the bucket approach tolerates small disorder within a second; a late hit for an overwritten bucket is dropped); many hits in the same second (counts, not individual entries); timestamp 0 colliding with initial values (hence `times` starts at −window).

**Extension points:** per-endpoint counters (dict of counters); finer granularity (100ms buckets); multiple windows (1m/5m/1h) by layering buckets; distributed: each server keeps local counters and a collector sums them, or use Redis `INCR` on per-second keys with `EXPIRE`.

### Event-driven notification system

**Clarify:** domain events (order shipped, payment failed) trigger notifications to users over channels (email, SMS, push, in-app) according to user preferences; templates; retries; no duplicates; rate limits / quiet hours.

**Classes:**
- `Event` (type, payload, idempotency key)
- `NotificationRule` / routing: event type -> template + default channels
- `UserPreferences` (opted-in channels, quiet hours)
- `Channel` interface (`send(recipient, message)`) with `EmailChannel`, `SmsChannel`, `PushChannel`
- `TemplateRenderer`
- `NotificationService` (subscribes to the event bus, fans out, dedupes, retries, records delivery status)

```python
import logging, time
from abc import ABC, abstractmethod
from dataclasses import dataclass
from string import Template

log = logging.getLogger(__name__)

@dataclass
class Event:
    id: str; type: str; user_id: str; data: dict

class Channel(ABC):
    name: str
    @abstractmethod
    def send(self, to: str, subject: str, body: str) -> None: ...

class EmailChannel(Channel):
    name = "email"
    def send(self, to, subject, body):
        print(f"EMAIL to {to}: {subject} | {body}")       # SES / SendGrid client in production

class SmsChannel(Channel):
    name = "sms"
    def send(self, to, subject, body):
        print(f"SMS to {to}: {body[:160]}")               # Twilio in production

TEMPLATES = {
    "order.shipped": ("Your order shipped", Template("Order $order_id is on the way. Track: $url")),
}

class NotificationService:
    def __init__(self, channels: list[Channel], users, prefs, max_retries=3):
        self.channels = {c.name: c for c in channels}
        self.users, self.prefs = users, prefs              # user_id -> contact info, user_id -> [channels]
        self.sent: set[tuple] = set()                      # (event_id, channel) for idempotency
        self.max_retries = max_retries
        self.dead_letters: list = []

    def handle(self, event: Event):
        if event.type not in TEMPLATES:
            return
        subject, tmpl = TEMPLATES[event.type]
        body = tmpl.safe_substitute(event.data)
        for ch_name in self.prefs.get(event.user_id, ["email"]):
            key = (event.id, ch_name)
            if key in self.sent:
                continue                                    # duplicate event delivery
            to = self.users[event.user_id].get(ch_name)
            if not to:
                continue
            for attempt in range(self.max_retries):
                try:
                    self.channels[ch_name].send(to, subject, body)
                    self.sent.add(key)
                    break
                except Exception:
                    log.warning("send failed", exc_info=True)
                    time.sleep(0.1 * 2 ** attempt)
            else:
                self.dead_letters.append((event, ch_name))

# wiring: bus.subscribe("order.*", service.handle)   (EventBus from section A)
```

**Production shape:** producers publish events to Kafka/SNS; a notification service consumes, resolves preferences, renders, and enqueues per-channel jobs (SQS queue per channel) so a slow SMS provider does not block email; workers send with retries and provider failover; delivery status stored for audit and the in-app inbox.

**Extension points:** new channel = new `Channel` subclass (Slack, WhatsApp, webhook); templates per locale; digesting/batching (collapse 10 events into one email); quiet hours and per-user rate limits; priority lanes (security alerts bypass digests); store the idempotency set in Redis/DB with TTL instead of memory.

### Simple workflow/state machine engine (e.g., order lifecycle)

**Approach:** a declarative transition table `(state, event) -> (next_state, guard, action)`. The engine validates transitions, runs guards and actions, records history, and notifies listeners. Keep the definition (data) separate from the engine (code), so new workflows need no new engine code.

```python
from dataclasses import dataclass, field
from datetime import datetime, timezone
from typing import Callable

@dataclass
class Transition:
    source: str
    event: str
    target: str
    guard: Callable[[dict], bool] = lambda ctx: True
    action: Callable[[dict], None] = lambda ctx: None

class InvalidTransition(Exception):
    pass

class StateMachine:
    def __init__(self, initial: str, transitions: list[Transition], terminal=frozenset()):
        self.initial, self.terminal = initial, terminal
        self.table = {(t.source, t.event): t for t in transitions}
        self.listeners: list[Callable] = []

    def allowed_events(self, state: str) -> list[str]:
        return [e for (s, e) in self.table if s == state]

    def fire(self, entity: "Workflow", event: str, **data):
        t = self.table.get((entity.state, event))
        if t is None:
            raise InvalidTransition(f"{event!r} not allowed in state {entity.state!r}")
        ctx = {**entity.context, **data}
        if not t.guard(ctx):
            raise InvalidTransition(f"guard rejected {event!r} in {entity.state!r}")
        t.action(ctx)                                    # side effects; raise to abort
        prev, entity.state = entity.state, t.target
        entity.context = ctx
        entity.history.append((datetime.now(timezone.utc), prev, event, t.target))
        for fn in self.listeners:
            fn(entity, prev, event, t.target)

@dataclass
class Workflow:
    id: str
    state: str
    context: dict = field(default_factory=dict)
    history: list = field(default_factory=list)
    version: int = 0                                     # optimistic locking when persisted

ORDER = StateMachine(
    initial="created",
    terminal={"delivered", "cancelled", "refunded"},
    transitions=[
        Transition("created", "pay", "paid", guard=lambda c: c.get("amount", 0) > 0),
        Transition("created", "cancel", "cancelled"),
        Transition("paid", "ship", "shipped", guard=lambda c: bool(c.get("address"))),
        Transition("paid", "cancel", "refunded", action=lambda c: print("refund", c.get("amount"))),
        Transition("shipped", "deliver", "delivered"),
    ],
)

order = Workflow("o1", ORDER.initial, {"amount": 42, "address": "1 Main St"})
ORDER.fire(order, "pay"); ORDER.fire(order, "ship"); ORDER.fire(order, "deliver")
assert order.state == "delivered"
```

**Extension points:** load definitions from YAML/JSON (versioned, so in-flight orders keep the definition they started with); timeouts (scheduled events like "auto-cancel if unpaid after 30 min"); async/long-running actions (enqueue jobs; the job fires the next event on completion); persistence with optimistic locking (`UPDATE ... WHERE id=? AND version=?`) to make concurrent events safe; outbox pattern for publishing state-change events reliably; hierarchical/parallel states (statecharts). Mention when to use a real engine instead: Temporal, AWS Step Functions, Camunda.

---

## D. Questions asked during/after coding

### What's the time and space complexity?

**What they want:** a precise, unprompted answer for every function you write, including the costs people forget.

**How to answer:**
- Name the variables first: "n is the number of records, k the number of results, d the embedding dimension."
- Give time and space separately, and say which dominates: "Sorting is O(n log n), the scan is O(n), so overall O(n log n). Extra space is O(n) for the hash map; the output is O(k)."
- Mention hidden costs: string slicing and concatenation (O(length) each), `list.pop(0)` and `x in list` (O(n)), `sorted()` inside a loop, recursion stack depth, Python's `heapq` being a min-heap, hash map worst case O(n) with collisions.
- Distinguish amortised (list append, dynamic array growth), average (hash map), and worst case (quicksort).
- For I/O-bound code, say what actually matters: number of network calls, round trips, bytes read. "This is O(n) CPU but n/100 HTTP requests, and latency dominates."
- Offer the trade-off: "I can get O(1) lookups with an extra O(n) index if reads dominate."

**Pitfall:** saying "O(n)" without saying what n is, or forgetting the recursion stack in DFS.

### How would you test this? Write the tests.

**Approach:** start with a few example-based tests (happy path), then edge cases, then error cases; mention property-based tests and integration tests. Use pytest, parametrise, and inject dependencies (clock, HTTP client, random) so tests are deterministic and fast.

```python
import pytest

# --- example + edge cases with parametrize (merge intervals from section A)
@pytest.mark.parametrize("given,expected", [
    ([], []),
    ([[1, 3]], [[1, 3]]),
    ([[1, 3], [2, 6], [8, 10]], [[1, 6], [8, 10]]),
    ([[1, 4], [4, 5]], [[1, 5]]),                 # touching
    ([[5, 6], [1, 2]], [[1, 2], [5, 6]]),         # unsorted input
    ([[1, 10], [2, 3]], [[1, 10]]),               # containment
])
def test_merge(given, expected):
    assert merge(given) == expected

# --- time-dependent code: inject a fake clock instead of sleeping
class FakeClock:
    def __init__(self): self.t = 0.0
    def __call__(self): return self.t
    def advance(self, s): self.t += s

def test_rate_limiter_refills():
    clock = FakeClock()
    rl = RateLimiter(user_cap=2, user_rate=1, global_cap=100, global_rate=100, clock=clock)
    assert rl.allow("u") and rl.allow("u")
    assert not rl.allow("u")                      # bucket empty
    clock.advance(1.0)
    assert rl.allow("u")                          # one token refilled
    assert rl.allow("v")                          # other users unaffected

# --- external calls: mock the dependency
def test_retry_gives_up_after_max_attempts():
    calls = []
    @retry(max_attempts=3, sleep=lambda s: None)
    def flaky():
        calls.append(1); raise TimeoutError()
    with pytest.raises(TimeoutError):
        flaky()
    assert len(calls) == 3

# --- property-based: compare against a brute-force oracle
from hypothesis import given, strategies as st

@given(st.lists(st.tuples(st.integers(0, 50), st.integers(0, 50)).map(lambda t: [min(t), max(t)])))
def test_merge_property(intervals):
    out = merge([iv[:] for iv in intervals])
    assert all(out[i][1] < out[i + 1][0] for i in range(len(out) - 1))       # sorted, disjoint
    for s, e in intervals:                                                   # every input covered
        assert any(a <= s and e <= b for a, b in out)
```

**Say also:** what you would test at other levels (an integration test against a real Postgres in Docker, a contract test for the third-party API, a load test for the rate limiter), and that you would add a regression test for every bug found.

### What edge cases did you consider (empty, nulls, duplicates, huge inputs, unicode)?

**What they want:** a systematic checklist, ideally raised *before* coding, then encoded as tests.

A checklist to run through out loud:
- **Empty / minimal:** empty list or string, single element, zero, k = 0, k > n.
- **Nulls / missing:** `None` values, missing dict keys, missing CSV columns, optional fields in API responses.
- **Duplicates:** repeated values, duplicate keys/ids, the same event delivered twice (idempotency).
- **Boundaries:** off-by-one at window edges (`<` vs `<=`), touching intervals, inclusive vs exclusive ranges, integer limits (not an issue in Python, but is in Java/Go), negative numbers, floats (`0.1 + 0.2`, use `Decimal` for money).
- **Ordering:** unsorted input when you assumed sorted; ties in sorting (define a stable tiebreak); out-of-order timestamps.
- **Huge inputs:** recursion depth (Python default limit ~1000; use iteration), O(n^2) blow-ups, memory (stream instead of loading), very long lines.
- **Unicode / text:** multi-byte characters, combining characters and emoji (`len("é")` can be 1 or 2 depending on normalisation; use `unicodedata.normalize("NFC", s)`), case folding (`casefold()`), BOMs, mixed encodings, right-to-left text.
- **Time:** time zones, DST transitions, clocks going backwards (use `time.monotonic()` for durations).
- **Concurrency / failure:** partial failure mid-batch, retries causing duplicates, timeouts, the same operation run concurrently.
- **Malicious input:** injection (never `eval`, parameterise SQL), path traversal in file names, giant payloads.

**Tip:** you do not need to handle all of these in a 45-minute round. Name them, handle the important ones, and say explicitly which you are deferring ("I'll assume input is valid ASCII for now; in production I'd normalise unicode here").

### How would this change if the data didn't fit in memory?

**Framework:** stream, partition, or approximate, or push the work into a system built for it.

- **Stream it:** process line by line or in chunks (generators, `pandas.read_csv(chunksize=...)`, `ijson` for big JSON). Works whenever the state you keep is small (counts, running sums, top-K heap).
- **Partition (divide and conquer):** hash-partition records by key into P files on disk so each partition fits in memory; process each independently (group-by, dedupe, joins). This is the MapReduce idea.
- **External sort:** sort chunks that fit in memory, write them as runs, then k-way merge with a heap (`heapq.merge`). Enables merge-joins and diffs of two huge sorted files.
- **Approximate / probabilistic structures:** Bloom filter for "seen before?", HyperLogLog for distinct counts, Count-Min Sketch + heap for top-K, reservoir sampling for a uniform sample. State the error bounds.
- **Use an index or database:** SQLite/DuckDB on a laptop handle tens of GB with SQL; for embeddings, use an on-disk ANN index (FAISS with IVF-PQ, DiskANN) instead of a brute-force matrix.
- **Distribute:** Spark/Dask/Ray or a warehouse (BigQuery, Snowflake) when one machine is not enough.

Example: "Top-K endpoints from 500 GB of logs: if unique endpoints fit in memory, stream and count with a dict. If not, hash-partition by endpoint into 100 files, count each, keep a global top-K heap. Or just load into BigQuery and run a GROUP BY; for a customer engagement, that is often the right answer."

### How would you make this thread-safe?

**Approach:** identify the shared mutable state and the invariants that span multiple fields, then pick the simplest mechanism that protects them.

- **Single lock (coarse-grained):** wrap each public method in `with self.lock:`. Correct and simple; start here. Protect check-then-act sequences (check balance, then debit) as one critical section.
- **Finer-grained:** lock striping (N locks, key hashed to a lock) for a cache or map with heavy contention; readers-writer lock when reads dominate.
- **Avoid shared state:** immutable data, copy-on-write snapshots (readers see a consistent snapshot), message passing (a single worker thread owns the state and others send commands via a `queue.Queue`).
- **Atomic primitives:** `queue.Queue`, `threading.Semaphore`, `collections.deque.append/popleft` (thread-safe in CPython), `itertools.count` for ids.
- **Pitfalls:** the GIL does not make compound operations atomic (`d[k] += 1` is read-modify-write); deadlocks from acquiring multiple locks in different orders (fixed lock ordering, or `RLock` for re-entrancy); calling user callbacks while holding a lock (copy the list, release, then call); holding locks during I/O.
- **Async code:** single-threaded, so no data races between `await` points, but invariants can break across an `await`; use `asyncio.Lock`.
- **Multiple processes/servers:** in-process locks no longer help; use database transactions, conditional updates, optimistic concurrency (version columns), or Redis atomic operations.

```python
import threading
from collections import OrderedDict

class ThreadSafeLRU:
    def __init__(self, cap):
        self.cap, self.d, self.lock = cap, OrderedDict(), threading.Lock()
    def get(self, k, default=None):
        with self.lock:                          # move_to_end mutates, so reads need the lock too
            if k not in self.d:
                return default
            self.d.move_to_end(k)
            return self.d[k]
    def put(self, k, v):
        with self.lock:
            self.d[k] = v; self.d.move_to_end(k)
            if len(self.d) > self.cap:
                self.d.popitem(last=False)
```

**Testing it:** hammer the object from many threads and check invariants (e.g. total balance conserved); use `sys.setswitchinterval` with a tiny value to increase interleavings.

### How would this work across multiple servers (distributed rate limiter, distributed cache)?

**Distributed rate limiter:**
- **Centralised counter store:** Redis. Sliding window with a sorted set per key (`ZREMRANGEBYSCORE`, `ZADD`, `ZCARD`), or a token bucket stored as a hash, executed in a **Lua script** so read-modify-write is atomic. Set TTLs so idle keys expire.
- **Trade-offs:** one Redis round trip per request (~1 ms); Redis is a dependency, so decide fail-open (allow traffic if Redis is down; usual for user-facing APIs) or fail-closed (for expensive or abusive endpoints).
- **Scale/latency optimisation:** local token buckets per server with a slice of the global quota (limit / N servers), periodically rebalanced; accept slight over-admission. Or sticky routing (consistent hashing on user id) so one server owns each user.
- **Where it lives:** often at the gateway (Envoy, NGINX, Kong, API Gateway) rather than in app code.

**Distributed cache:**
- **Partitioning:** consistent hashing (with virtual nodes) so adding/removing a node only moves ~1/N of keys; Redis Cluster uses 16384 hash slots.
- **Replication:** primary/replica for availability; reads from replicas may be stale.
- **Patterns:** cache-aside (app reads cache, falls back to DB, populates) is the default; write-through/write-behind for write-heavy cases.
- **Invalidation and consistency:** TTLs as a safety net; delete-on-write (not update) to avoid races; versioned keys; pub/sub to invalidate local in-process caches.
- **Failure modes:** thundering herd / cache stampede on a hot key expiry (request coalescing / single-flight, probabilistic early refresh, jittered TTLs); hot keys (replicate them or add a local L1 cache); cold start after a flush.

**Generic principle to state:** "anything that was a local variable becomes shared state, so I need atomic operations on a shared store, a partitioning scheme, and a decision about consistency vs availability when that store is unreachable."

### Now the requirement changed to X — how do you extend your code?

**What they want:** to see that your code was structured for change and that you can modify it calmly without breaking what works (this is the core of the "progressive" format reportedly used at Anthropic, OpenAI and Ramp, and of CodeSignal-style rounds).

**How to handle it live:**
1. **Restate the new requirement and clarify** its interaction with existing behaviour ("Should scheduled payments count towards top spenders?").
2. **Say where the change goes** before typing: "This is a new pricing rule, so it's a new `PricingStrategy`; nothing else changes." If the change touches many places, that is a signal to refactor first.
3. **Make the smallest change that keeps old tests green**, then add tests for the new behaviour.
4. **Refactor if needed** in a separate, visible step: extract a helper, introduce an interface, move from a dict to a class.

**Design habits that make extension cheap (mention them):**
- Isolate the parts likely to vary behind interfaces (Strategy for pricing/matching/allocation, State for lifecycles, Observer for side effects such as notifications).
- Keep pure logic separate from I/O so it can be tested and reused.
- Inject dependencies (clock, storage, HTTP client) rather than hard-coding them.
- Centralise validation and shared steps (e.g. `_process_due(ts)` called at the start of every bank operation).
- Prefer data-driven configuration (transition tables, rule tables) over branching code.

**Pitfall:** rewriting everything from scratch, or bolting on `if` flags in five places. Also avoid over-engineering at level 1: build simple, but put seams where change is likely.

### How would you productionize this (logging, metrics, config, error handling)?

A checklist to walk through (pick the relevant items; do not recite all of them):
- **Error handling:** validate inputs at the boundary; specific exception types; retries with backoff only for transient errors; timeouts on every network call; idempotency keys for retried writes; dead-letter queue for poison messages; never swallow exceptions silently.
- **Logging:** structured JSON logs with a request/correlation id; log at boundaries (request in/out, external calls, errors with stack traces); no secrets or PII in logs; sampling for high-volume debug logs.
- **Metrics:** RED (rate, errors, duration as p50/p95/p99) for services, USE for resources, plus business metrics (orders processed, rows rejected). Prometheus/Datadog/CloudWatch; dashboards and alerts on SLOs, not on every error.
- **Tracing:** OpenTelemetry spans across service calls to find latency sources.
- **Config:** environment variables or a config service; typed and validated at startup (Pydantic settings); secrets in a secret manager (AWS Secrets Manager, Vault), never in code; feature flags for risky changes.
- **Reliability:** health/readiness endpoints; graceful shutdown (drain queues, flush batches); bounded queues and back-pressure; rate limits and circuit breakers on dependencies.
- **Testing and delivery:** unit + integration tests in CI, type checking (mypy), linting; canary or staged rollout with fast rollback.
- **Security:** least-privilege credentials, input sanitisation, dependency scanning, audit logs for sensitive actions.
- **Documentation and ownership:** README, runbook for on-call, clear owner.

Example for the CSV ingester: "Run it as a job with a run id; log per-file counts; emit `rows_loaded` and `rows_rejected` metrics and alert if the rejection rate exceeds 5%; load into a staging table and swap atomically; write rejected rows to S3 for the customer; config for schema and thresholds per customer."

### Can you refactor this to be more readable?

**What they want:** that you can recognise and fix code smells quickly and explain why the new version is better, without changing behaviour.

**Common moves:**
- Extract well-named helper functions for each step (parse, validate, transform, write); one level of abstraction per function.
- Replace nested conditionals with guard clauses / early returns.
- Replace magic numbers and strings with named constants or enums.
- Replace tuples and loose dicts with `dataclass`es that name their fields.
- Replace long `if/elif` chains on a type with a dispatch dict or polymorphism.
- Use the standard library idioms: `collections.Counter`, `defaultdict`, `enumerate`, `zip`, comprehensions (but not nested three deep).
- Remove duplication; but do not over-abstract two similar lines.
- Add type hints and a short docstring for intent and edge cases.
- Keep behaviour identical: run the tests before and after.

```python
# Before
def f(d):
    r = []
    for x in d:
        if x["s"] == 1:
            if x["a"] > 100:
                r.append((x["id"], x["a"] * 0.9))
            else:
                r.append((x["id"], x["a"]))
    return r

# After
from dataclasses import dataclass

ACTIVE = 1
DISCOUNT_THRESHOLD = 100
DISCOUNT_RATE = 0.10

@dataclass
class PricedOrder:
    order_id: str
    price: float

def discounted_price(amount: float) -> float:
    return amount * (1 - DISCOUNT_RATE) if amount > DISCOUNT_THRESHOLD else amount

def price_active_orders(orders: list[dict]) -> list[PricedOrder]:
    return [PricedOrder(o["id"], discounted_price(o["a"])) for o in orders if o["s"] == ACTIVE]
```

**Say:** what you changed and why ("named the business rule, removed nesting, typed the output"), and what you would *not* change in the interview (behaviour, public interfaces) without asking.

### Why did you pick this data structure?

**What they want:** that your choice follows from the operations you need and their frequencies, and that you know the alternatives.

**How to answer:** "The hot operations are X and Y; this structure makes them O(...); the alternative Z would make X O(...), which matters because ..."

Quick reference to draw on:

| Need | Structure | Why |
|---|---|---|
| Lookup / membership by key | `dict` / `set` | O(1) average |
| Recency order + O(1) lookup | `OrderedDict` (hash map + doubly linked list) | LRU |
| Repeated min/max, top-K | `heapq` | O(log n) push/pop, O(n log k) top-K |
| FIFO, sliding windows, BFS | `collections.deque` | O(1) at both ends (list `pop(0)` is O(n)) |
| Sorted order with inserts | `bisect` on a list, or `sortedcontainers.SortedList` | O(log n) search; SortedList O(log n) insert |
| Prefix search, autocomplete | Trie | O(length of prefix) |
| Dynamic connectivity, grouping | Union-find | near O(1) union/find |
| Dependencies, ordering | Adjacency list + indegree map | O(V+E) topological sort |
| Counting | `Counter` | readable, `most_common` built in |
| Fixed time windows | Circular buffer of buckets | constant memory |
| Probabilistic membership | Bloom filter | tiny memory, false positives only |
| Dense numeric vectors | NumPy array | vectorised, cache-friendly |

Example: "For the LRU cache I need O(1) get, O(1) update of recency, and O(1) eviction of the oldest. A dict alone has no order; a list has O(n) removal; a hash map pointing into a doubly linked list gives all three, and `OrderedDict` is exactly that in Python."

Also mention practical factors: simplicity and readability, memory overhead (Python objects are large; arrays or NumPy for millions of numbers), and what the standard library provides so you do not hand-roll it in an interview unless asked.

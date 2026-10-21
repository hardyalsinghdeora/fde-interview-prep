# 05 — Learning Round: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts. The primers give you a head start so the round feels familiar, but the round scores *how you learn*, so still ask questions as if new. Adapt the behavioral templates to your own experience and practise saying everything out loud.

**The universal learning-round loop (use it for every topic below):**
1. **Frame** — "Before we start: what's the goal at the end of the hour? What will I be asked to build?"
2. **Anchor** — relate it to something you know: "Is this like X, or is the key difference Y?"
3. **Play back** — every 5–10 minutes: "So if I understand correctly, ... Is that right?"
4. **Probe edges** — "What happens when ...fails / is empty / is concurrent?"
5. **Apply** — talk through a small example by hand before writing code.
6. **Admit gaps** — "I don't know that yet; my guess is ..., can you confirm?" Guessing confidently and wrong is worse than asking.

If you already know the topic, say so briefly ("I've used Raft a bit, so I'll check my understanding against yours") and still play back — they are scoring the process.

## Possible topics you could be taught

### A new programming language feature or an unfamiliar language (Rust ownership, Go channels, Haskell basics)

**Rust ownership (primer)**
- Every value has exactly **one owner**; when the owner goes out of scope the value is dropped (freed). No GC, no manual `free`.
- Assigning or passing a heap value **moves** it: `let b = a;` makes `a` unusable. Types that are `Copy` (ints, bools) are copied instead.
- **Borrowing**: `&T` is a shared (read-only) reference, `&mut T` is an exclusive mutable reference. Rule: *any number of `&T` OR exactly one `&mut T`*, never both at once. This rule is what prevents data races at compile time.
- **Lifetimes** (`'a`) tell the compiler how long a reference is valid so you can never return a reference to freed memory.
- Errors: no exceptions; `Result<T, E>` and `Option<T>`, with `?` to propagate.

**Go channels (primer)**
- Goroutines are cheap green threads (`go f()`). Channels are typed pipes: `ch := make(chan int)` (unbuffered: send blocks until a receiver is ready) or `make(chan int, 10)` (buffered).
- `ch <- v` sends, `v := <-ch` receives, `close(ch)` signals no more values; `for v := range ch` reads until closed.
- `select` waits on several channel operations at once (use with `time.After` for timeouts).
- Motto: "Don't communicate by sharing memory; share memory by communicating." Classic patterns: worker pool, fan-out/fan-in, pipeline. `sync.WaitGroup` waits for goroutines.

**Haskell basics (primer)**
- Pure functions, immutable data, lazy evaluation. Side effects live in the `IO` type.
- Strong static types with inference; type signatures like `map :: (a -> b) -> [a] -> [b]`.
- Pattern matching and recursion instead of loops; algebraic data types (`data Shape = Circle Double | Rect Double Double`); `Maybe`/`Either` for missing values/errors.
- Type classes (`Eq`, `Show`, `Functor`, `Monad`) are like interfaces.

**How to act:** map each concept to one you know ("so a `&mut` borrow is like holding a write lock, checked at compile time?"), write tiny snippets and ask "would this compile?", and deliberately test the rule you just learned.

### An unfamiliar API / SDK (read the docs, then call it)

**What they watch:** do you read before you flail, and do you find the right part of the docs fast?

**Approach:**
1. Find the **quickstart** and the **auth** section first; get one "hello world" call working end to end.
2. Identify the **core nouns** (resources/objects) and **verbs** (endpoints/methods). Sketch the object model.
3. Check the boring-but-critical bits: pagination, rate limits, error format, idempotency, sync vs async, versioning.
4. Read one real response and print it before writing parsing code.
5. Wrap the API in a small client function so the rest of your code doesn't depend on its quirks.

**Questions to ask:** "Is there a sandbox key?", "Which call is the one I'll use most?", "What does a typical error look like?", "Are there gotchas you've seen people hit?"

### A new data structure (bloom filter, skip list, segment tree, trie, HyperLogLog, CRDT)

- **Bloom filter** — probabilistic set membership. A bit array of `m` bits plus `k` hash functions. Insert: set the `k` bits. Query: if any bit is 0 → *definitely not present*; all 1 → *probably present*. **No false negatives, some false positives**; can't delete (use a counting Bloom filter). False-positive rate ≈ (1 − e^(−kn/m))^k; optimal k ≈ (m/n)·ln 2. Used in databases (skip disk reads in LSM trees), caches, crawlers.
- **Skip list** — sorted linked list with extra "express lane" levels; each node is promoted to the next level with probability 1/2. Search/insert/delete expected **O(log n)**. Simpler than balanced trees and concurrency-friendly (used in Redis sorted sets, LevelDB/RocksDB memtables).
- **Segment tree** — binary tree over an array where each node stores an aggregate (sum/min/max) of a range. **O(log n) range query and point update**, O(n) build and space. With lazy propagation, O(log n) range updates too. (Fenwick tree is a lighter alternative for prefix sums.)
- **Trie (prefix tree)** — each edge is a character; paths spell keys. Insert/lookup **O(L)** in key length; great for autocomplete, prefix search, spell-check, IP routing (radix trie). Cost: memory.
- **HyperLogLog** — estimates the **count of distinct items** in tiny fixed memory (~1.5 KB for ~2% error). Hash each item; use the first bits to pick a register, record the max number of leading zeros seen; combine registers with a harmonic mean. Mergeable across shards (union = register-wise max). Used in Redis `PFCOUNT`, BigQuery/Presto `APPROX_COUNT_DISTINCT`.
- **CRDT (Conflict-free Replicated Data Type)** — data structures that replicas can update independently and merge without coordination, always converging. Merge must be commutative, associative and idempotent. Examples: G-Counter (per-replica counters, merge = element-wise max, value = sum), PN-Counter, OR-Set, LWW-Register. Used in collaborative editing and multi-region, offline-first apps.

**How to act:** ask "what operation is this optimised for, and what does it give up?" — every one of these trades exactness, memory or flexibility for speed. Work an example by hand (insert 3 items into a tiny Bloom filter) before coding.

### An algorithm (consistent hashing, MapReduce, Raft/Paxos basics, PageRank)

- **Consistent hashing** — place nodes and keys on a hash ring; a key belongs to the first node clockwise. Adding/removing a node moves only ~**1/N of keys** (vs nearly all with `hash % N`). **Virtual nodes** (many points per server) smooth out load and allow weighting. Used in Dynamo, Cassandra, CDNs, sharded caches. Related: rendezvous (highest-random-weight) hashing.
- **MapReduce** — **map** each input record to (key, value) pairs → **shuffle** groups by key across machines → **reduce** each key's values to an output. Fault tolerance by re-running failed tasks (functions must be deterministic). Optional **combiner** does a local pre-reduce to cut network traffic. Classic example: word count. Spark generalised it with in-memory DAGs.
- **Raft basics** — consensus for a replicated log. Nodes are **follower, candidate or leader**. Leader election: followers time out (randomised), become candidates, increment the **term**, request votes; a majority wins. The leader appends client commands to its log and replicates them via `AppendEntries`; an entry is **committed** once stored on a majority, then applied to the state machine. Safety: only a node with an up-to-date log can win an election. Tolerates **f failures with 2f+1 nodes**. Used in etcd, Consul, CockroachDB.
- **Paxos basics** — same goal, older and harder to follow: proposers send numbered **prepare** requests, acceptors promise not to accept lower numbers, then **accept** a value; a value chosen by a majority is decided. Multi-Paxos with a stable leader looks a lot like Raft.
- **PageRank** — a page's importance is the probability a "random surfer" lands on it. PR(p) = (1−d)/N + d · Σ PR(q)/outdeg(q) over pages q linking to p; damping d ≈ 0.85. Computed by **power iteration** until convergence; handle dangling nodes by spreading their rank evenly. Applies to any graph centrality problem (fraud rings, influence, citation networks).

**How to act:** draw it. A ring, a shuffle diagram, a 3-node cluster with a timeline. Then ask about failure: "What if the leader crashes after replicating to one follower?"

### A domain concept (supply chain, clinical trials, double-entry bookkeeping, options pricing)

- **Supply chain** — flow of goods from suppliers → manufacturing → distribution centres → stores/customers. Key ideas: **lead time**, **safety stock**, **reorder point** (≈ demand during lead time + safety stock), **bill of materials (BOM)**, inventory turns, fill rate, the **bullwhip effect** (small demand changes amplify upstream). Data: ERP (SAP), WMS, TMS, POS. Typical problems: forecasting, stockouts vs overstock, supplier risk.
- **Clinical trials** — phases: **Phase I** (safety, dose, small group), **Phase II** (efficacy, side effects), **Phase III** (large randomised controlled trial vs standard of care, basis for approval), **Phase IV** (post-market surveillance). Concepts: randomisation, blinding (double-blind), placebo/control arm, primary endpoint, inclusion/exclusion criteria, adverse events, site enrolment. Regulators: FDA (US), EMA (EU); data standards CDISC (SDTM/ADaM); GCP compliance, 21 CFR Part 11 for electronic records.
- **Double-entry bookkeeping** — every transaction touches **at least two accounts** and **total debits = total credits**. Accounting equation: **Assets = Liabilities + Equity**. Debits increase assets/expenses; credits increase liabilities, equity and revenue. Example: sell goods for $100 cash → debit Cash 100, credit Revenue 100. The **general ledger** holds all accounts; a **trial balance** checks debits equal credits. Engineering lessons: ledgers are append-only (fix errors with reversing entries), use integer minor units or decimals not floats, and make postings atomic and idempotent.
- **Options pricing** — a **call** is the right (not obligation) to buy at the **strike** price by expiry; a **put** is the right to sell. Value = intrinsic value + time value. Price rises with volatility and (usually) time to expiry. **Black–Scholes** prices European options from spot, strike, time, risk-free rate and volatility (assumes lognormal prices, constant volatility). **Greeks**: delta (sensitivity to price), gamma, theta (time decay), vega (volatility), rho. Binomial trees handle American options. **Implied volatility** is the volatility that makes the model price equal the market price.

**How to act:** ask for the *workflow and the people*: "Who does this day to day, what decision do they make, and what goes wrong?" Then ask for the key nouns, and map them to a data model as you go — this is exactly what a decomposition interviewer wants to see later.

### A company's own tool/DSL (e.g., an ontology model, a pipeline builder)

**Primer (Palantir-style ontology):** an ontology is a semantic layer over raw data: **object types** (e.g., Aircraft, Flight, Part) backed by datasets, **properties** (columns), **link types** (relationships, e.g., Flight → Aircraft), and **actions** (governed write-backs, e.g., "Ground aircraft", with validation and permissions). Applications and AI agents work against objects, not tables. A **pipeline builder** is a visual or code DAG of transforms: sources → joins/cleaning → outputs, with scheduling, lineage and data checks.

**How to act:**
- Ask for the **primitives** first ("What are the 4–5 building blocks?") and the **one example** that uses all of them.
- Map to known concepts ("object type ≈ table with a primary key; link ≈ foreign key; action ≈ a validated stored procedure?") and ask where the analogy breaks.
- Ask about **constraints**: versioning, permissions, how changes propagate, what happens on schema change.
- When applying it, model the problem out loud in their terms ("I'd create a Patient object type, an Admission object linked to it...").

### A regex / query language (GraphQL, Cypher, jq)

- **GraphQL** — one endpoint; client asks for exactly the fields it needs. Schema defines **types**; operations are **queries** (read), **mutations** (write), **subscriptions** (push). Example: `query { user(id: "1") { name orders(first: 5) { id total } } }`. Watch for N+1 resolver problems (fix with DataLoader batching), query depth/cost limits, and pagination (cursor "connections").
- **Cypher (Neo4j)** — pattern-matching graph queries using ASCII art: `MATCH (p:Person)-[:WORKS_AT]->(c:Company {name: "Acme"}) RETURN p.name`. Variable-length paths: `-[:KNOWS*1..3]->`. `CREATE`/`MERGE` to write; `shortestPath(...)` for paths.
- **jq** — a filter language for JSON: `.` is the input, `.items[]` iterates, `|` pipes, `select(.status == "open")` filters, `map(.price) | add` aggregates, `{id, name}` builds objects. Example: `jq '.items[] | select(.qty > 0) | {sku, qty}' data.json`.
- **Regex refresher** — `.` any char, `*` / `+` / `?` quantifiers, `[a-z]` classes, `^` `$` anchors, `\d \w \s`, `( )` groups, `(?: )` non-capturing, `*?` lazy. Beware catastrophic backtracking on nested quantifiers.

**How to act:** build queries incrementally — start with the simplest query that returns *something*, run it, then add one clause at a time. Say what you expect the output to be before you run it.

### A small unfamiliar codebase — add a feature to it

**Approach (narrate this):**
1. **Run it first** — install, run tests, run the app. Know what "working" looks like.
2. **Read the README, entry point and directory layout**; find where requests/data enter and leave.
3. **Trace one existing feature** that resembles the one you're adding, end to end. Copy its pattern.
4. **Find the seam** — the smallest place to add your change. Match existing style, naming and error handling.
5. **Write or extend a test first** (or at least reproduce the behavior manually).
6. Make the change in small steps; run tests after each.
7. Mention what you'd improve but didn't ("this function is doing three things; I'd refactor it after, not during").

**Questions to ask:** "Is there a part of the code you'd consider the core?", "Any conventions I should follow?", "Is this test suite trustworthy?"

### Git internals; Docker/Kubernetes basics

**Git internals**
- Git is a **content-addressed object store** (`.git/objects`), keyed by SHA-1 (SHA-256 optional) hashes. Four object types: **blob** (file contents), **tree** (directory listing → blobs/trees), **commit** (tree + parent(s) + author + message), **tag**.
- **Branches are just files containing a commit hash** (`.git/refs/heads/main`); `HEAD` points to the current branch. That's why branching is cheap.
- The **index** (staging area) is the proposed next tree.
- **Merge** creates a commit with two parents; **rebase** replays commits onto a new base (new hashes, rewrites history). `reflog` records where refs pointed — your undo button.

**Docker basics**
- An **image** is a layered, read-only filesystem built from a `Dockerfile`; a **container** is a running instance with its own process, network and filesystem namespaces (Linux namespaces + cgroups, not a VM).
- Layers are cached — put rarely-changing steps (dependency install) before frequently-changing ones (copy source). Use multi-stage builds and small base images.

**Kubernetes basics**
- **Pod** (one or more containers sharing network/storage) → **Deployment** (desired replica count, rolling updates) → **Service** (stable virtual IP / DNS for a set of pods) → **Ingress** (HTTP routing from outside).
- **ConfigMap / Secret** for config, **PersistentVolume** for storage, **HPA** for autoscaling, **Namespace** for isolation. Control plane (API server, etcd, scheduler, controllers) continually reconciles actual state to the desired state you declare in YAML.

### How transformers / embeddings work (for non-ML candidates)

- **Tokens** — text is split into sub-word tokens; each token becomes a vector (**embedding**).
- **Embeddings** — vectors where similar meanings are close together (measured by cosine similarity). That is what makes semantic search and RAG work: embed documents and the query, return the nearest vectors.
- **Self-attention** — for each token, the model computes **query, key and value** vectors; attention weights = softmax(Q·Kᵀ/√d); output = weighted sum of values. Each token "looks at" the other tokens that matter for its meaning. **Multi-head** attention does this several ways in parallel.
- **Transformer block** = attention + feed-forward network, with residual connections and layer norm; stack dozens of blocks. **Positional encodings** add word-order information.
- **Decoder-only LLMs** (GPT, Claude) are trained to predict the next token with a causal mask; generation repeats that one token at a time. Pre-training on large text corpora, then instruction tuning and RLHF/RLAIF to make them helpful and safe.
- **Practical consequences:** context window limits, cost scales with tokens, models can hallucinate, temperature controls randomness.

Deeper version: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### A game's rules, then write code to play/score it

**Approach:**
1. **Restate the rules** and ask about edge cases before coding: ties, invalid moves, end conditions, scoring of special cases (think bowling strikes/spares, poker hand ranking, tic-tac-toe draws).
2. **Model the state** explicitly: a `GameState` (board, players, turn, score) and pure functions `legal_moves(state)`, `apply(state, move)`, `is_over(state)`, `score(state)`.
3. **Work one example by hand**, then turn it into a test.
4. Build incrementally: scoring first, then validation, then a simple player (random or greedy), then search (minimax with alpha-beta) if asked.

Example skeleton:

```python
from dataclasses import dataclass, replace

@dataclass(frozen=True)
class State:
    board: tuple          # immutable so states are easy to copy/compare
    player: int           # whose turn: 0 or 1

def legal_moves(s: State) -> list[int]:
    return [i for i, c in enumerate(s.board) if c is None]

def apply(s: State, move: int) -> State:
    b = list(s.board); b[move] = s.player
    return replace(s, board=tuple(b), player=1 - s.player)
```

Immutable state makes testing and search simple.

## Questions they might ask

### Explain what you just learned back to me in your own words.

**What they want:** proof you built a correct mental model, not memorised phrases.

**Structure (≈60–90 seconds):**
1. **Purpose** — "This exists to solve X problem."
2. **Core idea** — the one mechanism that makes it work.
3. **How it works** — 2–4 steps, with a tiny example.
4. **Trade-off** — what it gives up.
5. **Check** — "Did I miss or mis-state anything?"

**Example (consistent hashing):** "The problem is that with `hash % N`, adding a server reshuffles almost every key. Consistent hashing puts servers and keys on a ring, and a key goes to the next server clockwise. Adding a server only takes keys from its neighbour, so about 1/N of keys move. Virtual nodes spread each server around the ring to balance load. The trade-off is a bit more bookkeeping and lookups need a sorted structure. Is that accurate?"

### How would you explain it to a non-technical customer?

- Lead with **why they care** (outcome), not how it works.
- Use **one analogy** from their world; drop jargon or define it in one phrase.
- Give the **limitation** honestly, in business terms.
- Check understanding: "Does that match what you were picturing?"

**Example (Bloom filter):** "It's like a bouncer with a very fast but imperfect guest list. If he says you're *not* on the list, you're definitely not. If he says you might be, we double-check the full list. It saves us checking the slow list for most people, and occasionally we do one extra check."

**Example (RAG / embeddings):** "Before the AI answers, we look up the most relevant pages from your own documents and hand them to it, like giving a new employee the right binder before they answer a customer. It answers from your content and can show where the answer came from."

### Now apply it: solve this problem using the concept.

**Approach:**
1. Restate the problem and **say which part of the concept applies** ("this is a range-sum query with updates, so a segment tree fits").
2. **Work a small example by hand** first.
3. Write the simplest correct version; state complexity.
4. Test edge cases out loud; then optimise or extend.
5. Keep using the interviewer: "Is it OK to assume X?" "Am I using this correctly?"

Example: applying a Bloom filter to "don't recommend articles a user has already seen":

```python
import hashlib

class BloomFilter:
    def __init__(self, m: int, k: int):
        self.m, self.k, self.bits = m, k, bytearray(m)

    def _idx(self, item: str):
        for i in range(self.k):
            h = hashlib.sha256(f"{i}:{item}".encode()).digest()
            yield int.from_bytes(h[:8], "big") % self.m

    def add(self, item: str) -> None:
        for i in self._idx(item):
            self.bits[i] = 1

    def __contains__(self, item: str) -> bool:
        return all(self.bits[i] for i in self._idx(item))

seen = BloomFilter(m=10_000, k=7)
seen.add("article-42")
print("article-42" in seen, "article-7" in seen)  # True, (almost surely) False
```

Time O(k) per add/query; space O(m) bits (bytearray used here for clarity; a bitarray uses m/8 bytes). A false positive only means we skip an unseen article occasionally — acceptable here; it would not be for, say, fraud blocking.

### What are the limitations/trade-offs of this approach?

**Framework — go through these lenses quickly:**
- **Correctness:** approximate? eventual consistency? edge cases where it fails?
- **Performance:** time/space, worst case vs average, hot spots.
- **Operational:** complexity to run, debug, monitor; failure modes.
- **Scale:** what breaks at 10x/100x?
- **Alternatives:** "When would I pick X instead?"

**Examples:**
- Bloom filter: false positives, can't delete, must size for expected n up front.
- Raft: needs a majority (unavailable during partitions for the minority side), leader is a throughput bottleneck, latency of a round trip per commit.
- Consistent hashing: uneven load without virtual nodes; hot keys still overload one node.
- MapReduce: high latency, disk-heavy shuffle, awkward for iterative algorithms.
- RAG with embeddings: misses exact-match terms (IDs, codes) — combine with keyword search; quality depends on chunking.

### Where else could this be useful?

**Pattern:** name the *abstract property* the concept gives you, then list places that need that property. Aim for 2–3 examples, at least one from the company's customer domain.

**Examples:**
- **Bloom filter** (cheap "definitely not" check): LSM-tree databases skipping SSTables, web crawlers avoiding revisits, CDN cache "one-hit wonder" filtering, checking leaked passwords locally.
- **Consistent hashing** (stable assignment as membership changes): sharded caches, load balancers with session affinity, distributing work to stream consumers.
- **PageRank** (importance in a graph): fraud ring detection, finding key suppliers in a supply network, ranking documents in enterprise search, identifying influential entities in an intelligence graph.
- **Double-entry** (invariant that every change balances): inventory movements, credits/usage billing, any audit-critical ledger.

### What questions do you still have?

Always have 2–3 real ones; "none" signals low curiosity. Good categories:
- **Edge/failure cases:** "What happens in Raft if two candidates tie repeatedly?"
- **Real-world usage:** "Where does this break down in practice at your company?"
- **Design rationale:** "Why was it designed this way rather than X?"
- **Next level:** "What would an expert know that I don't yet?"
- **Confirming a fuzzy point:** "I'm still not 100% sure about Y — can I check my understanding?"

### How would you learn this more deeply if you had a week?

A concrete plan beats "I'd read more":
- **Day 1 — Primary source:** the original paper or official docs (e.g., the Raft paper, the Rust Book chapters 4 and 10).
- **Day 2–3 — Build it:** implement a minimal version from scratch (a toy Raft, a Bloom filter with measured false-positive rate).
- **Day 4 — Read production code:** see how a real system does it (etcd's Raft, RocksDB Bloom filters, Redis HyperLogLog).
- **Day 5 — Break it:** test failure cases, benchmark, compare to alternatives.
- **Day 6 — Apply it** to a real problem I care about.
- **Day 7 — Teach it:** write a one-page explainer or explain to a colleague; the gaps show up immediately.
Also: find an expert to ask the questions I couldn't answer myself.

## Behavioral questions about learning

### Tell me about a time you had to learn something new very quickly.

**What they're looking for:** a repeatable method, speed with judgement, using people as resources, and a real outcome.

**STAR template:**
- **S:** [Context: new tech/domain, why it was urgent — customer deadline, teammate left, incident].
- **T:** [What you had to deliver, by when].
- **A:** [How you learned: scoped what you actually needed; primary docs; built a tiny prototype; found an expert and asked targeted questions; checked understanding; shipped incrementally].
- **R:** [Outcome with numbers; what you'd do differently; how you now learn fast].

**Example (adapt to your own experience):** "A customer's integration needed a Kafka consumer and I'd never used Kafka; we had a week. I spent the first afternoon on the official quickstart and getting a local broker running, then wrote down the five concepts I needed: topics, partitions, consumer groups, offsets, delivery semantics. I booked 30 minutes with our platform engineer and asked specifically about offset commits and rebalancing, which is where I suspected bugs would be. I built the consumer with idempotent writes so at-least-once delivery was safe. We shipped in five days and it processed about 2M events a day without data loss. The lesson I kept: learn the 20% you need, find the sharp edges from someone experienced, and design defensively around what you don't yet know."

**Pitfalls:** vague "I just read a lot"; no outcome; claiming mastery in days; not mentioning anyone who helped.

### How do you approach learning a new codebase?

- **Get it running** locally and run the tests; know what "healthy" looks like.
- **Top-down map:** README, architecture docs, directory structure, entry points, main data models, deploy config.
- **Follow one request or job end to end** with a debugger or logs — the fastest way to see how layers connect.
- **Read the data model / schema** — it tells you what the system is actually about.
- **Read recent PRs and git history** for the area I'll touch (`git log -p`, `git blame`) to learn conventions and why things are odd.
- **Make a small, real change early** (a bug fix, a test) to learn the build, review and deploy loop.
- **Keep notes / a diagram** and ask a long-tenured engineer to correct it.
- Hold opinions back until I understand why things are the way they are (Chesterton's fence).

### How do you approach learning a new business domain?

- **Start with people and decisions:** who are the users, what decisions do they make, what does a bad day look like? Shadow them if possible — sit with a dispatcher, an analyst, a nurse.
- **Learn the vocabulary:** build a glossary; domains are full of overloaded terms ("account", "order", "encounter").
- **Follow the money and the workflow:** how value flows and where handoffs happen; that's where problems live.
- **Learn the data:** what systems exist (ERP, EHR, CRM), what the key entities and IDs are, where data is wrong or late.
- **Learn the constraints:** regulation (HIPAA, SOX, ITAR), safety, unions, legacy contracts.
- **Read what practitioners read:** industry primers, trade publications, internal training material.
- **Play it back** to a domain expert: "Here's how I think a claim gets paid — where am I wrong?"
- Find the **metric they're judged on**; that anchors everything I build.

### What's the last technical thing you learned on your own?

**What they want:** genuine curiosity, depth (not just a buzzword), and that you build things.

**Template:** "Recently I learned [topic] because [real reason — problem or curiosity]. I [read X / built Y]. The thing that surprised me was [specific insight]. I've since used it to [application] / next I want to [follow-up]."

**Example (adapt to your own experience):** "I learned how MCP (Model Context Protocol) servers work because I wanted Claude to query my own Postgres database. I read the spec, built a small server exposing two tools and a resource, and connected it to a client. What surprised me was how much quality depended on tool descriptions and returning concise, structured results — the model chose tools much better once I wrote descriptions like API docs for a new hire. Next I want to add auth and per-user permissions."

**Pitfalls:** something you can't go one level deep on; something years old; no hands-on element.

### How do you stay up to date with technology?

Be specific and show a filter, not a firehose:
- **Primary sources:** release notes and engineering blogs of tools I use (e.g., Postgres, Kubernetes, OpenAI/Anthropic docs and changelogs), key papers.
- **Curated feeds:** a few newsletters/podcasts [name the ones you actually use], Hacker News for scanning.
- **Hands-on:** I build a small project with anything I think I'll use — reading isn't learning until I've run it.
- **People:** colleagues, meetups, open-source issue threads.
- **Filter:** I go deep only on things relevant to my work or customers; everything else gets a skim so I know it exists.
Close with a recent concrete example of something you tried.

### Tell me about a time you were the least experienced person in the room.

**What they're looking for:** humility plus contribution — you listened, asked good questions, and still added value (often a fresh perspective, execution or synthesis).

**STAR template:**
- **S:** [Meeting/project with senior experts — domain specialists, executives, staff engineers].
- **T:** [Your role and what was expected].
- **A:** [Prepared beforehand; listened more than talked; asked clarifying questions others were afraid to; contributed where you had an edge — a prototype, the data, a summary; followed up to learn].
- **R:** [Outcome; what you learned; relationships built].

**Example (adapt to your own experience):** "In my first month I joined a design review with three staff engineers and a client's head of operations. I read the design doc and the client's last three tickets the night before. During the meeting I mostly listened, but I asked one basic question — how often the upstream file actually arrived late — which nobody had data on. I pulled the logs that afternoon: it was late 30% of the time, which changed the design from a nightly batch to an event-driven trigger. I learned that a beginner's question backed by data can be the most useful thing in the room."

**Pitfalls:** staying silent and contributing nothing; pretending to be an expert; framing it as "they were wrong and I was right".

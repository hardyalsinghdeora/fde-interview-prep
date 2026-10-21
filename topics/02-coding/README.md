# 02 — Coding Questions

FDE coding rounds come in three flavours:
1. **Classic DS&A** (LeetCode easy–medium, sometimes hard). Common at Palantir, Scale, Databricks.
2. **Practical / incremental**: one problem that grows in stages as requirements change (Anthropic, OpenAI, Ramp, CodeSignal "progressive" format).
3. **Applied / API coding**: call an API, parse JSON, handle errors and pagination, build a small service.

Languages: Python is the safe default. Know one of Java/TypeScript/Go as well.

---

## A. Reported FDE coding problems
- [ ] Check whether a string is a palindrome (and variants: ignore punctuation, valid palindrome II).
- [ ] Find a path on a grid where 1s are walls (BFS/DFS shortest path; with k wall removals).
- [ ] Find time intervals where multiple people are all free (Scale AI OA, "employee free time").
- [ ] Rate limiter supporting per-user **and** global limits (token bucket / sliding window).
- [ ] LRU cache (and LFU cache, TTL cache).
- [ ] In-memory key-value store with transactions (`BEGIN`, `ROLLBACK`, `COMMIT`) and TTLs.
- [ ] In-memory database / bank system with progressive levels (deposit, transfer, top spenders, scheduled payments, merge accounts).
- [ ] File system: `mkdir`, `ls`, `addContentToFile`, `readContentFromFile`.
- [ ] Web crawler (single-threaded → multi-threaded, same domain only).
- [ ] Parse a log file and return top-K errors / endpoints / users.
- [ ] Stack-trace / log deduplication.
- [ ] Design and implement a task scheduler with dependencies (topological sort).
- [ ] Merge overlapping intervals; insert interval; meeting rooms I & II.
- [ ] Two-sum / three-sum variants.
- [ ] Group anagrams.
- [ ] Top-K frequent elements / words.
- [ ] Number of islands.
- [ ] Word search / word ladder.
- [ ] Course schedule (cycle detection).
- [ ] Implement a tokenizer / simple expression evaluator (basic calculator I/II).
- [ ] Flatten nested JSON / nested list iterator.
- [ ] Diff two JSON objects / two lists of records.
- [ ] Implement `retry with exponential backoff and jitter`.
- [ ] Implement a simple pub/sub or event bus.
- [ ] Paginate through an API and aggregate results.
- [ ] Batch requests: given a stream of items, send in batches of N or every T ms.
- [ ] Deduplicate records with fuzzy matching (entity resolution).
- [ ] Implement a cosine-similarity search over embeddings (brute force, then top-k with heap).
- [ ] Chunk a document for RAG with overlap; respect sentence boundaries.
- [ ] Stream tokens from an LLM API and handle partial JSON.
- [ ] Parse and validate tool-call JSON from an LLM; repair malformed JSON.
- [ ] Count tokens / truncate conversation history to fit a context window.
- [ ] Simple in-memory vector store with add/search/delete.
- [ ] Concurrent requests with a max concurrency limit (semaphore / async).
- [ ] Circuit breaker implementation.
- [ ] Debounce / throttle function (JS).
- [ ] Unix `tail -f` / `grep` style utility.
- [ ] CSV ingestion with bad rows: parse, validate, report errors.

## B. Topic checklist (classic DS&A)

### Arrays & strings
- [ ] Two pointers (container with most water, remove duplicates, trapping rain water)
- [ ] Sliding window (longest substring without repeating characters, minimum window substring)
- [ ] Prefix sums (subarray sum equals k, range sum query)
- [ ] Kadane's (maximum subarray)
- [ ] String parsing (decode string, string compression, valid parentheses)
- [ ] Rotate matrix, spiral matrix, set matrix zeroes
- [ ] Product of array except self
- [ ] Longest common prefix; longest palindromic substring

### Hashing
- [ ] Frequency maps, two-sum, isomorphic strings
- [ ] LRU with hash map + doubly linked list
- [ ] Design a hash map from scratch

### Linked lists
- [ ] Reverse a linked list (iterative/recursive, in k-groups)
- [ ] Detect cycle; find cycle start
- [ ] Merge two / k sorted lists
- [ ] Remove nth node from end; copy list with random pointer

### Stacks & queues
- [ ] Valid parentheses; min stack
- [ ] Monotonic stack (daily temperatures, next greater element, largest rectangle in histogram)
- [ ] Queue using stacks; sliding-window maximum (deque)

### Trees
- [ ] Traversals (in/pre/post/level order), iterative and recursive
- [ ] Max depth, diameter, balanced check
- [ ] Validate BST; kth smallest in BST
- [ ] Lowest common ancestor
- [ ] Serialize/deserialize a binary tree
- [ ] Path sum variants
- [ ] Trie: implement, word search II, autocomplete

### Heaps
- [ ] Kth largest; top-K frequent; merge k sorted
- [ ] Median from data stream
- [ ] Task scheduler / reorganize string

### Graphs
- [ ] BFS/DFS on grids (islands, rotting oranges, walls & gates)
- [ ] Shortest path (BFS unweighted, Dijkstra weighted)
- [ ] Topological sort (course schedule, build order, alien dictionary)
- [ ] Union-find (number of connected components, redundant connection, accounts merge)
- [ ] Clone graph; bipartite check
- [ ] Minimum spanning tree (Kruskal/Prim) — less common
- [ ] Network delay time; cheapest flights within k stops

### Intervals
- [ ] Merge intervals, insert interval, non-overlapping intervals
- [ ] Meeting rooms II (min rooms), employee free time

### Binary search
- [ ] Classic, first/last position, search in rotated array
- [ ] Binary search on the answer (koko eating bananas, capacity to ship packages)
- [ ] Find peak element; median of two sorted arrays (hard)

### Recursion & backtracking
- [ ] Subsets, permutations, combinations, combination sum
- [ ] N-Queens, Sudoku solver
- [ ] Generate parentheses; letter combinations of a phone number

### Dynamic programming
- [ ] Climbing stairs, house robber, coin change
- [ ] Longest increasing subsequence
- [ ] Longest common subsequence, edit distance
- [ ] Unique paths, minimum path sum
- [ ] Word break; decode ways
- [ ] 0/1 knapsack, partition equal subset sum

### Bit manipulation & math
- [ ] Single number, counting bits, power of two
- [ ] GCD/LCM, prime sieve, fast exponentiation
- [ ] Reservoir sampling; random pick with weight

## C. Object-oriented / low-level design (code it)
- [ ] Parking lot
- [ ] Elevator system
- [ ] Library management system
- [ ] Vending machine
- [ ] Chess / tic-tac-toe game
- [ ] Snake game
- [ ] Spreadsheet with formula cells (dependency graph, cycle detection)
- [ ] Inventory management system
- [ ] Hotel / restaurant reservation system
- [ ] Ride-sharing matching (simplified)
- [ ] Logger with rate limiting ("print message only if not printed in last 10s")
- [ ] Hit counter (hits in last 5 minutes)
- [ ] Event-driven notification system
- [ ] Simple workflow/state machine engine (e.g., order lifecycle)

## D. Questions asked during/after coding
- [ ] What's the time and space complexity?
- [ ] How would you test this? Write the tests.
- [ ] What edge cases did you consider (empty, nulls, duplicates, huge inputs, unicode)?
- [ ] How would this change if the data didn't fit in memory?
- [ ] How would you make this thread-safe?
- [ ] How would this work across multiple servers (distributed rate limiter, distributed cache)?
- [ ] Now the requirement changed to X — how do you extend your code?
- [ ] How would you productionize this (logging, metrics, config, error handling)?
- [ ] Can you refactor this to be more readable?
- [ ] Why did you pick this data structure?

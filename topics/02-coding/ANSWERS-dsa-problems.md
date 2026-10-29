# 02 — Coding: Standard Problem List (NeetCode 150 Gaps): Model Answers

Questions: [README.md](README.md#b2-standard-problem-list-neetcode-150-gaps)

**How to use these answers:** these are reference solutions, not scripts to memorise. Together with section B ([ANSWERS-dsa-topics.md](ANSWERS-dsa-topics.md)) they cover the NeetCode 150 / Blind 75 lists. For each problem, name the pattern first, then practise saying the approach, complexity and edge cases out loud before you type. Every code block runs standalone and ends with asserts on LeetCode's example inputs, so you can paste it into a file and check your own variant against it.

---

## Foundations

### Big-O cheat sheet (common Python operations and data structures)

**Pattern:** know the cost of the built-ins you lean on, because the hidden O(n) inside a loop is the most common reason a "linear" solution is really quadratic.

| Structure | Operation | Cost |
|---|---|---|
| `list` | index, assign, `append`, `pop()` | O(1) (append amortised) |
| `list` | `pop(0)`, `insert(0, x)`, `x in lst`, `remove`, slice `a[i:j]` | O(n) / O(j − i) |
| `list` | `sort()`, `sorted()` | O(n log n), stable (Timsort) |
| `dict` / `set` | get, set, `in`, delete | O(1) average, O(n) worst |
| `collections.deque` | `append`, `appendleft`, `pop`, `popleft` | O(1); indexing the middle is O(n) |
| `heapq` | `heappush`, `heappop` / `heapify` / `h[0]` | O(log n) / O(n) / O(1) |
| `heapq` | `nlargest(k, xs)`, `nsmallest` | O(n log k) |
| `bisect` | `bisect_left` / `insort` | O(log n) / O(n) (shifting) |
| `str` | `s + t` in a loop / `"".join(parts)` | O(n²) total / O(n) |
| `Counter` | build / `most_common(k)` | O(n) / O(n log k) |

Input size → target complexity: n ≤ 12 → O(n!); n ≤ 25 → O(2ⁿ); n ≤ 500 → O(n³); n ≤ 10⁴ → O(n²); n ≤ 10⁶ → O(n log n); larger → O(n) or O(log n).

```python
from collections import deque
import bisect

q = deque([1, 2, 3]); q.append(4)
assert q.popleft() == 1                 # O(1); list.pop(0) would be O(n)

parts = [str(i) for i in range(5)]
assert "".join(parts) == "01234"        # O(n); += in a loop is O(n^2) in the worst case

a = [1, 3, 5, 7]
assert bisect.bisect_left(a, 5) == 2    # O(log n) search
bisect.insort(a, 4); assert a == [1, 3, 4, 5, 7]   # O(n): elements shift

seen = set(a)
assert 5 in seen                        # O(1) average, vs O(n) for 5 in a
```

- Space counts too: recursion depth (DFS on a 10⁵-node path overflows Python's default limit of 1000), slices create copies, `sorted()` allocates a new list.
- Say which case you mean: amortised (list append), average (hash map), worst (quicksort, hash collisions).

### Sorting algorithms (merge sort, quicksort, counting/bucket sort; when Timsort is enough)

**Pattern:** merge sort is divide and conquer with a linear merge (stable, O(n) extra space); quicksort partitions around a pivot in place (random pivot to avoid the O(n²) worst case); counting sort beats O(n log n) when keys are small integers.

```python
import random

def merge_sort(a):
    if len(a) <= 1: return a
    mid = len(a) // 2
    l, r = merge_sort(a[:mid]), merge_sort(a[mid:])
    out, i, j = [], 0, 0
    while i < len(l) and j < len(r):
        if l[i] <= r[j]: out.append(l[i]); i += 1     # <= keeps it stable
        else: out.append(r[j]); j += 1
    return out + l[i:] + r[j:]

def quicksort(a, lo=0, hi=None):        # in place, random pivot, Lomuto partition
    if hi is None: hi = len(a) - 1
    if lo >= hi: return a
    p = random.randint(lo, hi); a[p], a[hi] = a[hi], a[p]
    pivot, w = a[hi], lo
    for i in range(lo, hi):
        if a[i] < pivot: a[i], a[w] = a[w], a[i]; w += 1
    a[w], a[hi] = a[hi], a[w]           # pivot lands in its final position
    quicksort(a, lo, w - 1); quicksort(a, w + 1, hi)
    return a

def counting_sort(a, max_val):          # non-negative ints <= max_val
    counts = [0] * (max_val + 1)
    for x in a: counts[x] += 1
    return [v for v, c in enumerate(counts) for _ in range(c)]

random.seed(0)
for _ in range(100):
    xs = [random.randint(0, 50) for _ in range(random.randint(0, 30))]
    assert merge_sort(xs) == quicksort(xs[:]) == counting_sort(xs, 50) == sorted(xs)
```

- Merge sort: O(n log n) always, O(n) space. Quicksort: O(n log n) average, O(n²) worst, O(log n) stack. Counting sort: O(n + k). Bucket sort: O(n) average for uniformly spread floats.
- In an interview, use `sorted()` / `list.sort()` (Timsort: stable, O(n log n), O(n) on nearly sorted data, supports `key=`) unless the question is the sort itself, or the keys are bounded integers and O(n) matters.
- Lomuto partition degrades to O(n²) on many duplicates; mention 3-way (Dutch national flag) partitioning as the fix.

### Quickselect (kth element in average O(n))

**Pattern:** partition like quicksort, but recurse only into the side that contains the target index. Expected work is n + n/2 + n/4 + ... = O(n).

```python
import random

def find_kth_largest(nums, k):          # in place, iterative
    target = len(nums) - k              # index in ascending order
    lo, hi = 0, len(nums) - 1
    while True:
        p = random.randint(lo, hi); nums[p], nums[hi] = nums[hi], nums[p]
        pivot, w = nums[hi], lo
        for i in range(lo, hi):
            if nums[i] < pivot: nums[i], nums[w] = nums[w], nums[i]; w += 1
        nums[w], nums[hi] = nums[hi], nums[w]
        if w == target: return nums[w]
        if w < target: lo = w + 1
        else: hi = w - 1

def kth_largest_3way(nums, k):          # simpler, handles many duplicates well; O(n) extra space
    pivot = random.choice(nums)
    big = [x for x in nums if x > pivot]
    eq = sum(1 for x in nums if x == pivot)
    if k <= len(big): return kth_largest_3way(big, k)
    if k <= len(big) + eq: return pivot
    return kth_largest_3way([x for x in nums if x < pivot], k - len(big) - eq)

assert find_kth_largest([3, 2, 1, 5, 6, 4], 2) == 5
assert find_kth_largest([3, 2, 3, 1, 2, 4, 5, 5, 6], 4) == 4
assert kth_largest_3way([3, 2, 3, 1, 2, 4, 5, 5, 6], 4) == 4
```

- O(n) average, O(n²) worst (random pivot makes it vanishingly unlikely), O(1) extra space for the in-place version.
- Compare with the heap answer (O(n log k), better for streams) and median-of-medians (O(n) worst case; name it, do not code it).

### String matching (KMP, Rabin-Karp)

**Pattern:** KMP precomputes, for each prefix of the pattern, the length of its longest proper prefix that is also a suffix (LPS), so a mismatch never moves the text pointer backwards. Rabin-Karp compares rolling hashes of each window and verifies on a hash match.

```python
def kmp_search(text, pat):              # first index or -1
    if not pat: return 0
    lps, k = [0] * len(pat), 0
    for i in range(1, len(pat)):
        while k and pat[i] != pat[k]: k = lps[k - 1]
        if pat[i] == pat[k]: k += 1
        lps[i] = k
    k = 0
    for i, c in enumerate(text):
        while k and c != pat[k]: k = lps[k - 1]
        if c == pat[k]: k += 1
        if k == len(pat): return i - k + 1
    return -1

def rabin_karp(text, pat, base=256, mod=1_000_000_007):
    m, n = len(pat), len(text)
    if m > n: return -1
    high = pow(base, m - 1, mod)
    hp = ht = 0
    for i in range(m):
        hp = (hp * base + ord(pat[i])) % mod; ht = (ht * base + ord(text[i])) % mod
    for i in range(n - m + 1):
        if hp == ht and text[i:i + m] == pat: return i      # verify: hashes can collide
        if i + m < n:
            ht = ((ht - ord(text[i]) * high) * base + ord(text[i + m])) % mod
    return -1

for f in (kmp_search, rabin_karp):
    assert f("sadbutsad", "sad") == 0
    assert f("leetcode", "leeto") == -1
    assert f("aabaaabaaac", "aabaaac") == 4
```

- KMP: O(n + m) time, O(m) space. Rabin-Karp: O(n + m) expected, O(n·m) worst with many collisions.
- In real Python code `text.find(pat)` is the answer; KMP's LPS array also solves "shortest palindrome" and "repeated substring pattern". Rabin-Karp generalises to many patterns and to "longest duplicate substring" (binary search + rolling hash).

### Fenwick tree and segment tree (range sum with point updates)

**Pattern:** a prefix-sum array makes updates O(n). A Fenwick (binary indexed) tree stores partial sums over ranges sized by the lowest set bit, giving O(log n) update and prefix query. A segment tree stores sums of halves recursively and generalises to min/max/any associative operation.

```python
class Fenwick:
    def __init__(self, n):
        self.t = [0] * (n + 1)          # 1-indexed
    def add(self, i, delta):            # 0-indexed public API
        i += 1
        while i < len(self.t): self.t[i] += delta; i += i & -i
    def prefix(self, i):                # sum of [0, i)
        s = 0
        while i > 0: s += self.t[i]; i -= i & -i
        return s
    def range_sum(self, l, r):          # inclusive
        return self.prefix(r + 1) - self.prefix(l)

class SegmentTree:                      # iterative, bottom-up; leaves at [n, 2n)
    def __init__(self, nums):
        self.n = n = len(nums)
        self.t = [0] * n + list(nums)
        for i in range(n - 1, 0, -1): self.t[i] = self.t[2 * i] + self.t[2 * i + 1]
    def update(self, i, val):
        i += self.n; self.t[i] = val
        while i > 1: i //= 2; self.t[i] = self.t[2 * i] + self.t[2 * i + 1]
    def range_sum(self, l, r):          # inclusive
        s, l, r = 0, l + self.n, r + self.n + 1
        while l < r:
            if l & 1: s += self.t[l]; l += 1
            if r & 1: r -= 1; s += self.t[r]
            l //= 2; r //= 2
        return s

nums = [1, 3, 5]                        # LeetCode 307 example
fw = Fenwick(3)
for i, x in enumerate(nums): fw.add(i, x)
st = SegmentTree(nums)
assert fw.range_sum(0, 2) == st.range_sum(0, 2) == 9
fw.add(1, 2 - nums[1]); st.update(1, 2)  # Fenwick takes a delta, so subtract the old value
assert fw.range_sum(0, 2) == st.range_sum(0, 2) == 8
assert fw.range_sum(1, 1) == st.range_sum(1, 1) == 2
```

- Both: O(log n) update and query, O(n) space. Build is O(n) for the segment tree, O(n log n) for the Fenwick tree done by repeated `add`.
- Fenwick is shorter but limited to invertible operations (sums, XOR); use a segment tree for range min/max, and a lazy segment tree for range updates.
- Follow-ups: count of smaller numbers after self, range sum query 2D mutable, "my calendar" style problems.

### Bellman-Ford and Floyd-Warshall (negative edges, all-pairs)

**Pattern:** Bellman-Ford relaxes every edge V − 1 times (a shortest path has at most V − 1 edges); if a V-th round still relaxes something, there is a negative cycle. Floyd-Warshall is DP over "intermediate vertices allowed so far": `d[i][j] = min(d[i][j], d[i][k] + d[k][j])`.

```python
INF = float("inf")

def bellman_ford(n, edges, src):        # edges: (u, v, w); None if a negative cycle is reachable
    dist = [INF] * n; dist[src] = 0
    for _ in range(n - 1):
        changed = False
        for u, v, w in edges:
            if dist[u] + w < dist[v]: dist[v] = dist[u] + w; changed = True
        if not changed: break           # early exit
    for u, v, w in edges:
        if dist[u] + w < dist[v]: return None
    return dist

def floyd_warshall(n, edges):
    d = [[0 if i == j else INF for j in range(n)] for i in range(n)]
    for u, v, w in edges: d[u][v] = min(d[u][v], w)
    for k in range(n):                  # k must be the outer loop
        for i in range(n):
            for j in range(n):
                if d[i][k] + d[k][j] < d[i][j]: d[i][j] = d[i][k] + d[k][j]
    return d                            # negative cycle iff some d[i][i] < 0

edges = [(0, 1, 4), (0, 2, 5), (1, 2, -3), (2, 3, 4)]
assert bellman_ford(4, edges, 0) == [0, 4, 1, 5]
assert bellman_ford(3, [(0, 1, 1), (1, 2, -2), (2, 1, 1)], 0) is None
d = floyd_warshall(4, edges)
assert d[0] == [0, 4, 1, 5] and d[1][3] == 1 and d[3][0] == INF
```

- Bellman-Ford: O(V·E) time, O(V) space. Floyd-Warshall: O(V³) time, O(V²) space; fine up to a few hundred vertices.
- Use Dijkstra when weights are non-negative; Bellman-Ford when there are negative edges or a bound on the number of edges (cheapest flights within k stops); Floyd-Warshall for all-pairs on small dense graphs.

## Arrays & hashing

### Contains Duplicate (set)

**Pattern:** a set gives O(1) membership; return as soon as a value repeats.

```python
def contains_duplicate(nums):
    seen = set()
    for x in nums:
        if x in seen: return True
        seen.add(x)
    return False

assert contains_duplicate([1, 2, 3, 1]) is True
assert contains_duplicate([1, 2, 3, 4]) is False
assert contains_duplicate([1, 1, 1, 3, 3, 4, 3, 2, 4, 2]) is True
```

- O(n) time, O(n) space. One-liner: `len(set(nums)) != len(nums)` (no early exit).
- O(1) extra space alternative: sort and compare neighbours, O(n log n). Follow-ups: Contains Duplicate II (within distance k: sliding window set), III (value within t: buckets).

### Encode and Decode Strings (length-prefix each string)

**Pattern:** any delimiter can appear inside a string, so prefix each string with its length and a separator: `"4#neet"`. The decoder reads digits up to `#`, then exactly that many characters.

```python
def encode(strs):
    return "".join(f"{len(s)}#{s}" for s in strs)

def decode(s):
    res, i = [], 0
    while i < len(s):
        j = s.index("#", i)             # the first '#' after i ends the length
        n = int(s[i:j])
        res.append(s[j + 1:j + 1 + n]); i = j + 1 + n
    return res

for strs in (["neet", "code", "love", "you"], ["we", "say", ":", "yes"], [], [""], ["a#b", "12#", ""]):
    assert decode(encode(strs)) == strs
```

- O(total length) time and space for both.
- Pitfall: escaping a delimiter works too but is fiddlier; a fixed-width 4-byte length header is the binary-protocol version of the same idea.

### Valid Sudoku (row/column/box sets)

**Pattern:** one pass over the 81 cells; each digit must be new in its row, its column and its 3×3 box (index `(r // 3, c // 3)`).

```python
def is_valid_sudoku(board):
    seen = set()
    for r in range(9):
        for c in range(9):
            v = board[r][c]
            if v == ".": continue
            keys = (("r", r, v), ("c", c, v), ("b", r // 3, c // 3, v))
            if any(k in seen for k in keys): return False
            seen.update(keys)
    return True

board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],
         [".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],
         ["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],
         [".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],
         [".",".",".",".","8",".",".","7","9"]]
assert is_valid_sudoku(board) is True
board[0][0] = "8"                       # example 2: two 8s in the top-left box
assert is_valid_sudoku(board) is False
```

- O(81) = O(1) time and space for a fixed board; O(n²) for an n×n board.
- Valid does not mean solvable; solving is backtracking (see N-Queens, Sudoku solver in section B).

### Longest Consecutive Sequence (set; start only at sequence heads)

**Pattern:** put everything in a set; only start counting at `x` when `x - 1` is absent, so each sequence is walked once.

```python
def longest_consecutive(nums):
    s, best = set(nums), 0
    for x in s:
        if x - 1 in s: continue         # not the start of a run
        y = x
        while y + 1 in s: y += 1
        best = max(best, y - x + 1)
    return best

assert longest_consecutive([100, 4, 200, 1, 3, 2]) == 4
assert longest_consecutive([0, 3, 7, 2, 5, 8, 4, 6, 0, 1]) == 9
assert longest_consecutive([]) == 0
```

- O(n) time (each element is visited at most twice), O(n) space.
- Pitfall: iterate over the set, not the list, or duplicates of a run head repeat the walk. Sorting gives O(n log n) and is a fine first answer to mention.

## Sliding window

### Best Time to Buy and Sell Stock (track running minimum)

**Pattern:** for each day, the best sale is today's price minus the cheapest price seen so far.

```python
def max_profit(prices):
    lo, best = float("inf"), 0
    for p in prices:
        lo = min(lo, p)
        best = max(best, p - lo)
    return best

assert max_profit([7, 1, 5, 3, 6, 4]) == 5
assert max_profit([7, 6, 4, 3, 1]) == 0
```

- O(n) time, O(1) space.
- Variants: unlimited transactions (sum every positive day-to-day difference); with cooldown or fees (state machine DP, see 2-D DP below); at most k transactions (DP over k).

### Longest Repeating Character Replacement (window length minus max frequency <= k)

**Pattern:** a window is fixable if `len - count of its most common char <= k`. Grow right; when invalid, move left by one. `maxf` never needs to decrease: the answer only grows when a larger `maxf` is found.

```python
from collections import Counter
def character_replacement(s, k):
    cnt, left, maxf, best = Counter(), 0, 0, 0
    for r, c in enumerate(s):
        cnt[c] += 1; maxf = max(maxf, cnt[c])
        if r - left + 1 - maxf > k:     # invalid: slide (not shrink) the window
            cnt[s[left]] -= 1; left += 1
        best = max(best, r - left + 1)
    return best

assert character_replacement("ABAB", 2) == 4
assert character_replacement("AABABBA", 1) == 4
```

- O(n) time, O(alphabet) space.
- Pitfall: explaining why stale `maxf` is safe is the interview signal. The safe, slightly slower version recomputes `max(cnt.values())` (O(26 n)).

### Permutation in String (fixed-size window of counts)

**Pattern:** slide a window of length `len(s1)` across `s2` and compare its character counts with `s1`'s.

```python
from collections import Counter
def check_inclusion(s1, s2):
    m = len(s1)
    need, win = Counter(s1), Counter(s2[:m])
    if win == need: return True
    for i in range(m, len(s2)):
        win[s2[i]] += 1; win[s2[i - m]] -= 1
        if win[s2[i - m]] == 0: del win[s2[i - m]]   # keep equality comparison exact
        if win == need: return True
    return False

assert check_inclusion("ab", "eidbaooo") is True
assert check_inclusion("ab", "eidboaoo") is False
assert check_inclusion("abc", "ab") is False
```

- O(26·n) time with Counter comparison, O(26) space. Optimise to O(n) by tracking how many of the 26 counts currently match.
- Same pattern: Find All Anagrams in a String (collect every start index).

## Stack

### Evaluate Reverse Polish Notation (operand stack; truncate division toward zero)

**Pattern:** push numbers; on an operator pop two (right operand first), apply, push the result.

```python
import operator
def eval_rpn(tokens):
    ops = {"+": operator.add, "-": operator.sub, "*": operator.mul,
           "/": lambda a, b: int(a / b)}     # truncate toward zero
    st = []
    for t in tokens:
        if t in ops:
            b, a = st.pop(), st.pop()
            st.append(ops[t](a, b))
        else:
            st.append(int(t))
    return st[0]

assert eval_rpn(["2", "1", "+", "3", "*"]) == 9
assert eval_rpn(["4", "13", "5", "/", "+"]) == 6
assert eval_rpn(["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]) == 22
```

- O(n) time, O(n) space.
- Pitfalls: Python's `//` floors (`-7 // 2 == -4`), the problem wants truncation (`-3`); `int(a / b)` is fine for 32-bit values, for huge ints use `abs(a) // abs(b)` with the sign fixed. Pop order matters for `-` and `/`. Do not detect numbers with `isdigit()` ("-11").

### Car Fleet (sort by position; stack of arrival times)

**Pattern:** process cars from closest to the target backwards. A car that would arrive no later than the fleet ahead catches it and merges; a slower arrival time starts a new fleet.

```python
def car_fleet(target, position, speed):
    fleets, slowest = 0, 0.0
    for p, s in sorted(zip(position, speed), reverse=True):
        t = (target - p) / s
        if t > slowest:                 # cannot catch the fleet ahead
            fleets += 1; slowest = t
    return fleets

assert car_fleet(12, [10, 8, 0, 5, 3], [2, 4, 1, 1, 3]) == 3
assert car_fleet(10, [3], [3]) == 1
assert car_fleet(100, [0, 2, 4], [4, 2, 1]) == 1
```

- O(n log n) time for the sort, O(n) space.
- The "stack" is only ever inspected at its top, so a single `slowest` variable replaces it. To avoid float equality issues, compare `(target - p) * s_prev` with `(target - p_prev) * s` instead.

## Binary search

### Search a 2D Matrix (treat as one sorted array)

**Pattern:** rows are sorted and each row starts after the previous ends, so index `k` maps to `(k // C, k % C)` in a virtual sorted array of length R·C.

```python
def search_matrix(m, target):
    R, C = len(m), len(m[0])
    lo, hi = 0, R * C - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        v = m[mid // C][mid % C]
        if v == target: return True
        if v < target: lo = mid + 1
        else: hi = mid - 1
    return False

m = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]
assert search_matrix(m, 3) is True
assert search_matrix(m, 13) is False
assert search_matrix([[1]], 1) is True
```

- O(log(R·C)) time, O(1) space.
- Follow-up: Search a 2D Matrix II (rows and columns sorted independently): start at the top-right corner and step left or down, O(R + C).

### Find Minimum in Rotated Sorted Array (compare mid with right end)

**Pattern:** if `nums[mid] > nums[hi]`, the drop (and the minimum) is to the right of mid; otherwise the minimum is at mid or to its left.

```python
def find_min(nums):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] > nums[hi]: lo = mid + 1
        else: hi = mid                  # mid could be the minimum
    return nums[lo]

assert find_min([3, 4, 5, 1, 2]) == 1
assert find_min([4, 5, 6, 7, 0, 1, 2]) == 0
assert find_min([11, 13, 15, 17]) == 11
assert find_min([2, 1]) == 1
```

- O(log n) time, O(1) space.
- Pitfalls: compare with `nums[hi]`, not `nums[lo]` (comparing with lo fails on an unrotated array). With duplicates (problem II), `nums[mid] == nums[hi]` means `hi -= 1`, and the worst case becomes O(n).

### Time Based Key-Value Store (per-key timestamp list + bisect)

**Pattern:** timestamps arrive in increasing order per key, so append to a sorted list and binary-search for the last timestamp `<=` the query.

```python
import bisect
from collections import defaultdict

class TimeMap:
    def __init__(self):
        self.times = defaultdict(list)  # key -> [timestamps]
        self.vals = defaultdict(list)   # key -> [values], parallel list

    def set(self, key, value, timestamp):
        self.times[key].append(timestamp); self.vals[key].append(value)

    def get(self, key, timestamp):
        i = bisect.bisect_right(self.times.get(key, []), timestamp)
        return self.vals[key][i - 1] if i else ""

tm = TimeMap()
tm.set("foo", "bar", 1)
assert tm.get("foo", 1) == "bar" and tm.get("foo", 3) == "bar"
tm.set("foo", "bar2", 4)
assert tm.get("foo", 4) == "bar2" and tm.get("foo", 5) == "bar2"
assert tm.get("foo", 0) == "" and tm.get("missing", 1) == ""
```

- `set`: O(1). `get`: O(log n) for n versions of the key. Space O(total sets).
- Parallel lists keep `bisect` simple (`key=` on bisect needs 3.10+). Follow-ups: out-of-order timestamps (use `insort` or a sorted container), memory limits (drop old versions), the real-world version is MVCC / snapshot reads.

## Linked list

### Reorder List (find middle, reverse second half, merge)

**Pattern:** L0→Ln→L1→Ln-1… is the first half interleaved with the reversed second half. Three standard sub-routines, done in place.

```python
class ListNode:
    def __init__(self, val=0, next=None): self.val, self.next = val, next
def build(xs):
    head = None
    for x in reversed(xs): head = ListNode(x, head)
    return head
def to_list(n):
    out = []
    while n: out.append(n.val); n = n.next
    return out

def reorder_list(head):
    slow = fast = head
    while fast and fast.next: slow, fast = slow.next, fast.next.next
    prev, cur = None, slow.next; slow.next = None     # cut, then reverse the second half
    while cur: cur.next, prev, cur = prev, cur, cur.next
    a, b = head, prev
    while b:                            # second half is never longer than the first
        a.next, a = b, a.next
        b.next, b = a, b.next

for xs, want in (([1, 2, 3, 4], [1, 4, 2, 3]), ([1, 2, 3, 4, 5], [1, 5, 2, 4, 3]), ([1], [1])):
    h = build(xs); reorder_list(h); assert to_list(h) == want
```

- O(n) time, O(1) space.
- Pitfall: forgetting `slow.next = None` creates a cycle. The easy O(n)-space version copies nodes into a list and uses two pointers; say it first, then optimise.

### Add Two Numbers (digit-by-digit with carry)

**Pattern:** digits are stored least significant first, so walk both lists together, adding with a carry; a dummy head avoids special cases.

```python
class ListNode:
    def __init__(self, val=0, next=None): self.val, self.next = val, next
def build(xs):
    head = None
    for x in reversed(xs): head = ListNode(x, head)
    return head
def to_list(n):
    out = []
    while n: out.append(n.val); n = n.next
    return out

def add_two_numbers(a, b):
    dummy = tail = ListNode(); carry = 0
    while a or b or carry:
        s = carry + (a.val if a else 0) + (b.val if b else 0)
        carry, d = divmod(s, 10)
        tail.next = tail = ListNode(d)
        a = a.next if a else None; b = b.next if b else None
    return dummy.next

assert to_list(add_two_numbers(build([2, 4, 3]), build([5, 6, 4]))) == [7, 0, 8]
assert to_list(add_two_numbers(build([0]), build([0]))) == [0]
assert to_list(add_two_numbers(build([9] * 7), build([9] * 4))) == [8, 9, 9, 9, 0, 0, 0, 1]
```

- O(max(m, n)) time, O(max(m, n)) for the output.
- Pitfalls: the final carry (`or carry` in the loop condition). Follow-up Add Two Numbers II (most significant digit first): use two stacks, or reverse both lists.

### Find the Duplicate Number (Floyd's cycle detection on indices)

**Pattern:** values are in `1..n` with n + 1 slots, so `i → nums[i]` is a linked list that must contain a cycle; the duplicate value is the cycle's entrance. Same algorithm as "find cycle start".

```python
def find_duplicate(nums):
    slow = fast = nums[0]
    while True:
        slow, fast = nums[slow], nums[nums[fast]]
        if slow == fast: break
    slow = nums[0]
    while slow != fast:
        slow, fast = nums[slow], nums[fast]
    return slow

assert find_duplicate([1, 3, 4, 2, 2]) == 2
assert find_duplicate([3, 1, 3, 4, 2]) == 3
assert find_duplicate([3, 3, 3, 3, 3]) == 3
```

- O(n) time, O(1) space, does not modify the array (the constraints that rule out sorting and sets).
- Alternative within the constraints: binary search on the value `v`, counting elements `<= v` (pigeonhole), O(n log n).

## Trees

### Invert Binary Tree

**Pattern:** swap the children of every node; any traversal works.

```python
from collections import deque
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right
def level_order(root):
    out, q = [], deque([root] if root else [])
    while q:
        n = q.popleft(); out.append(n.val)
        q.extend(c for c in (n.left, n.right) if c)
    return out

def invert_tree(root):
    if root:
        root.left, root.right = invert_tree(root.right), invert_tree(root.left)
    return root

T = TreeNode
root = T(4, T(2, T(1), T(3)), T(7, T(6), T(9)))
assert level_order(invert_tree(root)) == [4, 7, 2, 9, 6, 3, 1]
assert level_order(invert_tree(T(2, T(1), T(3)))) == [2, 3, 1]
assert invert_tree(None) is None
```

- O(n) time, O(h) stack (O(n) for a skewed tree; use an explicit stack or BFS to avoid recursion limits).
- Related: Symmetric Tree (compare `left` with mirrored `right`).

### Same Tree

**Pattern:** two trees are equal if the roots match and both pairs of subtrees are equal; handle `None` first.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right

def is_same_tree(p, q):
    if not p or not q: return p is q    # both None -> True, one None -> False
    return p.val == q.val and is_same_tree(p.left, q.left) and is_same_tree(p.right, q.right)

T = TreeNode
assert is_same_tree(T(1, T(2), T(3)), T(1, T(2), T(3))) is True
assert is_same_tree(T(1, T(2)), T(1, None, T(2))) is False
assert is_same_tree(T(1, T(2), T(1)), T(1, T(1), T(2))) is False
```

- O(min(m, n)) time (stops at the first difference), O(h) stack.
- This helper is the building block for Subtree of Another Tree and Symmetric Tree.

### Subtree of Another Tree (same-tree check at every node; or serialise)

**Pattern:** for each node of `root`, check whether the tree rooted there equals `sub`. The linear alternative serialises both trees with null markers and does a substring search.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right

def is_same(p, q):
    if not p or not q: return p is q
    return p.val == q.val and is_same(p.left, q.left) and is_same(p.right, q.right)

def is_subtree(root, sub):
    if not root: return sub is None
    return is_same(root, sub) or is_subtree(root.left, sub) or is_subtree(root.right, sub)

def is_subtree_linear(root, sub):       # serialise with markers, then substring search
    def ser(n): return f"^{n.val}{ser(n.left)}{ser(n.right)}" if n else "#"
    return ser(sub) in ser(root)

T = TreeNode
sub = T(4, T(1), T(2))
r1 = T(3, T(4, T(1), T(2)), T(5))
r2 = T(3, T(4, T(1), T(2, T(0))), T(5))
for f in (is_subtree, is_subtree_linear):
    assert f(r1, sub) is True and f(r2, sub) is False
assert is_subtree_linear(T(12), T(2)) is False    # the "^" marker stops "2" matching inside "12"
```

- Recursive: O(m·n) worst case, O(h) stack. Serialised: O(m + n) with KMP (Python's `in` is fast in practice).
- Pitfall: without a start marker, value `2` matches inside `12`; without null markers, different shapes serialise the same.

### Binary Tree Right Side View (BFS, last node per level)

**Pattern:** level-order traversal; the last node in each level is the one visible from the right.

```python
from collections import deque
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right

def right_side_view(root):
    res, q = [], deque([root] if root else [])
    while q:
        res.append(q[-1].val)           # rightmost node of this level
        for _ in range(len(q)):
            n = q.popleft()
            q.extend(c for c in (n.left, n.right) if c)
    return res

T = TreeNode
assert right_side_view(T(1, T(2, None, T(5)), T(3, None, T(4)))) == [1, 3, 4]
assert right_side_view(T(1, T(2, T(4)), T(3))) == [1, 3, 4]       # left node visible at depth 2
assert right_side_view(None) == []
```

- O(n) time, O(w) space for the widest level.
- DFS alternative: visit right child first and record a node when `depth == len(res)`. Left side view: take `q[0]` instead.

### Count Good Nodes in Binary Tree (DFS carrying the path maximum)

**Pattern:** a node is good if no node on the root-to-node path is larger, so pass the running maximum down.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right

def good_nodes(root):
    count, st = 0, [(root, float("-inf"))] if root else []
    while st:                           # iterative DFS: safe on deep trees
        n, mx = st.pop()
        if n.val >= mx: count += 1
        mx = max(mx, n.val)
        st.extend((c, mx) for c in (n.left, n.right) if c)
    return count

T = TreeNode
assert good_nodes(T(3, T(1, T(3)), T(4, T(1), T(5)))) == 4
assert good_nodes(T(3, T(3, T(4), T(2)))) == 3
assert good_nodes(T(1)) == 1
```

- O(n) time, O(h) space.
- Pitfall: `>=`, not `>`: equal values count as good. The same "carry state down the path" pattern solves Validate BST (carry bounds) and path sums.

### Construct Binary Tree from Preorder and Inorder Traversal (root from preorder, split by inorder index)

**Pattern:** preorder's next value is the current subtree's root; its position in inorder splits the remaining values into left and right subtrees. A value → index map makes each split O(1).

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None): self.val, self.left, self.right = val, left, right

def build_tree(preorder, inorder):
    idx = {v: i for i, v in enumerate(inorder)}
    pre = iter(preorder)
    def build(lo, hi):                  # subtree covering inorder[lo:hi]
        if lo >= hi: return None
        v = next(pre)
        n = TreeNode(v)
        n.left = build(lo, idx[v])      # left first: preorder is root, left, right
        n.right = build(idx[v] + 1, hi)
        return n
    return build(0, len(inorder))

def traversals(n):
    if not n: return [], []
    lp, li = traversals(n.left); rp, ri = traversals(n.right)
    return [n.val] + lp + rp, li + [n.val] + ri

for pre, ino in (([3, 9, 20, 15, 7], [9, 3, 15, 20, 7]), ([-1], [-1]), ([1, 2, 3], [3, 2, 1])):
    assert traversals(build_tree(pre, ino)) == (pre, ino)
root = build_tree([3, 9, 20, 15, 7], [9, 3, 15, 20, 7])
assert (root.val, root.left.val, root.right.val, root.right.left.val) == (3, 9, 20, 15)
```

- O(n) time, O(n) space for the map and recursion.
- Pitfalls: slicing lists (`preorder[1:k+1]`) makes it O(n²); values must be unique for the map. Variants: inorder + postorder (consume postorder from the end, build right first); preorder + postorder is ambiguous without a full-tree guarantee.

## Heap

### Kth Largest Element in a Stream (min-heap of size k)

**Pattern:** keep only the k largest values in a min-heap; its root is the kth largest.

```python
import heapq

class KthLargest:
    def __init__(self, k, nums):
        self.k, self.h = k, list(nums)
        heapq.heapify(self.h)
        while len(self.h) > k: heapq.heappop(self.h)

    def add(self, val):
        if len(self.h) < self.k: heapq.heappush(self.h, val)
        elif val > self.h[0]: heapq.heapreplace(self.h, val)   # pop + push in one O(log k) step
        return self.h[0]

kl = KthLargest(3, [4, 5, 8, 2])
assert [kl.add(x) for x in (3, 5, 10, 9, 4)] == [4, 5, 5, 8, 8]
kl2 = KthLargest(1, [])
assert [kl2.add(x) for x in (-3, -2, -4, 0, 4)] == [-3, -2, -2, 0, 4]
```

- Init: O(n + (n − k) log n). `add`: O(log k). Space O(k).
- Pitfall: the initial list may have fewer than k items. Contrast with Median from Data Stream (two heaps) in section B.

### Last Stone Weight (max-heap via negation)

**Pattern:** repeatedly take the two heaviest stones; push back the difference if non-zero. `heapq` is a min-heap, so store negatives.

```python
import heapq
def last_stone_weight(stones):
    h = [-s for s in stones]; heapq.heapify(h)
    while len(h) > 1:
        y, x = -heapq.heappop(h), -heapq.heappop(h)   # y >= x
        if y != x: heapq.heappush(h, -(y - x))
    return -h[0] if h else 0

assert last_stone_weight([2, 7, 4, 1, 8, 1]) == 1
assert last_stone_weight([1]) == 1
assert last_stone_weight([2, 2]) == 0
```

- O(n log n) time, O(n) space.
- Follow-up: Last Stone Weight II (any two stones, minimise the remainder) is a partition-into-two-subsets DP, not a heap problem.

### K Closest Points to Origin (max-heap of size k, or nsmallest)

**Pattern:** keep the k smallest distances seen so far in a max-heap of size k (push negated squared distance). Compare squared distances; no `sqrt` needed.

```python
import heapq
def k_closest(points, k):
    h = []                              # max-heap via negation: (-dist2, x, y)
    for x, y in points:
        heapq.heappush(h, (-(x * x + y * y), x, y))
        if len(h) > k: heapq.heappop(h) # drop the farthest
    return [[x, y] for _, x, y in h]

def k_closest_short(points, k):
    return heapq.nsmallest(k, points, key=lambda p: p[0] ** 2 + p[1] ** 2)

for f in (k_closest, k_closest_short):
    assert sorted(f([[1, 3], [-2, 2]], 1)) == [[-2, 2]]
    assert sorted(f([[3, 3], [5, -1], [-2, 4]], 2)) == [[-2, 4], [3, 3]]
```

- O(n log k) time, O(k) space. Quickselect on distances gives O(n) average if the order of the output does not matter.
- Pitfall: a min-heap of all n points is O(n log n); fine to mention, but the size-k heap is the expected answer for streams.

### Design Twitter (per-user tweet lists + heap merge of followees)

**Pattern:** store each user's tweets with a global increasing timestamp. The news feed is a k-way merge of the followees' (and own) tweet lists, newest first, stopping after 10.

```python
import heapq, itertools
from collections import defaultdict

class Twitter:
    def __init__(self):
        self.clock = itertools.count()
        self.tweets = defaultdict(list)     # user -> [(time, tweet_id)], oldest first
        self.following = defaultdict(set)

    def postTweet(self, user, tweet_id):
        self.tweets[user].append((next(self.clock), tweet_id))

    def getNewsFeed(self, user):
        h = []
        for u in self.following[user] | {user}:
            if self.tweets[u]:
                i = len(self.tweets[u]) - 1
                t, tid = self.tweets[u][i]
                h.append((-t, tid, u, i))
        heapq.heapify(h); feed = []
        while h and len(feed) < 10:
            _, tid, u, i = heapq.heappop(h); feed.append(tid)
            if i:
                t, nid = self.tweets[u][i - 1]
                heapq.heappush(h, (-t, nid, u, i - 1))
        return feed

    def follow(self, a, b):
        if a != b: self.following[a].add(b)

    def unfollow(self, a, b):
        self.following[a].discard(b)

tw = Twitter()
tw.postTweet(1, 5); assert tw.getNewsFeed(1) == [5]
tw.follow(1, 2); tw.postTweet(2, 6); assert tw.getNewsFeed(1) == [6, 5]
tw.unfollow(1, 2); assert tw.getNewsFeed(1) == [5]
for i in range(20): tw.postTweet(1, 100 + i)
assert tw.getNewsFeed(1) == list(range(119, 109, -1))
```

- `postTweet`, `follow`, `unfollow`: O(1). `getNewsFeed`: O(F + 10 log F) for F followees. Space O(users + tweets + follows).
- Pitfalls: `unfollow` of someone not followed (use `discard`); following yourself. System design follow-up: fan-out on write (push to followers' timelines) vs fan-out on read (this merge), and the hybrid for celebrities.

## Backtracking

### Palindrome Partitioning (backtrack over prefix cuts)

**Pattern:** at index `i`, try every end `j` such that `s[i:j]` is a palindrome, recurse from `j`, then undo. Precompute a palindrome table to make each check O(1).

```python
def partition(s):
    n = len(s)
    pal = [[False] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        for j in range(i, n):
            pal[i][j] = s[i] == s[j] and (j - i < 2 or pal[i + 1][j - 1])
    res, path = [], []
    def bt(i):
        if i == n: res.append(path[:]); return
        for j in range(i, n):
            if pal[i][j]:
                path.append(s[i:j + 1]); bt(j + 1); path.pop()
    bt(0)
    return res

assert partition("aab") == [["a", "a", "b"], ["aa", "b"]]
assert partition("a") == [["a"]]
assert len(partition("aaaa")) == 8      # 2^(n-1) when every substring is a palindrome
```

- O(n · 2ⁿ) time worst case (2ⁿ⁻¹ partitions, each copied in O(n)), O(n²) for the table.
- Follow-up: Palindrome Partitioning II (minimum cuts) drops the enumeration: `cuts[j] = min(cuts[i - 1] + 1)` over palindromic `s[i..j]`, O(n²).

## Graphs

### Max Area of Island (DFS returning area)

**Pattern:** same flood fill as Number of Islands, but count cells as you sink them and keep the maximum.

```python
DIRS = ((1, 0), (-1, 0), (0, 1), (0, -1))
def max_area_of_island(grid):
    R, C, best = len(grid), len(grid[0]), 0
    for i in range(R):
        for j in range(C):
            if grid[i][j] != 1: continue
            grid[i][j] = 0; st, area = [(i, j)], 0
            while st:
                r, c = st.pop(); area += 1
                for dr, dc in DIRS:
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] == 1:
                        grid[nr][nc] = 0; st.append((nr, nc))
            best = max(best, area)
    return best

g = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],
     [0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],
     [0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
assert max_area_of_island(g) == 6
assert max_area_of_island([[0, 0, 0, 0, 0, 0, 0, 0]]) == 0
```

- O(R·C) time, O(R·C) worst-case stack.
- Pitfall: this mutates the input; say so, or keep a `seen` set. Follow-up: Making a Large Island (label islands with ids and sizes, then try flipping each 0).

### Pacific Atlantic Water Flow (reverse flow: search inward from each ocean)

**Pattern:** instead of asking where water from each cell can go (O((R·C)²)), start from each ocean's border and climb to cells of equal or greater height. The answer is the intersection of the two reachable sets.

```python
def pacific_atlantic(h):
    R, C = len(h), len(h[0])
    def reach(starts):
        seen, st = set(starts), list(starts)
        while st:
            r, c = st.pop()
            for nr, nc in ((r + 1, c), (r - 1, c), (r, c + 1), (r, c - 1)):
                if (0 <= nr < R and 0 <= nc < C and (nr, nc) not in seen
                        and h[nr][nc] >= h[r][c]):      # water flows downhill to (r, c)
                    seen.add((nr, nc)); st.append((nr, nc))
        return seen
    pac = reach([(0, c) for c in range(C)] + [(r, 0) for r in range(R)])
    atl = reach([(R - 1, c) for c in range(C)] + [(r, C - 1) for r in range(R)])
    return sorted([r, c] for r, c in pac & atl)

h = [[1, 2, 2, 3, 5], [3, 2, 3, 4, 4], [2, 4, 5, 3, 1], [6, 7, 1, 4, 5], [5, 1, 1, 2, 4]]
assert pacific_atlantic(h) == [[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]]
assert pacific_atlantic([[1]]) == [[0, 0]]
```

- O(R·C) time and space (each cell visited at most once per ocean).
- The general lesson: "can X reach any target?" for many X is cheaper as one multi-source search backwards from the targets.

### Surrounded Regions (mark border-connected O's first)

**Pattern:** an `O` survives only if it connects to the border. Flood fill from border `O`s marking them safe, then flip every other `O` to `X` and restore the safe ones.

```python
def solve(board):
    R, C = len(board), len(board[0])
    st = [(r, c) for r in range(R) for c in range(C)
          if (r in (0, R - 1) or c in (0, C - 1)) and board[r][c] == "O"]
    while st:
        r, c = st.pop()
        if 0 <= r < R and 0 <= c < C and board[r][c] == "O":
            board[r][c] = "S"           # safe
            st += [(r + 1, c), (r - 1, c), (r, c + 1), (r, c - 1)]
    for r in range(R):
        for c in range(C):
            board[r][c] = "O" if board[r][c] == "S" else "X"

b = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
solve(b)
assert b == [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
b = [["X"]]; solve(b); assert b == [["X"]]
```

- O(R·C) time and space.
- Union-find with a virtual "border" node also works; flood fill is simpler to get right under time pressure.

### Graph Valid Tree (n - 1 edges and connected)

**Pattern:** a graph is a tree exactly when it has n − 1 edges and no cycle (equivalently, is connected). Union-find detects a cycle when an edge joins two nodes already in the same set.

```python
def valid_tree(n, edges):
    if len(edges) != n - 1: return False
    parent = list(range(n))
    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]   # path halving
            x = parent[x]
        return x
    for a, b in edges:
        ra, rb = find(a), find(b)
        if ra == rb: return False       # cycle
        parent[ra] = rb
    return True                         # n - 1 edges + no cycle => connected

assert valid_tree(5, [[0, 1], [0, 2], [0, 3], [1, 4]]) is True
assert valid_tree(5, [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]) is False
assert valid_tree(4, [[0, 1], [2, 3]]) is False
assert valid_tree(1, []) is True
```

- O(E·α(n)) ≈ O(n) time, O(n) space.
- Alternative: BFS/DFS from node 0 and check every node is visited (with the edge-count check). Related: Number of Connected Components (count successful unions).

## Advanced graphs

### Reconstruct Itinerary (Hierholzer's Eulerian path)

**Pattern:** every ticket must be used once, so this is an Eulerian path. Hierholzer's algorithm: follow the lexically smallest unused edge until stuck, then add the stuck airport to the route; the route built in reverse is the answer.

```python
from collections import defaultdict
def find_itinerary(tickets):
    g = defaultdict(list)
    for a, b in sorted(tickets, reverse=True):
        g[a].append(b)                  # reverse-sorted, so pop() gives the smallest
    route, st = [], ["JFK"]
    while st:
        while g[st[-1]]: st.append(g[st[-1]].pop())
        route.append(st.pop())          # dead end: it goes last among what's left
    return route[::-1]

assert find_itinerary([["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]) == \
    ["JFK", "MUC", "LHR", "SFO", "SJC"]
assert find_itinerary([["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]) == \
    ["JFK", "ATL", "JFK", "SFO", "ATL", "SFO"]
assert find_itinerary([["JFK","KUL"],["JFK","NRT"],["NRT","JFK"]]) == ["JFK", "NRT", "JFK", "KUL"]
```

- O(E log E) time for sorting, O(E) space.
- Pitfall: greedy "always take the smallest next airport" fails on the third example (it strands you at KUL); plain backtracking works but is exponential in the worst case.

### Min Cost to Connect All Points (dense Prim, O(n^2))

**Pattern:** minimum spanning tree on a complete graph with Manhattan weights. With n² implicit edges, array-based Prim (pick the closest unvisited point, then relax all others) beats building the edge list.

```python
def min_cost_connect_points(points):
    n = len(points)
    dist = [float("inf")] * n; dist[0] = 0
    in_tree, total = [False] * n, 0
    for _ in range(n):
        u = min((d, i) for i, d in enumerate(dist) if not in_tree[i])[1]
        in_tree[u] = True; total += dist[u]
        ux, uy = points[u]
        for v in range(n):
            if not in_tree[v]:
                dist[v] = min(dist[v], abs(ux - points[v][0]) + abs(uy - points[v][1]))
    return total

assert min_cost_connect_points([[0, 0], [2, 2], [3, 10], [5, 2], [7, 0]]) == 20
assert min_cost_connect_points([[3, 12], [-2, 5], [-4, 1]]) == 18
assert min_cost_connect_points([[0, 0]]) == 0
```

- O(n²) time, O(n) space. Kruskal on all pairs is O(n² log n) time and O(n²) space; heap Prim is O(n² log n) here.
- See Minimum spanning tree (Kruskal/Prim) in section B for the general templates.

### Swim in Rising Water (Dijkstra on max elevation along the path)

**Pattern:** the cost of a path is its highest cell, and we want the path minimising that. Dijkstra works with `max` in place of `+`: always expand the reachable cell with the lowest elevation.

```python
import heapq
def swim_in_water(grid):
    n = len(grid)
    h, seen = [(grid[0][0], 0, 0)], {(0, 0)}
    while h:
        t, r, c = heapq.heappop(h)
        if (r, c) == (n - 1, n - 1): return t
        for nr, nc in ((r + 1, c), (r - 1, c), (r, c + 1), (r, c - 1)):
            if 0 <= nr < n and 0 <= nc < n and (nr, nc) not in seen:
                seen.add((nr, nc))
                heapq.heappush(h, (max(t, grid[nr][nc]), nr, nc))

assert swim_in_water([[0, 2], [1, 3]]) == 3
assert swim_in_water([[0, 1, 2, 3, 4], [24, 23, 22, 21, 5], [12, 13, 14, 15, 16],
                      [11, 17, 18, 19, 20], [10, 9, 8, 7, 6]]) == 16
assert swim_in_water([[0]]) == 0
```

- O(n² log n) time, O(n²) space.
- Alternatives: binary search on t with BFS (O(n² log n)), or union-find adding cells in elevation order until corners connect. Same pattern: Path With Minimum Effort.

## 1-D dynamic programming

### Min Cost Climbing Stairs

**Pattern:** `dp[i]` = cheapest cost to stand on step i = `min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`; only two values are needed.

```python
def min_cost_climbing_stairs(cost):
    a = b = 0                           # cost to reach step i-2 and i-1 (start at 0 or 1 for free)
    for i in range(2, len(cost) + 1):
        a, b = b, min(b + cost[i - 1], a + cost[i - 2])
    return b

assert min_cost_climbing_stairs([10, 15, 20]) == 15
assert min_cost_climbing_stairs([1, 100, 1, 1, 1, 100, 1, 1, 100, 1]) == 6
assert min_cost_climbing_stairs([0, 0]) == 0
```

- O(n) time, O(1) space.
- Pitfall: the top is one past the last index, and you may start on step 0 or 1 free of charge.

### Palindromic Substrings (expand around centres)

**Pattern:** every palindrome has a centre (a character or a gap: 2n − 1 centres). Expand outward from each and count every successful step.

```python
def count_substrings(s):
    count = 0
    for c in range(2 * len(s) - 1):
        l, r = c // 2, c // 2 + c % 2   # even c: odd-length centre; odd c: gap
        while l >= 0 and r < len(s) and s[l] == s[r]:
            count += 1; l -= 1; r += 1
    return count

assert count_substrings("abc") == 3
assert count_substrings("aaa") == 6
assert count_substrings("") == 0
```

- O(n²) time, O(1) space. The DP table `pal[i][j]` is also O(n²) time but O(n²) space; Manacher is O(n).
- Same expansion as Longest Palindromic Substring in section B; here you count instead of tracking the best.

## 2-D dynamic programming

### Best Time to Buy and Sell Stock with Cooldown (state machine: hold / sold / rest)

**Pattern:** three states at the end of each day: `hold` (own a share), `sold` (sold today, so tomorrow is cooldown), `rest` (no share, free to buy). Transition all three from yesterday's values.

```python
def max_profit_cooldown(prices):
    hold, sold, rest = float("-inf"), 0, 0
    for p in prices:
        hold, sold, rest = max(hold, rest - p), hold + p, max(rest, sold)
    return max(sold, rest)

assert max_profit_cooldown([1, 2, 3, 0, 2]) == 3
assert max_profit_cooldown([1]) == 0
assert max_profit_cooldown([2, 1]) == 0
```

- O(n) time, O(1) space.
- Pitfall: update all states simultaneously from the previous day (tuple assignment), or buying uses today's `rest`. The same machine with a fee on sell solves "with transaction fee".

### Target Sum (subset-sum reduction or count of sums)

**Pattern:** track how many ways each running sum can be reached after each number. Equivalent reduction: choose a positive subset P with `sum(P) = (total + target) / 2` and count subsets (0/1 knapsack).

```python
from collections import Counter
def find_target_sum_ways(nums, target):
    ways = Counter({0: 1})
    for x in nums:
        nxt = Counter()
        for s, c in ways.items():
            nxt[s + x] += c; nxt[s - x] += c
        ways = nxt
    return ways[target]

def find_target_sum_ways_knapsack(nums, target):
    total = sum(nums)
    if abs(target) > total or (total + target) % 2: return 0
    goal = (total + target) // 2
    dp = [1] + [0] * goal
    for x in nums:
        for s in range(goal, x - 1, -1): dp[s] += dp[s - x]   # backwards: each x once
    return dp[goal]

for f in (find_target_sum_ways, find_target_sum_ways_knapsack):
    assert f([1, 1, 1, 1, 1], 3) == 5
    assert f([1], 1) == 1
    assert f([0, 0, 1], 1) == 4     # zeros can take either sign
```

- Counter version: O(n · S) time and O(S) space, S = sum of nums. Knapsack: O(n · goal) time, O(goal) space.
- Pitfall: the parity and range checks in the reduction; zeros double the count (handled naturally by both).

### Interleaving String (dp over prefixes of both strings)

**Pattern:** `dp[i][j]` = can `s3[:i+j]` be formed by interleaving `s1[:i]` and `s2[:j]`. The last character of that prefix came from either `s1` or `s2`.

```python
def is_interleave(s1, s2, s3):
    m, n = len(s1), len(s2)
    if m + n != len(s3): return False
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    dp[0][0] = True
    for i in range(m + 1):
        for j in range(n + 1):
            if i and dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]: dp[i][j] = True
            if j and dp[i][j - 1] and s2[j - 1] == s3[i + j - 1]: dp[i][j] = True
    return dp[m][n]

assert is_interleave("aabcc", "dbbca", "aadbbcbcac") is True
assert is_interleave("aabcc", "dbbca", "aadbbbaccc") is False
assert is_interleave("", "", "") is True
assert is_interleave("a", "", "aa") is False
```

- O(m·n) time, O(m·n) space; a single row reduces space to O(n).
- Pitfall: greedy two-pointer matching fails when both strings offer the same next character; check the lengths first.

### Longest Increasing Path in a Matrix (DFS + memo)

**Pattern:** strictly increasing moves cannot cycle, so the grid is a DAG and `longest(r, c) = 1 + max(longest(neighbour))` over larger neighbours can be memoised.

```python
from functools import lru_cache
def longest_increasing_path(m):
    R, C = len(m), len(m[0])
    @lru_cache(maxsize=None)
    def dfs(r, c):
        best = 1
        for nr, nc in ((r + 1, c), (r - 1, c), (r, c + 1), (r, c - 1)):
            if 0 <= nr < R and 0 <= nc < C and m[nr][nc] > m[r][c]:
                best = max(best, 1 + dfs(nr, nc))
        return best
    return max(dfs(r, c) for r in range(R) for c in range(C))

assert longest_increasing_path([[9, 9, 4], [6, 6, 8], [2, 1, 1]]) == 4
assert longest_increasing_path([[3, 4, 5], [3, 2, 6], [2, 2, 1]]) == 4
assert longest_increasing_path([[1]]) == 1
```

- O(R·C) time (each cell computed once, 4 neighbours), O(R·C) space.
- Pitfalls: recursion depth can reach R·C (raise the limit, or use the iterative alternative: topological sort / BFS peeling cells by out-degree, where the number of layers is the answer). No `visited` set is needed because of strictness.

### Distinct Subsequences (count dp over prefixes)

**Pattern:** `dp[j]` = number of ways to form `t[:j]` from the prefix of `s` seen so far. For each character of `s`, if it equals `t[j-1]` it can extend every way of forming `t[:j-1]`.

```python
def num_distinct(s, t):
    dp = [1] + [0] * len(t)             # one way to form the empty string
    for c in s:
        for j in range(len(t), 0, -1):  # backwards so c is used once per position
            if t[j - 1] == c: dp[j] += dp[j - 1]
    return dp[len(t)]

assert num_distinct("rabbbit", "rabbit") == 3
assert num_distinct("babgbag", "bag") == 5
assert num_distinct("abc", "") == 1
assert num_distinct("", "a") == 0
```

- O(|s|·|t|) time, O(|t|) space.
- 2-D form: `dp[i][j] = dp[i-1][j] + (s[i-1] == t[j-1]) * dp[i-1][j-1]`. Pitfall: iterating `j` forwards reuses the same character twice.

### Burst Balloons (interval DP: choose the last balloon to burst)

**Pattern:** choosing the first balloon to burst makes neighbours change unpredictably; choosing the last balloon `k` in an open interval `(l, r)` fixes its neighbours as `l` and `r`. Pad with 1s at both ends.

```python
def max_coins(nums):
    a = [1] + nums + [1]
    n = len(a)
    dp = [[0] * n for _ in range(n)]    # dp[l][r]: best for balloons strictly between l and r
    for gap in range(2, n):
        for l in range(n - gap):
            r = l + gap
            dp[l][r] = max(dp[l][k] + a[l] * a[k] * a[r] + dp[k][r] for k in range(l + 1, r))
    return dp[0][n - 1]

assert max_coins([3, 1, 5, 8]) == 167
assert max_coins([1, 5]) == 10
assert max_coins([7]) == 7
```

- O(n³) time, O(n²) space.
- Pitfall: fill by increasing interval length so sub-intervals are ready. Same "last operation splits the interval" pattern: Minimum Cost to Cut a Stick, matrix-chain multiplication.

### Regular Expression Matching (dp over string and pattern indices)

**Pattern:** `dp[i][j]` = does `s[i:]` match `p[j:]`. If `p[j+1]` is `*`, either skip `x*` entirely or, if the first character matches, consume one character of `s` and stay on `x*`.

```python
def is_match(s, p):
    m, n = len(s), len(p)
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    dp[m][n] = True
    for i in range(m, -1, -1):
        for j in range(n - 1, -1, -1):
            first = i < m and p[j] in (s[i], ".")
            if j + 1 < n and p[j + 1] == "*":
                dp[i][j] = dp[i][j + 2] or (first and dp[i + 1][j])
            else:
                dp[i][j] = first and dp[i + 1][j + 1]
    return dp[0][0]

assert is_match("aa", "a") is False
assert is_match("aa", "a*") is True
assert is_match("ab", ".*") is True
assert is_match("aab", "c*a*b") is True
assert is_match("mississippi", "mis*is*p*.") is False
assert is_match("", "a*b*") is True
```

- O(m·n) time and space.
- Pitfalls: `i` must run to `m` inclusive (an empty `s` can still match `a*`); `*` applies to the preceding element, not any string (that is Wildcard Matching, a sibling problem).

## Greedy

### Jump Game (track farthest reachable)

**Pattern:** scan left to right keeping the farthest index reachable; if you ever stand beyond it, you are stuck.

```python
def can_jump(nums):
    far = 0
    for i, x in enumerate(nums):
        if i > far: return False
        far = max(far, i + x)
    return True

assert can_jump([2, 3, 1, 1, 4]) is True
assert can_jump([3, 2, 1, 0, 4]) is False
assert can_jump([0]) is True
```

- O(n) time, O(1) space.
- Equivalent backwards greedy: move a `goal` left to any index that can reach it; answer is `goal == 0`. The O(n²) DP is the naive version to mention and discard.

### Jump Game II (BFS by levels over ranges)

**Pattern:** indices reachable with `j` jumps form a contiguous range; the next range ends at the farthest point reachable from the current one. Count range expansions.

```python
def jump(nums):
    jumps, end, far = 0, 0, 0
    for i in range(len(nums) - 1):      # never need to jump from the last index
        far = max(far, i + nums[i])
        if i == end:                    # finished this level
            jumps += 1; end = far
    return jumps

assert jump([2, 3, 1, 1, 4]) == 2
assert jump([2, 3, 0, 1, 4]) == 2
assert jump([0]) == 0
```

- O(n) time, O(1) space.
- Pitfall: looping to `len(nums)` counts an extra jump when `end` lands exactly on the last index.

### Gas Station (greedy: running surplus)

**Pattern:** if total gas < total cost, no start works. Otherwise, whenever the running tank goes negative at station i, no start from the current candidate up to i works, so restart at i + 1.

```python
def can_complete_circuit(gas, cost):
    if sum(gas) < sum(cost): return -1
    start, tank = 0, 0
    for i, (g, c) in enumerate(zip(gas, cost)):
        tank += g - c
        if tank < 0: start, tank = i + 1, 0
    return start

assert can_complete_circuit([1, 2, 3, 4, 5], [3, 4, 5, 1, 2]) == 3
assert can_complete_circuit([2, 3, 4], [3, 4, 3]) == -1
assert can_complete_circuit([5], [4]) == 0
```

- O(n) time, O(1) space.
- The proof is the interview signal: if starting at s fails at i, any start between s and i arrives at i with less fuel (it skips a non-negative prefix), so it fails too.

### Hand of Straights (count map; start groups at the smallest card)

**Pattern:** the smallest remaining card must start a group, so consume `c` copies each of `x, x+1, ..., x+W-1` where `c` is the count of the smallest card `x`.

```python
from collections import Counter
def is_n_straight_hand(hand, w):
    if len(hand) % w: return False
    cnt = Counter(hand)
    for x in sorted(cnt):
        c = cnt[x]
        if c == 0: continue
        for y in range(x, x + w):
            if cnt[y] < c: return False
            cnt[y] -= c
    return True

assert is_n_straight_hand([1, 2, 3, 6, 2, 3, 4, 7, 8], 3) is True
assert is_n_straight_hand([1, 2, 3, 4, 5], 4) is False
assert is_n_straight_hand([1, 1, 2, 2, 3, 3], 3) is True
```

- O(n log n + n·W) time worst case (O(n log n) when grouping counts as above), O(n) space.
- Identical problem: Divide Array in Sets of K Consecutive Numbers. A min-heap of distinct values works too.

### Merge Triplets to Form Target Triplet (keep only triplets that never exceed the target)

**Pattern:** merging takes element-wise maxima, so any triplet with a coordinate above the target is poison. Among the safe triplets, check that each target coordinate is hit exactly by at least one of them.

```python
def merge_triplets(triplets, target):
    hit = set()
    for t in triplets:
        if all(a <= b for a, b in zip(t, target)):
            hit.update(i for i in range(3) if t[i] == target[i])
    return len(hit) == 3

assert merge_triplets([[2, 5, 3], [1, 8, 4], [1, 7, 5]], [2, 7, 5]) is True
assert merge_triplets([[3, 4, 5], [4, 5, 6]], [3, 2, 5]) is False
assert merge_triplets([[2, 5, 3], [2, 3, 4], [1, 2, 5], [5, 2, 3]], [5, 5, 5]) is True
```

- O(n) time, O(1) space.
- Explain why order does not matter: max is commutative and associative, so merging all safe triplets is the best you can do.

### Partition Labels (last occurrence of each character)

**Pattern:** a part must extend to the last occurrence of every character inside it. Scan, extending `end` to each character's last index; when `i == end`, close the part.

```python
def partition_labels(s):
    last = {c: i for i, c in enumerate(s)}
    res, start, end = [], 0, 0
    for i, c in enumerate(s):
        end = max(end, last[c])
        if i == end:
            res.append(end - start + 1); start = i + 1
    return res

assert partition_labels("ababcbacadefegdehijhklij") == [9, 7, 8]
assert partition_labels("eccbbbbdec") == [10]
assert partition_labels("abc") == [1, 1, 1]
```

- O(n) time, O(alphabet) space.
- This is interval merging in disguise: each character defines the interval [first, last].

### Valid Parenthesis String (track range of open counts)

**Pattern:** `*` makes the number of unmatched `(` uncertain, so track its possible range `[lo, hi]`. `)` lowers both; `*` widens it. Fail if `hi < 0`; clamp `lo` at 0; succeed if `lo == 0` at the end.

```python
def check_valid_string(s):
    lo = hi = 0
    for c in s:
        if c == "(": lo += 1; hi += 1
        elif c == ")": lo -= 1; hi -= 1
        else: lo -= 1; hi += 1          # '*' as ')', or as '('
        if hi < 0: return False         # too many ')' even if every '*' is '('
        lo = max(lo, 0)
    return lo == 0

assert check_valid_string("()") is True
assert check_valid_string("(*)") is True
assert check_valid_string("(*))") is True
assert check_valid_string(")(") is False
assert check_valid_string("((*") is False
```

- O(n) time, O(1) space.
- Alternatives: two stacks of indices (`(` and `*`), matching leftover `(` with later `*`, O(n) space; or an O(n³) interval DP. Say the stack version if the range argument does not come quickly.

## Intervals

### Minimum Interval to Include Each Query (sort queries; min-heap by interval size)

**Pattern:** process queries in increasing order. Push every interval that has started (`left <= q`) into a min-heap keyed by size; pop intervals that have ended (`right < q`). The heap top is the answer.

```python
import heapq
def min_interval(intervals, queries):
    intervals = sorted(intervals)
    res, h, i = {}, [], 0
    for q in sorted(set(queries)):
        while i < len(intervals) and intervals[i][0] <= q:
            l, r = intervals[i]; heapq.heappush(h, (r - l + 1, r)); i += 1
        while h and h[0][1] < q: heapq.heappop(h)     # lazy deletion of ended intervals
        res[q] = h[0][0] if h else -1
    return [res[q] for q in queries]    # restore the original query order

assert min_interval([[1, 4], [2, 4], [3, 6], [4, 4]], [2, 3, 4, 5]) == [3, 3, 1, 4]
assert min_interval([[2, 3], [2, 5], [1, 8], [20, 25]], [2, 19, 5, 22]) == [2, -1, 4, 6]
```

- O((n + q) log n + q log q) time, O(n + q) space.
- Pitfalls: answering in sorted order but returning in original order; lazy deletion is safe because an interval that ended before q has ended for all later queries too.

## Math & geometry

### Happy Number (cycle detection on digit-square sums)

**Pattern:** repeatedly replacing n with the sum of the squares of its digits either reaches 1 or enters a cycle. Detect the cycle with a set or Floyd's slow/fast pointers.

```python
def is_happy(n):
    def step(x): return sum(int(d) ** 2 for d in str(x))
    slow, fast = n, step(n)
    while fast != 1 and slow != fast:
        slow, fast = step(slow), step(step(fast))
    return fast == 1

assert is_happy(19) is True
assert is_happy(2) is False
assert is_happy(1) is True
```

- O(log n) per step; the sequence quickly drops below 243 (for any number with 3+ digits the next value is smaller), so the total is O(log n). O(1) space with Floyd.
- Floyd here is the same as linked-list cycle detection; a `seen` set is the simpler first answer.

### Multiply Strings (grade-school multiplication into a digit array)

**Pattern:** digit `i` of `a` times digit `j` of `b` contributes to positions `i + j` (carry) and `i + j + 1` in a result array of length `m + n`.

```python
def multiply(a, b):
    if a == "0" or b == "0": return "0"
    res = [0] * (len(a) + len(b))
    for i in range(len(a) - 1, -1, -1):
        for j in range(len(b) - 1, -1, -1):
            total = int(a[i]) * int(b[j]) + res[i + j + 1]
            res[i + j + 1] = total % 10
            res[i + j] += total // 10
    return "".join(map(str, res)).lstrip("0")

assert multiply("2", "3") == "6"
assert multiply("123", "456") == "56088"
assert multiply("0", "52") == "0"
assert multiply("999", "999") == str(999 * 999)
```

- O(m·n) time, O(m + n) space.
- Pitfall: the problem forbids converting to int; Python's big ints would trivialise it. Follow-up: Karatsuba is O(n^1.58); FFT for huge numbers (name only).

### Detect Squares (count points; enumerate diagonal corners)

**Pattern:** store point counts. For a query point, every stored point that is diagonal to it (same distance in x and y, non-zero) fixes a square; multiply the counts of the other two corners.

```python
from collections import Counter

class DetectSquares:
    def __init__(self):
        self.cnt = Counter()

    def add(self, point):
        self.cnt[tuple(point)] += 1

    def count(self, point):
        x, y = point; res = 0
        for (px, py), c in self.cnt.items():
            if px != x and abs(px - x) == abs(py - y):          # diagonal corner
                res += c * self.cnt[(x, py)] * self.cnt[(px, y)]  # Counter reads do not insert
        return res

ds = DetectSquares()
for p in ([3, 10], [11, 2], [3, 2]): ds.add(p)
assert ds.count([11, 10]) == 1
assert ds.count([14, 8]) == 0
ds.add([11, 2])                         # duplicates count separately
assert ds.count([11, 10]) == 2
```

- `add`: O(1). `count`: O(P) for P distinct points. Space O(P).
- Pitfalls: duplicate points multiply the count; zero-area squares must be excluded (`px != x`). Optimisation: index points by x so only the query's column is scanned.

## Bit manipulation

### Reverse Bits

**Pattern:** 32 times, shift the result left and append the input's lowest bit, then shift the input right.

```python
def reverse_bits(n):
    res = 0
    for _ in range(32):
        res = (res << 1) | (n & 1)
        n >>= 1
    return res

assert reverse_bits(0b00000010100101000001111010011100) == 964176192
assert reverse_bits(0b11111111111111111111111111111101) == 3221225471
assert reverse_bits(0) == 0
```

- O(32) = O(1) time and space.
- Follow-up "called many times": cache the reversal of each byte (256-entry table) and combine four lookups, or use divide-and-conquer masks (swap halves, then quarters, and so on).

### Missing Number (XOR or Gauss sum)

**Pattern:** XOR every index 0..n with every value; pairs cancel and only the missing number remains. Equivalently, `n(n+1)/2 - sum(nums)`.

```python
def missing_number(nums):
    res = len(nums)
    for i, x in enumerate(nums): res ^= i ^ x
    return res

def missing_number_sum(nums):
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)

for f in (missing_number, missing_number_sum):
    assert f([3, 0, 1]) == 2
    assert f([0, 1]) == 2
    assert f([9, 6, 4, 2, 3, 5, 7, 0, 1]) == 8
```

- O(n) time, O(1) space for both.
- The sum version can overflow in fixed-width languages; XOR cannot. Sorting or a set also work but cost O(n log n) or O(n) space.

### Sum of Two Integers (XOR plus carry, masked to 32 bits)

**Pattern:** `a ^ b` adds without carries; `(a & b) << 1` is the carry. Repeat until there is no carry. Python ints are unbounded, so mask to 32 bits and convert back to signed at the end.

```python
def get_sum(a, b):
    MASK, MAX = 0xFFFFFFFF, 0x7FFFFFFF
    while b & MASK:
        a, b = (a ^ b) & MASK, ((a & b) << 1) & MASK
    a &= MASK
    return a if a <= MAX else ~(a ^ MASK)   # reinterpret as signed 32-bit

assert get_sum(1, 2) == 3
assert get_sum(2, 3) == 5
assert get_sum(-1, 1) == 0
assert get_sum(-2, -3) == -5
assert get_sum(-12, 0) == -12
```

- O(32) iterations at most, O(1) space.
- Pitfall: without the mask, negative numbers loop forever in Python. `~(a ^ MASK)` maps an unsigned 32-bit value with the top bit set to its negative equivalent.

### Reverse Integer (digit by digit with 32-bit overflow check)

**Pattern:** pop digits with `divmod` and push them onto the result, checking for overflow before each push, as if you only had 32-bit integers.

```python
def reverse(x):
    INT_MAX = 2**31 - 1
    sign, x, res = (-1 if x < 0 else 1), abs(x), 0
    while x:
        x, d = divmod(x, 10)
        if res > (INT_MAX - d) // 10: return 0     # res * 10 + d would overflow
        res = res * 10 + d
    return sign * res

assert reverse(123) == 321
assert reverse(-123) == -321
assert reverse(120) == 21
assert reverse(1534236469) == 0
assert reverse(-2147483648) == 0
```

- O(log₁₀ x) time, O(1) space.
- Pitfalls: Python's `%` and `//` floor on negatives, so take `abs` first; the check must happen before the multiply. The `str(x)[::-1]` approach is fine to mention but dodges the point of the question.

# 02 — Coding: DS&A Topic Model Answers

Questions: [README.md](README.md) (section B, "Topic checklist (classic DS&A)")

**How to use these answers:** these are reference solutions, not scripts to memorise. For each topic, learn the pattern first, then practise saying the approach, complexity and edge cases out loud before you type. Adapt naming and style to what you would naturally write.

Shared node classes used below:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val, self.next = val, next

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right
```

## Arrays & strings

### Two pointers (container with most water, remove duplicates, trapping rain water)

**Pattern:** two indices that move toward each other (sorted / symmetric problems) or in the same direction (read/write pointer). Each step discards part of the search space with a proof of why it cannot hold the answer. O(n) time, O(1) space.

```python
def max_area(h):                      # container with most water
    i, j, best = 0, len(h) - 1, 0
    while i < j:
        best = max(best, (j - i) * min(h[i], h[j]))
        if h[i] < h[j]: i += 1        # the shorter wall limits area; moving the taller can't help
        else: j -= 1
    return best

def remove_duplicates(nums):          # sorted array, in place, return new length
    w = 0
    for x in nums:
        if w == 0 or x != nums[w - 1]:
            nums[w] = x; w += 1
    return w

def trap(h):                          # trapping rain water
    i, j, lmax, rmax, water = 0, len(h) - 1, 0, 0, 0
    while i < j:
        if h[i] < h[j]:
            lmax = max(lmax, h[i]); water += lmax - h[i]; i += 1
        else:
            rmax = max(rmax, h[j]); water += rmax - h[j]; j -= 1
    return water
```

- All three: O(n) time, O(1) space.
- Trapping rain water insight: water at a bar = min(max left, max right) − height. Process the side with the smaller wall, because its bound is already known.

### Sliding window (longest substring without repeating characters, minimum window substring)

**Pattern:** expand `right` every step; shrink `left` while the window is invalid (or while it is valid, for "minimum" problems). Track state in a hash map/counter. O(n) because each index enters and leaves once.

```python
def length_of_longest_substring(s):
    last, left, best = {}, 0, 0
    for r, c in enumerate(s):
        if c in last and last[c] >= left:
            left = last[c] + 1            # jump past the previous occurrence
        last[c] = r
        best = max(best, r - left + 1)
    return best

from collections import Counter
def min_window(s, t):
    need, missing = Counter(t), len(t)
    left, best = 0, (0, float("inf"))
    for r, c in enumerate(s):
        if need[c] > 0: missing -= 1
        need[c] -= 1
        while missing == 0:               # window valid: try to shrink
            if r - left < best[1] - best[0]:
                best = (left, r)
            need[s[left]] += 1
            if need[s[left]] > 0: missing += 1
            left += 1
    return "" if best[1] == float("inf") else s[best[0]:best[1] + 1]
```

- Longest substring: O(n) time, O(min(n, alphabet)) space.
- Minimum window: O(|s| + |t|) time, O(alphabet) space.

### Prefix sums (subarray sum equals k, range sum query)

**Pattern:** `prefix[i]` = sum of first i elements; sum(i..j) = `prefix[j+1] - prefix[i]`. Combine with a hash map of prefix-sum counts to count subarrays with a target sum (works with negatives, unlike sliding window).

```python
from collections import defaultdict
def subarray_sum(nums, k):
    seen, cur, count = defaultdict(int), 0, 0
    seen[0] = 1                         # empty prefix
    for x in nums:
        cur += x
        count += seen[cur - k]          # earlier prefixes p with cur - p == k
        seen[cur] += 1
    return count

class NumArray:                         # range sum query, immutable
    def __init__(self, nums):
        self.p = [0]
        for x in nums: self.p.append(self.p[-1] + x)
    def sum_range(self, i, j):
        return self.p[j + 1] - self.p[i]
```

- Subarray sum equals k: O(n) time, O(n) space.
- Range sum: O(n) build, O(1) query. If the array is mutable, use a Fenwick tree / segment tree (O(log n) update and query).

### Kadane's (maximum subarray)

**Pattern:** at each index, the best subarray ending here either extends the previous one or starts fresh: `cur = max(x, cur + x)`.

```python
def max_subarray(nums):
    cur = best = nums[0]
    for x in nums[1:]:
        cur = max(x, cur + x)
        best = max(best, cur)
    return best
```

- O(n) time, O(1) space. Handles all-negative arrays because it starts from `nums[0]`.
- Variants: track start/end indices when `cur` resets; maximum product subarray tracks both max and min (a negative flips them).

### String parsing (decode string, string compression, valid parentheses)

**Pattern:** single pass with a stack for nesting, or a read/write pointer for in-place rewrites. Be explicit about multi-digit numbers and edge cases.

```python
def decode_string(s):                  # "3[a2[c]]" -> "accaccacc"
    stack, cur, num = [], "", 0
    for c in s:
        if c.isdigit():
            num = num * 10 + int(c)
        elif c == "[":
            stack.append((cur, num)); cur, num = "", 0
        elif c == "]":
            prev, k = stack.pop(); cur = prev + cur * k
        else:
            cur += c
    return cur

def compress(chars):                    # in place: ["a","a","b"] -> ["a","2","b"], returns length
    w = i = 0
    while i < len(chars):
        j = i
        while j < len(chars) and chars[j] == chars[i]: j += 1
        chars[w] = chars[i]; w += 1
        if j - i > 1:
            for d in str(j - i): chars[w] = d; w += 1
        i = j
    return w

def is_valid(s):                        # valid parentheses
    pairs, stack = {")": "(", "]": "[", "}": "{"}, []
    for c in s:
        if c in pairs:
            if not stack or stack.pop() != pairs[c]: return False
        else:
            stack.append(c)
    return not stack
```

- Decode string: O(output length) time and space (string concatenation can be made linear by keeping lists).
- Compression: O(n) time, O(1) extra space. Valid parentheses: O(n) time, O(n) space.

### Rotate matrix, spiral matrix, set matrix zeroes

```python
def rotate(m):                          # 90 degrees clockwise, in place
    n = len(m)
    for i in range(n):
        for j in range(i + 1, n):
            m[i][j], m[j][i] = m[j][i], m[i][j]   # transpose
    for row in m: row.reverse()                   # mirror horizontally

def spiral_order(m):
    res = []
    top, bot, left, right = 0, len(m) - 1, 0, len(m[0]) - 1
    while top <= bot and left <= right:
        res += [m[top][j] for j in range(left, right + 1)]; top += 1
        res += [m[i][right] for i in range(top, bot + 1)]; right -= 1
        if top <= bot:
            res += [m[bot][j] for j in range(right, left - 1, -1)]; bot -= 1
        if left <= right:
            res += [m[i][left] for i in range(bot, top - 1, -1)]; left += 1
    return res

def set_zeroes(m):                      # O(1) extra space: use row 0 / col 0 as markers
    R, C = len(m), len(m[0])
    row0 = any(m[0][j] == 0 for j in range(C))
    col0 = any(m[i][0] == 0 for i in range(R))
    for i in range(1, R):
        for j in range(1, C):
            if m[i][j] == 0: m[i][0] = m[0][j] = 0
    for i in range(1, R):
        for j in range(1, C):
            if m[i][0] == 0 or m[0][j] == 0: m[i][j] = 0
    if row0:
        for j in range(C): m[0][j] = 0
    if col0:
        for i in range(R): m[i][0] = 0
```

- Rotate: O(n²) time, O(1) space. Spiral: O(R·C) time, O(1) extra. Set zeroes: O(R·C) time, O(1) extra (the simple version with two sets is O(R + C) space — mention it first, then optimise).

### Product of array except self

**Pattern:** prefix product from the left times suffix product from the right, no division (division breaks on zeros).

```python
def product_except_self(nums):
    n = len(nums); out = [1] * n
    left = 1
    for i in range(n):
        out[i] = left; left *= nums[i]
    right = 1
    for i in range(n - 1, -1, -1):
        out[i] *= right; right *= nums[i]
    return out
```

- O(n) time, O(1) extra space (output array not counted).

### Longest common prefix; longest palindromic substring

```python
def longest_common_prefix(strs):
    if not strs: return ""
    lo, hi = min(strs), max(strs)       # lexicographic min/max differ the most
    i = 0
    while i < len(lo) and lo[i] == hi[i]: i += 1
    return lo[:i]

def longest_palindrome(s):              # expand around centre
    best = (0, 0)
    for c in range(len(s)):
        for l, r in ((c, c), (c, c + 1)):          # odd and even length
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1; r += 1
            if r - l - 1 > best[1] - best[0]:
                best = (l + 1, r)
    return s[best[0]:best[1]]
```

- LCP: O(S) total characters compared (O(n·L) worst case), O(1) space. Vertical scanning works equally well.
- Longest palindrome: O(n²) time, O(1) space. Manacher's algorithm is O(n) — name it, rarely needed.

## Hashing

### Frequency maps, two-sum, isomorphic strings

**Pattern:** trade O(n) space for O(1) lookups. "Have I seen the complement?" and "count occurrences" are the two core uses.

```python
from collections import Counter
def is_anagram(a, b):                   # frequency map
    return Counter(a) == Counter(b)

def two_sum(nums, target):
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen: return [seen[target - x], i]
        seen[x] = i
    return []

def is_isomorphic(s, t):                # mapping must be one-to-one both ways
    if len(s) != len(t): return False
    st, ts = {}, {}
    for a, b in zip(s, t):
        if st.setdefault(a, b) != b or ts.setdefault(b, a) != a:
            return False
    return True
```

- All O(n) time, O(n) space (O(alphabet) for character maps).
- Two-sum on a sorted array: two pointers, O(1) space.

### LRU with hash map + doubly linked list

**Pattern:** hash map key → node for O(1) lookup; doubly linked list ordered by recency for O(1) move-to-front and evict-from-tail. Sentinel head/tail nodes remove edge cases.

```python
class _Node:
    __slots__ = ("key", "val", "prev", "next")
    def __init__(self, key=0, val=0):
        self.key, self.val, self.prev, self.next = key, val, None, None

class LRUCache:
    def __init__(self, capacity):
        self.cap, self.map = capacity, {}
        self.head, self.tail = _Node(), _Node()      # head.next = most recent
        self.head.next, self.tail.prev = self.tail, self.head

    def _remove(self, n):
        n.prev.next, n.next.prev = n.next, n.prev

    def _add_front(self, n):
        n.prev, n.next = self.head, self.head.next
        self.head.next.prev = n; self.head.next = n

    def get(self, key):
        if key not in self.map: return -1
        n = self.map[key]; self._remove(n); self._add_front(n)
        return n.val

    def put(self, key, val):
        if key in self.map: self._remove(self.map[key])
        n = self.map[key] = _Node(key, val); self._add_front(n)
        if len(self.map) > self.cap:
            lru = self.tail.prev; self._remove(lru); del self.map[lru.key]
```

- O(1) get/put, O(capacity) space. Mention that in Python `OrderedDict` with `move_to_end` / `popitem(last=False)` does the same in ~10 lines, but interviewers usually want the manual version. Production follow-ups: thread safety (a lock), TTLs, size in bytes rather than count.

### Design a hash map from scratch

**Pattern:** array of buckets, hash → index, separate chaining (list per bucket), resize when load factor exceeds a threshold.

```python
class MyHashMap:
    def __init__(self, cap=8):
        self.buckets = [[] for _ in range(cap)]; self.size = 0

    def _bucket(self, key):
        return self.buckets[hash(key) % len(self.buckets)]

    def put(self, key, val):
        b = self._bucket(key)
        for pair in b:
            if pair[0] == key: pair[1] = val; return
        b.append([key, val]); self.size += 1
        if self.size > 0.75 * len(self.buckets): self._resize()

    def get(self, key, default=-1):
        for k, v in self._bucket(key):
            if k == key: return v
        return default

    def remove(self, key):
        b = self._bucket(key)
        for i, (k, _) in enumerate(b):
            if k == key: b.pop(i); self.size -= 1; return

    def _resize(self):
        old = self.buckets
        self.buckets = [[] for _ in range(2 * len(old))]
        for b in old:
            for k, v in b: self._bucket(k).append([k, v])
```

- Average O(1) per operation, O(n) worst case (all keys collide). Resizing is O(n) but amortised O(1).
- Talking points: chaining vs open addressing (linear probing is cache-friendly, needs tombstones for delete); load factor; good hash functions; why keys must be immutable.

## Linked lists

### Reverse a linked list (iterative/recursive, in k-groups)

**Pattern:** three pointers `prev, cur, nxt`; use a dummy head whenever the head might change.

```python
def reverse_iter(head):
    prev = None
    while head:
        head.next, prev, head = prev, head, head.next
    return prev

def reverse_rec(head):
    if not head or not head.next: return head
    new_head = reverse_rec(head.next)
    head.next.next = head; head.next = None
    return new_head

def reverse_k_group(head, k):
    dummy = ListNode(0, head); group_prev = dummy
    while True:
        kth = group_prev
        for _ in range(k):
            kth = kth.next
            if not kth: return dummy.next          # fewer than k left: leave as is
        group_next = kth.next
        prev, cur = group_next, group_prev.next
        while cur is not group_next:
            cur.next, prev, cur = prev, cur, cur.next
        first = group_prev.next
        group_prev.next = kth; group_prev = first
```

- Iterative: O(n) time, O(1) space. Recursive: O(n) time, O(n) stack. k-group: O(n) time, O(1) space.

### Detect cycle; find cycle start

**Pattern:** Floyd's tortoise and hare. After they meet, reset one pointer to head; moving both one step at a time, they meet at the cycle start (distance head→start equals distance meet→start modulo cycle length).

```python
def detect_cycle(head):                 # returns start node or None
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            slow = head
            while slow is not fast:
                slow, fast = slow.next, fast.next
            return slow
    return None
```

- O(n) time, O(1) space. A visited set also works in O(n) space.

### Merge two / k sorted lists

```python
import heapq
def merge_two(a, b):
    dummy = tail = ListNode()
    while a and b:
        if a.val <= b.val: tail.next, a = a, a.next
        else: tail.next, b = b, b.next
        tail = tail.next
    tail.next = a or b
    return dummy.next

def merge_k(lists):
    heap = [(n.val, i, n) for i, n in enumerate(lists) if n]   # i breaks ties
    heapq.heapify(heap)
    dummy = tail = ListNode()
    while heap:
        _, i, n = heapq.heappop(heap)
        tail.next = tail = n
        if n.next: heapq.heappush(heap, (n.next.val, i, n.next))
    return dummy.next
```

- Merge two: O(n + m) time, O(1) space. Merge k: O(N log k) time, O(k) heap (N = total nodes). Alternative: pairwise divide and conquer, also O(N log k).

### Remove nth node from end; copy list with random pointer

```python
def remove_nth_from_end(head, n):
    dummy = ListNode(0, head); fast = slow = dummy
    for _ in range(n + 1): fast = fast.next   # gap of n nodes
    while fast: fast, slow = fast.next, slow.next
    slow.next = slow.next.next
    return dummy.next

class RNode:
    def __init__(self, val, next=None, random=None):
        self.val, self.next, self.random = val, next, random

def copy_random_list(head):
    old2new = {None: None}
    cur = head
    while cur:
        old2new[cur] = RNode(cur.val); cur = cur.next
    cur = head
    while cur:
        old2new[cur].next = old2new[cur.next]
        old2new[cur].random = old2new[cur.random]
        cur = cur.next
    return old2new[head]
```

- Remove nth: one pass, O(n) time, O(1) space; the dummy handles removing the head.
- Copy random: O(n) time, O(n) map. O(1)-space variant: interleave copies (A→A'→B→B'), set `A'.random = A.random.next`, then unweave.

## Stacks & queues

### Valid parentheses; min stack

Valid parentheses: see `is_valid` under String parsing — push openers, on a closer pop and check it matches; valid iff the stack is empty at the end. O(n) time and space.

```python
class MinStack:                         # store (value, min so far) pairs
    def __init__(self): self.s = []
    def push(self, x):
        self.s.append((x, min(x, self.s[-1][1]) if self.s else x))
    def pop(self): self.s.pop()
    def top(self): return self.s[-1][0]
    def get_min(self): return self.s[-1][1]
```

- All operations O(1); O(n) space. Space optimisation: a second stack that only pushes when the new value is <= the current min.

### Monotonic stack (daily temperatures, next greater element, largest rectangle in histogram)

**Pattern:** keep a stack of indices whose values are monotonic. When a new element breaks the order, pop — and the popped element has just found its "next greater/smaller". Each index is pushed and popped once: O(n).

```python
def daily_temperatures(t):
    res, st = [0] * len(t), []          # stack of indices, decreasing temps
    for i, x in enumerate(t):
        while st and t[st[-1]] < x:
            j = st.pop(); res[j] = i - j
        st.append(i)
    return res

def next_greater_element(nums1, nums2):  # nums1 is a subset of nums2
    nxt, st = {}, []
    for x in nums2:
        while st and st[-1] < x: nxt[st.pop()] = x
        st.append(x)
    return [nxt.get(x, -1) for x in nums1]

def largest_rectangle(heights):
    st, best = [], 0                     # increasing heights
    for i, h in enumerate(heights + [0]):  # sentinel 0 flushes the stack
        while st and heights[st[-1]] >= h:
            height = heights[st.pop()]
            left = st[-1] + 1 if st else 0
            best = max(best, height * (i - left))
        st.append(i)
    return best
```

- All O(n) time, O(n) space. For circular arrays (next greater element II), iterate `2n` times using `i % n`.

### Queue using stacks; sliding-window maximum (deque)

```python
class MyQueue:
    def __init__(self): self.inb, self.out = [], []
    def push(self, x): self.inb.append(x)
    def _shift(self):
        if not self.out:
            while self.inb: self.out.append(self.inb.pop())
    def pop(self): self._shift(); return self.out.pop()
    def peek(self): self._shift(); return self.out[-1]
    def empty(self): return not self.inb and not self.out

from collections import deque
def max_sliding_window(nums, k):
    dq, res = deque(), []               # indices, values decreasing
    for i, x in enumerate(nums):
        while dq and nums[dq[-1]] <= x: dq.pop()
        dq.append(i)
        if dq[0] <= i - k: dq.popleft()  # left element fell out of window
        if i >= k - 1: res.append(nums[dq[0]])
    return res
```

- Queue: amortised O(1) per operation (each element moved at most once), O(n) space.
- Sliding-window max: O(n) time, O(k) space. The heap alternative is O(n log n).

## Trees

### Traversals (in/pre/post/level order), iterative and recursive

**Pattern:** recursive DFS is three lines; iterative DFS uses an explicit stack; level order uses a queue and processes one level per loop.

```python
from collections import deque
def inorder_rec(root):
    return inorder_rec(root.left) + [root.val] + inorder_rec(root.right) if root else []

def inorder_iter(root):
    res, st, cur = [], [], root
    while cur or st:
        while cur: st.append(cur); cur = cur.left   # go left as far as possible
        cur = st.pop(); res.append(cur.val); cur = cur.right
    return res

def preorder_iter(root):
    res, st = [], [root] if root else []
    while st:
        n = st.pop(); res.append(n.val)
        if n.right: st.append(n.right)              # right first so left is popped first
        if n.left: st.append(n.left)
    return res

def postorder_iter(root):              # reverse of (root, right, left)
    res, st = [], [root] if root else []
    while st:
        n = st.pop(); res.append(n.val)
        if n.left: st.append(n.left)
        if n.right: st.append(n.right)
    return res[::-1]

def level_order(root):
    res, q = [], deque([root] if root else [])
    while q:
        level = []
        for _ in range(len(q)):
            n = q.popleft(); level.append(n.val)
            if n.left: q.append(n.left)
            if n.right: q.append(n.right)
        res.append(level)
    return res
```

- All O(n) time, O(h) space for DFS (h = height; O(n) if skewed), O(w) for BFS (w = max width).
- Why iterative matters: Python's default recursion limit (~1000) breaks on deep/skewed trees.

### Max depth, diameter, balanced check

**Pattern:** post-order — compute something from children, combine at the parent, and update a global answer if needed.

```python
def max_depth(root):
    return 1 + max(max_depth(root.left), max_depth(root.right)) if root else 0

def diameter(root):                     # longest path in edges, may not pass the root
    best = 0
    def depth(n):
        nonlocal best
        if not n: return 0
        l, r = depth(n.left), depth(n.right)
        best = max(best, l + r)
        return 1 + max(l, r)
    depth(root)
    return best

def is_balanced(root):                  # height, or -1 for "unbalanced"
    def h(n):
        if not n: return 0
        l, r = h(n.left), h(n.right)
        if l < 0 or r < 0 or abs(l - r) > 1: return -1
        return 1 + max(l, r)
    return h(root) >= 0
```

- All O(n) time, O(h) space. The naive balanced check that calls height at every node is O(n²) — mention and avoid it.

### Validate BST; kth smallest in BST

```python
def is_valid_bst(root, lo=float("-inf"), hi=float("inf")):
    if not root: return True
    if not lo < root.val < hi: return False
    return is_valid_bst(root.left, lo, root.val) and is_valid_bst(root.right, root.val, hi)

def kth_smallest(root, k):             # iterative in-order, stop early
    st, cur = [], root
    while True:
        while cur: st.append(cur); cur = cur.left
        cur = st.pop(); k -= 1
        if k == 0: return cur.val
        cur = cur.right
```

- Validate: O(n) time, O(h) space. Common bug: only comparing a node to its direct children — you must pass bounds down.
- Kth smallest: O(h + k) time, O(h) space. Follow-up (frequent inserts): store subtree sizes in nodes for O(h) queries.

### Lowest common ancestor

```python
def lca(root, p, q):                    # general binary tree
    if not root or root is p or root is q: return root
    l, r = lca(root.left, p, q), lca(root.right, p, q)
    return root if l and r else l or r

def lca_bst(root, p, q):                # BST: use ordering
    while root:
        if p.val < root.val and q.val < root.val: root = root.left
        elif p.val > root.val and q.val > root.val: root = root.right
        else: return root
```

- Binary tree: O(n) time, O(h) space. BST: O(h) time, O(1) space.
- Variant with parent pointers: same as finding the intersection of two linked lists.

### Serialize/deserialize a binary tree

**Pattern:** pre-order with explicit null markers fully determines the tree.

```python
def serialize(root):
    out = []
    def dfs(n):
        if not n: out.append("#"); return
        out.append(str(n.val)); dfs(n.left); dfs(n.right)
    dfs(root)
    return ",".join(out)

def deserialize(data):
    it = iter(data.split(","))
    def build():
        v = next(it)
        if v == "#": return None
        n = TreeNode(int(v)); n.left = build(); n.right = build()
        return n
    return build()
```

- O(n) time and space both ways. BFS (level-order) encoding is the other common choice. Talk about delimiter choice if values are strings.

### Path sum variants

```python
def has_path_sum(root, target):         # root-to-leaf
    if not root: return False
    if not root.left and not root.right: return root.val == target
    return has_path_sum(root.left, target - root.val) or has_path_sum(root.right, target - root.val)

def path_sum_all(root, target):         # list all root-to-leaf paths
    res = []
    def dfs(n, rem, path):
        if not n: return
        path.append(n.val)
        if not n.left and not n.right and rem == n.val: res.append(path[:])
        dfs(n.left, rem - n.val, path); dfs(n.right, rem - n.val, path)
        path.pop()                      # backtrack
    dfs(root, target, [])
    return res

from collections import defaultdict
def path_sum_any(root, target):         # count downward paths starting anywhere (prefix sums)
    seen = defaultdict(int); seen[0] = 1
    def dfs(n, cur):
        if not n: return 0
        cur += n.val
        cnt = seen[cur - target]
        seen[cur] += 1
        cnt += dfs(n.left, cur) + dfs(n.right, cur)
        seen[cur] -= 1                  # undo when leaving this branch
        return cnt
    return dfs(root, 0)

def max_path_sum(root):                 # any node to any node
    best = float("-inf")
    def gain(n):
        nonlocal best
        if not n: return 0
        l, r = max(gain(n.left), 0), max(gain(n.right), 0)
        best = max(best, n.val + l + r)
        return n.val + max(l, r)
    gain(root)
    return best
```

- I: O(n) time, O(h) space. II: O(n·h) time (copying paths). III: O(n) time and space (vs O(n²) brute force). Max path sum: O(n), O(h).

### Trie: implement, word search II, autocomplete

```python
class Trie:
    def __init__(self): self.root = {}
    def insert(self, word):
        n = self.root
        for c in word: n = n.setdefault(c, {})
        n["$"] = word                   # end marker stores the word
    def _node(self, prefix):
        n = self.root
        for c in prefix:
            if c not in n: return None
            n = n[c]
        return n
    def search(self, word):
        n = self._node(word); return n is not None and "$" in n
    def starts_with(self, prefix): return self._node(prefix) is not None
    def autocomplete(self, prefix, limit=10):
        n, res = self._node(prefix), []
        def dfs(n):
            if len(res) >= limit: return
            if "$" in n: res.append(n["$"])
            for c in sorted(k for k in n if k != "$"): dfs(n[c])
        if n: dfs(n)
        return res

def find_words(board, words):           # word search II
    t = Trie()
    for w in words: t.insert(w)
    R, C, found = len(board), len(board[0]), []
    def dfs(i, j, node):
        c = board[i][j]
        nxt = node.get(c)
        if not nxt: return
        if "$" in nxt: found.append(nxt.pop("$"))   # pop to avoid duplicates
        board[i][j] = "#"
        for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            ni, nj = i + di, j + dj
            if 0 <= ni < R and 0 <= nj < C and board[ni][nj] != "#":
                dfs(ni, nj, nxt)
        board[i][j] = c
        if not nxt: node.pop(c)         # prune exhausted branches
    for i in range(R):
        for j in range(C): dfs(i, j, t.root)
    return found
```

- Insert/search/prefix: O(L) per word. Space O(total characters).
- Word search II: O(R·C·4·3^(L−1)) worst case; the shared trie plus pruning is what makes it fast in practice versus running word search I per word.
- Autocomplete: O(L + size of subtree visited). In production, store the top-k completions (by frequency) at each node so lookups are O(L).

## Heaps

### Kth largest; top-K frequent; merge k sorted

**Pattern:** to keep the k largest, use a min-heap of size k (the root is the kth largest). Python's `heapq` is a min-heap; negate values for a max-heap.

```python
import heapq
from collections import Counter
def find_kth_largest(nums, k):
    h = []
    for x in nums:
        heapq.heappush(h, x)
        if len(h) > k: heapq.heappop(h)
    return h[0]

def top_k_frequent(nums, k):            # bucket sort: O(n)
    cnt = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for x, c in cnt.items(): buckets[c].append(x)
    res = []
    for c in range(len(buckets) - 1, 0, -1):
        res += buckets[c]
        if len(res) >= k: return res[:k]
    return res
    # heap alternative: heapq.nlargest(k, cnt, key=cnt.get)  -> O(n log k)

def merge_k_sorted_arrays(arrays):
    h = [(a[0], i, 0) for i, a in enumerate(arrays) if a]
    heapq.heapify(h); out = []
    while h:
        v, i, j = heapq.heappop(h); out.append(v)
        if j + 1 < len(arrays[i]): heapq.heappush(h, (arrays[i][j + 1], i, j + 1))
    return out
```

- Kth largest: O(n log k) time, O(k) space. Quickselect is O(n) average, O(n²) worst.
- Top-K frequent: O(n) with buckets, O(n log k) with a heap.
- Merge k sorted: O(N log k) time, O(k) heap (see `merge_k` under Linked lists for the list version).

### Median from data stream

**Pattern:** two heaps — a max-heap for the lower half, a min-heap for the upper half, sizes differ by at most one.

```python
import heapq
class MedianFinder:
    def __init__(self): self.lo, self.hi = [], []   # lo is a max-heap via negation
    def add_num(self, x):
        heapq.heappush(self.lo, -x)
        heapq.heappush(self.hi, -heapq.heappop(self.lo))  # keep every lo <= every hi
        if len(self.hi) > len(self.lo):
            heapq.heappush(self.lo, -heapq.heappop(self.hi))
    def find_median(self):
        if len(self.lo) > len(self.hi): return -self.lo[0]
        return (-self.lo[0] + self.hi[0]) / 2
```

- add O(log n), median O(1), O(n) space. Follow-ups: values in a small range → counting array; sliding-window median → two heaps with lazy deletion or a sorted list.

### Task scheduler / reorganize string

**Pattern:** greedy — always place the most frequent remaining item that is allowed. A max-heap gives the most frequent; a cooldown queue holds items that are not yet allowed.

```python
import heapq
from collections import Counter
def least_interval(tasks, n):           # math formula
    cnt = Counter(tasks).values()
    mx = max(cnt); n_max = sum(1 for c in cnt if c == mx)
    return max(len(tasks), (mx - 1) * (n + 1) + n_max)

def reorganize_string(s):               # no two adjacent chars equal
    h = [(-c, ch) for ch, c in Counter(s).items()]
    heapq.heapify(h)
    res, prev = [], None                # prev = item on cooldown for one step
    while h:
        c, ch = heapq.heappop(h)
        res.append(ch)
        if prev: heapq.heappush(h, prev)
        prev = (c + 1, ch) if c + 1 < 0 else None
    return "".join(res) if len(res) == len(s) else ""
```

- Task scheduler: O(n) time, O(1) space (26 letters). Explain the formula: the most frequent task forms `mx − 1` frames of length `n + 1`, plus a final partial frame; if there are enough distinct tasks, there is no idle time and the answer is just `len(tasks)`.
- Reorganize string: O(n log A) time (A = alphabet size), O(A) space. Impossible iff the max count > (n + 1) // 2.

## Graphs

### BFS/DFS on grids (islands, rotting oranges, walls & gates)

**Pattern:** treat cells as nodes and the 4 neighbours as edges. DFS/BFS for connectivity; **multi-source BFS** (put all sources in the queue at distance 0) for "time to spread" or "distance to nearest".

```python
from collections import deque
DIRS = ((1, 0), (-1, 0), (0, 1), (0, -1))

def num_islands(grid):                  # grid of "1"/"0"; iterative to avoid recursion limits
    R, C, count = len(grid), len(grid[0]), 0
    for i in range(R):
        for j in range(C):
            if grid[i][j] != "1": continue
            count += 1; grid[i][j] = "0"; st = [(i, j)]
            while st:
                r, c = st.pop()
                for dr, dc in DIRS:
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] == "1":
                        grid[nr][nc] = "0"; st.append((nr, nc))
    return count

def oranges_rotting(grid):              # 0 empty, 1 fresh, 2 rotten
    R, C = len(grid), len(grid[0])
    q = deque((i, j) for i in range(R) for j in range(C) if grid[i][j] == 2)
    fresh = sum(row.count(1) for row in grid); minutes = 0
    while q and fresh:
        for _ in range(len(q)):
            r, c = q.popleft()
            for dr, dc in DIRS:
                nr, nc = r + dr, c + dc
                if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] == 1:
                    grid[nr][nc] = 2; fresh -= 1; q.append((nr, nc))
        minutes += 1
    return -1 if fresh else minutes

def walls_and_gates(rooms):             # -1 wall, 0 gate, INF empty -> distance to nearest gate
    INF = 2**31 - 1; R, C = len(rooms), len(rooms[0])
    q = deque((i, j) for i in range(R) for j in range(C) if rooms[i][j] == 0)
    while q:
        r, c = q.popleft()
        for dr, dc in DIRS:
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C and rooms[nr][nc] == INF:
                rooms[nr][nc] = rooms[r][c] + 1; q.append((nr, nc))
```

- All O(R·C) time and O(R·C) worst-case space. Mark visited when you **enqueue**, not when you dequeue, or cells get added many times.

### Shortest path (BFS unweighted, Dijkstra weighted)

```python
from collections import deque
import heapq
def bfs_shortest(adj, src, dst):        # adj: {node: [neighbours]}
    dist = {src: 0}; q = deque([src])
    while q:
        u = q.popleft()
        if u == dst: return dist[u]
        for v in adj.get(u, []):
            if v not in dist: dist[v] = dist[u] + 1; q.append(v)
    return -1

def dijkstra(adj, src):                 # adj: {u: [(v, w)]}, w >= 0
    dist = {src: 0}; pq = [(0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist.get(u, float("inf")): continue   # stale entry
        for v, w in adj.get(u, []):
            nd = d + w
            if nd < dist.get(v, float("inf")):
                dist[v] = nd; heapq.heappush(pq, (nd, v))
    return dist
```

- BFS: O(V + E). Dijkstra: O((V + E) log V) with a binary heap.
- Know when each fails: negative weights → Bellman-Ford O(V·E); 0/1 weights → 0-1 BFS with a deque; all-pairs on small graphs → Floyd-Warshall O(V³); a good heuristic → A*.

### Topological sort (course schedule, build order, alien dictionary)

**Pattern:** Kahn's algorithm — repeatedly take nodes with in-degree 0. If you cannot output all nodes, there is a cycle.

```python
from collections import deque, defaultdict
def topo_sort(n, edges):                # edges: (a, b) means a before b; returns order or []
    adj, indeg = defaultdict(list), [0] * n
    for a, b in edges: adj[a].append(b); indeg[b] += 1
    q = deque(i for i in range(n) if indeg[i] == 0); order = []
    while q:
        u = q.popleft(); order.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0: q.append(v)
    return order if len(order) == n else []

def can_finish(num_courses, prerequisites):   # course schedule: [course, prereq]
    return len(topo_sort(num_courses, [(p, c) for c, p in prerequisites])) == num_courses
# build order is the same as course schedule II: return topo_sort(...) itself.

def alien_order(words):
    adj = {c: set() for w in words for c in w}
    indeg = {c: 0 for c in adj}
    for w1, w2 in zip(words, words[1:]):
        for a, b in zip(w1, w2):
            if a != b:
                if b not in adj[a]: adj[a].add(b); indeg[b] += 1
                break
        else:
            if len(w1) > len(w2): return ""   # "abc" before "ab" is invalid
    q = deque(c for c in indeg if indeg[c] == 0); out = []
    while q:
        c = q.popleft(); out.append(c)
        for d in adj[c]:
            indeg[d] -= 1
            if indeg[d] == 0: q.append(d)
    return "".join(out) if len(out) == len(indeg) else ""
```

- O(V + E) time and space. Alien dictionary: O(total characters) to build the graph. DFS with three colours (unvisited/visiting/done) is the alternative; reverse post-order is a valid topological order.

### Union-find (number of connected components, redundant connection, accounts merge)

**Pattern:** disjoint-set union with path compression and union by rank/size: near-O(1) amortised (inverse Ackermann) per operation.

```python
class DSU:
    def __init__(self, n): self.p, self.sz = list(range(n)), [1] * n
    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]   # path halving
            x = self.p[x]
        return x
    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb: return False
        if self.sz[ra] < self.sz[rb]: ra, rb = rb, ra
        self.p[rb] = ra; self.sz[ra] += self.sz[rb]
        return True

def count_components(n, edges):
    d = DSU(n)
    return n - sum(d.union(a, b) for a, b in edges)

def find_redundant_connection(edges):   # nodes 1..n; first edge that closes a cycle
    d = DSU(len(edges) + 1)
    for a, b in edges:
        if not d.union(a, b): return [a, b]

def accounts_merge(accounts):           # [name, email1, email2, ...]
    d, email_id, owner = DSU(sum(len(a) - 1 for a in accounts)), {}, {}
    for acc in accounts:
        for e in acc[1:]:
            if e not in email_id: email_id[e] = len(email_id); owner[e] = acc[0]
            d.union(email_id[acc[1]], email_id[e])
    groups = defaultdict(list)
    for e, i in email_id.items(): groups[d.find(i)].append(e)
    return [[owner[es[0]]] + sorted(es) for es in groups.values()]
```

- All roughly O(E·α(V)) time, O(V) space; accounts merge adds O(N log N) for sorting emails.
- Use union-find when edges arrive incrementally or you only need connectivity; use BFS/DFS when you need paths.

### Clone graph; bipartite check

```python
class GNode:
    def __init__(self, val=0, neighbors=None):
        self.val, self.neighbors = val, neighbors or []

def clone_graph(node):
    if not node: return None
    copies = {node: GNode(node.val)}; st = [node]
    while st:
        u = st.pop()
        for v in u.neighbors:
            if v not in copies: copies[v] = GNode(v.val); st.append(v)
            copies[u].neighbors.append(copies[v])
    return copies[node]

def is_bipartite(graph):                # adjacency list, may be disconnected
    color = {}
    for s in range(len(graph)):
        if s in color: continue
        color[s] = 0; q = deque([s])
        while q:
            u = q.popleft()
            for v in graph[u]:
                if v not in color: color[v] = color[u] ^ 1; q.append(v)
                elif color[v] == color[u]: return False
    return True
```

- Both O(V + E) time, O(V) space. Bipartite ⇔ 2-colourable ⇔ no odd-length cycle. Remember to loop over every start node for disconnected graphs.

### Minimum spanning tree (Kruskal/Prim) — less common

```python
def kruskal(n, edges):                  # edges: (w, u, v); returns total weight
    d, total, used = DSU(n), 0, 0
    for w, u, v in sorted(edges):
        if d.union(u, v):
            total += w; used += 1
            if used == n - 1: break
    return total if used == n - 1 else -1   # -1: graph not connected

def prim(n, adj):                       # adj: {u: [(w, v)]}
    seen, total, pq = set(), 0, [(0, 0)]
    while pq and len(seen) < n:
        w, u = heapq.heappop(pq)
        if u in seen: continue
        seen.add(u); total += w
        for nw, v in adj.get(u, []):
            if v not in seen: heapq.heappush(pq, (nw, v))
    return total if len(seen) == n else -1
```

- Kruskal: O(E log E), good for sparse graphs given as an edge list. Prim: O(E log V), good with adjacency lists / dense graphs. Typical framing: "connect all points with minimum cable cost".

### Network delay time; cheapest flights within k stops

```python
def network_delay_time(times, n, k):    # times: (u, v, w), nodes 1..n
    adj = defaultdict(list)
    for u, v, w in times: adj[u].append((v, w))
    dist = dijkstra(adj, k)
    return max(dist.values()) if len(dist) == n else -1

def find_cheapest_price(n, flights, src, dst, k):   # Bellman-Ford, k+1 rounds
    cost = [float("inf")] * n; cost[src] = 0
    for _ in range(k + 1):
        nxt = cost[:]                   # copy so each round adds at most one edge
        for u, v, p in flights:
            if cost[u] + p < nxt[v]: nxt[v] = cost[u] + p
        cost = nxt
    return -1 if cost[dst] == float("inf") else cost[dst]
```

- Network delay: Dijkstra, O((V + E) log V); answer is the max shortest distance.
- Cheapest flights: plain Dijkstra fails because the cheapest path may use too many stops. Bounded Bellman-Ford is O(k·E) time, O(V) space; BFS by levels or Dijkstra on (cost, node, stops) state also work.

## Intervals

### Merge intervals, insert interval, non-overlapping intervals

**Pattern:** sort by start (or by end for greedy selection), then sweep once comparing with the last kept interval.

```python
def merge(intervals):
    res = []
    for s, e in sorted(intervals):
        if res and s <= res[-1][1]: res[-1][1] = max(res[-1][1], e)
        else: res.append([s, e])
    return res

def insert(intervals, new):             # intervals already sorted and disjoint
    res, i, n = [], 0, len(intervals)
    s, e = new
    while i < n and intervals[i][1] < s: res.append(intervals[i]); i += 1
    while i < n and intervals[i][0] <= e:
        s, e = min(s, intervals[i][0]), max(e, intervals[i][1]); i += 1
    res.append([s, e])
    return res + intervals[i:]

def erase_overlap_intervals(intervals): # min removals so the rest don't overlap
    removed, end = 0, float("-inf")
    for s, e in sorted(intervals, key=lambda x: x[1]):   # earliest end first
        if s >= end: end = e
        else: removed += 1
    return removed
```

- Merge: O(n log n) time, O(n) output. Insert: O(n). Non-overlapping: O(n log n) — greedy by earliest end keeps the most intervals (classic activity selection; prove by exchange argument).
- Clarify whether touching intervals ([1,2],[2,3]) count as overlapping.

### Meeting rooms II (min rooms), employee free time

```python
import heapq
def min_meeting_rooms(intervals):
    ends = []                           # min-heap of end times of rooms in use
    for s, e in sorted(intervals):
        if ends and ends[0] <= s: heapq.heapreplace(ends, e)   # reuse a freed room
        else: heapq.heappush(ends, e)
    return len(ends)

def employee_free_time(schedules):      # list of lists of [s, e], each sorted
    all_iv = sorted(iv for emp in schedules for iv in emp)
    res, end = [], all_iv[0][1]
    for s, e in all_iv[1:]:
        if s > end: res.append([end, s])
        end = max(end, e)
    return res
```

- Meeting rooms II: O(n log n) time, O(n) space. Alternative sweep line: sort starts and ends separately, +1 on start, −1 on end, track the max.
- Employee free time: O(N log N) with a flat sort; O(N log k) if you merge the k sorted schedules with a heap.

## Binary search

### Classic, first/last position, search in rotated array

**Template:** find the first index where a monotonic predicate becomes true. Use half-open `[lo, hi)`; this one template covers most variants and avoids off-by-one bugs.

```python
def first_true(lo, hi, pred):           # smallest x in [lo, hi) with pred(x), else hi
    while lo < hi:
        mid = (lo + hi) // 2
        if pred(mid): hi = mid
        else: lo = mid + 1
    return lo

def search(nums, target):               # classic
    i = first_true(0, len(nums), lambda m: nums[m] >= target)
    return i if i < len(nums) and nums[i] == target else -1

def search_range(nums, target):         # first and last position
    l = first_true(0, len(nums), lambda m: nums[m] >= target)
    if l == len(nums) or nums[l] != target: return [-1, -1]
    r = first_true(0, len(nums), lambda m: nums[m] > target) - 1
    return [l, r]

def search_rotated(nums, target):       # distinct values
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target: return mid
        if nums[lo] <= nums[mid]:       # left half sorted
            if nums[lo] <= target < nums[mid]: hi = mid - 1
            else: lo = mid + 1
        else:                           # right half sorted
            if nums[mid] < target <= nums[hi]: lo = mid + 1
            else: hi = mid - 1
    return -1
```

- All O(log n) time, O(1) space. Python's `bisect_left` / `bisect_right` give first/last position directly.
- Rotated with duplicates: worst case O(n) (when `nums[lo] == nums[mid] == nums[hi]`, shrink both ends).

### Binary search on the answer (koko eating bananas, capacity to ship packages)

**Pattern:** the answer lies in a range and "is X feasible?" is monotonic. Binary-search X, with an O(n) feasibility check.

```python
def min_eating_speed(piles, h):
    hours = lambda k: sum((p + k - 1) // k for p in piles)   # ceil division
    return first_true(1, max(piles) + 1, lambda k: hours(k) <= h)

def ship_within_days(weights, days):
    def ok(cap):
        need, load = 1, 0
        for w in weights:
            if load + w > cap: need += 1; load = 0
            load += w
        return need <= days
    return first_true(max(weights), sum(weights) + 1, ok)
```

- Koko: O(n log M) where M = max pile. Shipping: O(n log S) where S = sum of weights. Both O(1) extra space.
- Spot it from phrasing like "minimum maximum", "smallest capacity such that", "at most D days". Same pattern: split array largest sum, minimum days to make bouquets.

### Find peak element; median of two sorted arrays (hard)

```python
def find_peak_element(nums):            # any i with nums[i] > neighbours
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] < nums[mid + 1]: lo = mid + 1   # uphill to the right: a peak exists there
        else: hi = mid
    return lo

def find_median_sorted_arrays(a, b):
    if len(a) > len(b): a, b = b, a     # binary search the shorter array
    m, n = len(a), len(b); half = (m + n + 1) // 2
    lo, hi = 0, m
    while lo <= hi:
        i = (lo + hi) // 2; j = half - i          # i from a, j from b in the left half
        a_l = a[i - 1] if i > 0 else float("-inf")
        a_r = a[i] if i < m else float("inf")
        b_l = b[j - 1] if j > 0 else float("-inf")
        b_r = b[j] if j < n else float("inf")
        if a_l <= b_r and b_l <= a_r:             # valid partition
            if (m + n) % 2: return max(a_l, b_l)
            return (max(a_l, b_l) + min(a_r, b_r)) / 2
        if a_l > b_r: hi = i - 1
        else: lo = i + 1
```

- Peak: O(log n), O(1). Works because adjacent elements differ and the ends act as −∞.
- Median of two sorted arrays: O(log min(m, n)) time, O(1) space. Explain the idea first: choose a cut in each array so the left halves together hold half the elements and every left element ≤ every right element. Mention the simpler O(m + n) merge first.

## Recursion & backtracking

### Subsets, permutations, combinations, combination sum

**Template:**

```text
def backtrack(state, choices):
    if is_solution(state): record(copy of state)
    for choice in choices:
        if not valid(choice): continue
        make(choice); backtrack(...); undo(choice)
```

```python
def subsets(nums):
    res, path = [], []
    def bt(i):
        if i == len(nums): res.append(path[:]); return
        bt(i + 1)                          # skip nums[i]
        path.append(nums[i]); bt(i + 1); path.pop()   # take nums[i]
    bt(0)
    return res

def permutations(nums):
    res, path, used = [], [], [False] * len(nums)
    def bt():
        if len(path) == len(nums): res.append(path[:]); return
        for i, x in enumerate(nums):
            if used[i]: continue
            used[i] = True; path.append(x); bt(); path.pop(); used[i] = False
    bt()
    return res

def combine(n, k):                      # all k-combinations of 1..n
    res, path = [], []
    def bt(start):
        if len(path) == k: res.append(path[:]); return
        for x in range(start, n - (k - len(path)) + 2):   # prune: enough numbers left
            path.append(x); bt(x + 1); path.pop()
    bt(1)
    return res

def combination_sum(cands, target):     # reuse allowed
    cands.sort(); res, path = [], []
    def bt(start, rem):
        if rem == 0: res.append(path[:]); return
        for i in range(start, len(cands)):
            if cands[i] > rem: break    # sorted, so prune the rest
            path.append(cands[i]); bt(i, rem - cands[i]); path.pop()  # i, not i+1: reuse
    bt(0, target)
    return res
```

- Subsets: O(n·2ⁿ). Permutations: O(n·n!). Combinations: O(k·C(n, k)). Combination sum: exponential, bounded by O(N^(T/min)). Space O(n) recursion depth plus output.
- Duplicates in the input (subsets II, permutations II, combination sum II): sort, then skip `nums[i] == nums[i-1]` at the same recursion level.

### N-Queens, Sudoku solver

```python
def solve_n_queens(n):
    res, cols, d1, d2, board = [], set(), set(), set(), []
    def bt(r):
        if r == n:
            res.append(["." * c + "Q" + "." * (n - c - 1) for c in board]); return
        for c in range(n):
            if c in cols or r - c in d1 or r + c in d2: continue
            cols.add(c); d1.add(r - c); d2.add(r + c); board.append(c)
            bt(r + 1)
            cols.remove(c); d1.remove(r - c); d2.remove(r + c); board.pop()
    bt(0)
    return res

def solve_sudoku(b):                    # 9x9 list of lists, "." for empty; in place
    rows = [set() for _ in range(9)]; cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]; empty = []
    for r in range(9):
        for c in range(9):
            v = b[r][c]
            if v == ".": empty.append((r, c))
            else: rows[r].add(v); cols[c].add(v); boxes[r // 3 * 3 + c // 3].add(v)
    def bt(k):
        if k == len(empty): return True
        r, c = empty[k]; bx = r // 3 * 3 + c // 3
        for v in "123456789":
            if v in rows[r] or v in cols[c] or v in boxes[bx]: continue
            b[r][c] = v; rows[r].add(v); cols[c].add(v); boxes[bx].add(v)
            if bt(k + 1): return True
            b[r][c] = "."; rows[r].remove(v); cols[c].remove(v); boxes[bx].remove(v)
        return False
    bt(0)
```

- N-Queens: O(n!) time, O(n) space (one queen per row; diagonals identified by `r − c` and `r + c`).
- Sudoku: O(9^m) worst case for m empty cells, O(1) space for the fixed board. Speed-up: always fill the cell with the fewest candidates first (MRV heuristic), or use bitmasks.

### Generate parentheses; letter combinations of a phone number

```python
def generate_parenthesis(n):
    res = []
    def bt(s, open_, close):
        if len(s) == 2 * n: res.append(s); return
        if open_ < n: bt(s + "(", open_ + 1, close)
        if close < open_: bt(s + ")", open_, close + 1)   # never close more than opened
    bt("", 0, 0)
    return res

def letter_combinations(digits):
    if not digits: return []
    m = {"2": "abc", "3": "def", "4": "ghi", "5": "jkl",
         "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"}
    res = [""]
    for d in digits:
        res = [p + c for p in res for c in m[d]]
    return res
```

- Generate parentheses: O(4ⁿ/√n) results (Catalan number), each of length 2n.
- Letter combinations: O(4ⁿ·n) time and output space. The iterative version shown is equivalent to backtracking.

## Dynamic programming

**General approach (say this out loud):** (1) define the state — what `dp[i]` means in words; (2) write the recurrence; (3) base cases; (4) iteration order; (5) answer location; (6) optimise space if each row depends only on the previous one. Start from the brute-force recursion, add `@lru_cache` (top-down), then convert to bottom-up if asked.

### Climbing stairs, house robber, coin change

```python
def climb_stairs(n):                    # dp[i] = dp[i-1] + dp[i-2]
    a, b = 1, 1
    for _ in range(n): a, b = b, a + b
    return a

def rob(nums):                          # best = max(skip this house, take it + best two back)
    prev2 = prev1 = 0
    for x in nums: prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1

def coin_change(coins, amount):         # fewest coins; unbounded
    INF = amount + 1
    dp = [0] + [INF] * amount
    for a in range(1, amount + 1):
        for c in coins:
            if c <= a: dp[a] = min(dp[a], dp[a - c] + 1)
    return dp[amount] if dp[amount] != INF else -1
```

- Climbing stairs, house robber: O(n) time, O(1) space. Coin change: O(amount·#coins) time, O(amount) space.
- Variants: house robber II (circular) = max(rob(nums[1:]), rob(nums[:-1])); coin change II (number of ways) loops coins outer, amounts inner, so each combination counts once.

### Longest increasing subsequence

```python
from bisect import bisect_left
def length_of_lis(nums):
    tails = []                          # tails[k] = smallest tail of an increasing subsequence of length k+1
    for x in nums:
        i = bisect_left(tails, x)
        if i == len(tails): tails.append(x)
        else: tails[i] = x
    return len(tails)
```

- O(n log n) time, O(n) space. Mention the O(n²) DP first: `dp[i] = 1 + max(dp[j] for j < i if nums[j] < nums[i])`. `tails` is not itself an LIS; keep parent pointers if you must reconstruct it.

### Longest common subsequence, edit distance

```python
def lcs(a, b):
    dp = [[0] * (len(b) + 1) for _ in range(len(a) + 1)]
    for i in range(1, len(a) + 1):
        for j in range(1, len(b) + 1):
            dp[i][j] = dp[i-1][j-1] + 1 if a[i-1] == b[j-1] else max(dp[i-1][j], dp[i][j-1])
    return dp[-1][-1]

def edit_distance(a, b):                # insert / delete / replace, cost 1 each
    prev = list(range(len(b) + 1))      # row for the empty prefix of a
    for i in range(1, len(a) + 1):
        cur = [i] + [0] * len(b)
        for j in range(1, len(b) + 1):
            if a[i-1] == b[j-1]: cur[j] = prev[j-1]
            else: cur[j] = 1 + min(prev[j],     # delete from a
                                   cur[j-1],    # insert into a
                                   prev[j-1])   # replace
        prev = cur
    return prev[-1]
```

- Both O(m·n) time; LCS shown with O(m·n) space, edit distance with O(n) rolling rows. Real-world tie-in: diff tools, fuzzy matching of customer records, spell-check.

### Unique paths, minimum path sum

```python
def unique_paths(m, n):                 # only right/down moves
    row = [1] * n
    for _ in range(1, m):
        for j in range(1, n): row[j] += row[j - 1]
    return row[-1]

def min_path_sum(grid):
    R, C = len(grid), len(grid[0]); dp = [float("inf")] * C; dp[0] = 0
    for i in range(R):
        dp[0] += grid[i][0]
        for j in range(1, C): dp[j] = min(dp[j], dp[j - 1]) + grid[i][j]
    return dp[-1]
```

- Both O(R·C) time, O(C) space. Unique paths also has a closed form: C(m + n − 2, m − 1) (`math.comb`). With obstacles, set `dp[j] = 0` on blocked cells.

### Word break; decode ways

```python
def word_break(s, words):
    ws, max_len = set(words), max(map(len, words), default=0)
    dp = [True] + [False] * len(s)      # dp[i]: s[:i] can be segmented
    for i in range(1, len(s) + 1):
        for j in range(max(0, i - max_len), i):
            if dp[j] and s[j:i] in ws: dp[i] = True; break
    return dp[-1]

def num_decodings(s):                   # "12" -> 2 ("AB", "L")
    prev2, prev1 = 1, 1 if s and s[0] != "0" else 0   # dp[i-2], dp[i-1]
    for i in range(1, len(s)):
        cur = prev1 if s[i] != "0" else 0
        if 10 <= int(s[i-1:i+1]) <= 26: cur += prev2
        prev2, prev1 = prev1, cur
    return prev1 if s else 0
```

- Word break: O(n·L) substring checks (L = max word length), each O(L) to hash: O(n·L²); O(n) space. A trie avoids re-hashing.
- Decode ways: O(n) time, O(1) space. Edge cases: leading "0", "10", "20", "30" (invalid), "06".

### 0/1 knapsack, partition equal subset sum

```python
def knapsack(weights, values, cap):     # each item at most once
    dp = [0] * (cap + 1)
    for w, v in zip(weights, values):
        for c in range(cap, w - 1, -1):  # iterate DOWN so each item is used once
            dp[c] = max(dp[c], dp[c - w] + v)
    return dp[cap]

def can_partition(nums):
    total = sum(nums)
    if total % 2: return False
    target = total // 2
    dp = [True] + [False] * target      # dp[s]: some subset sums to s
    for x in nums:
        for s in range(target, x - 1, -1): dp[s] = dp[s] or dp[s - x]
    return dp[target]
```

- Knapsack: O(n·cap) time, O(cap) space (pseudo-polynomial). Iterating capacity upward instead gives the unbounded knapsack.
- Partition: O(n·sum/2) time, O(sum/2) space. Python trick: a bitset `bits |= bits << x` does the same very fast.

## Bit manipulation & math

### Single number, counting bits, power of two

**Key identities:** `x ^ x = 0`, `x ^ 0 = x`; `x & (x - 1)` clears the lowest set bit; `x & -x` isolates it.

```python
from functools import reduce
from operator import xor
def single_number(nums):                # every other element appears twice
    return reduce(xor, nums, 0)

def count_bits(n):                      # popcount for 0..n
    res = [0] * (n + 1)
    for i in range(1, n + 1): res[i] = res[i >> 1] + (i & 1)
    return res

def is_power_of_two(n):
    return n > 0 and n & (n - 1) == 0
```

- Single number: O(n) time, O(1) space. Counting bits: O(n) time. Power of two: O(1).
- Python note: ints are unbounded, so for 32-bit problems with negatives mask with `& 0xFFFFFFFF`. `int.bit_count()` (3.10+) gives popcount directly.

### GCD/LCM, prime sieve, fast exponentiation

```python
def gcd(a, b):                          # Euclid
    while b: a, b = b, a % b
    return abs(a)

def lcm(a, b): return a // gcd(a, b) * b if a and b else 0

def sieve(n):                           # all primes < n
    if n < 3: return []
    is_p = bytearray([1]) * n; is_p[0] = is_p[1] = 0
    for i in range(2, int(n ** 0.5) + 1):
        if is_p[i]: is_p[i*i::i] = bytearray(len(range(i*i, n, i)))
    return [i for i in range(n) if is_p[i]]

def fast_pow(base, exp, mod=None):      # exponentiation by squaring
    res = 1
    while exp > 0:
        if exp & 1: res = res * base % mod if mod else res * base
        base = base * base % mod if mod else base * base
        exp >>= 1
    return res
```

- GCD: O(log min(a, b)). LCM: divide before multiplying to avoid overflow in other languages.
- Sieve: O(n log log n) time, O(n) space; start crossing out at `i*i`.
- Fast power: O(log exp) multiplications. Built-ins: `math.gcd`, `math.lcm`, `pow(b, e, m)`. Negative exponent → compute `1 / fast_pow(base, -exp)`.

### Reservoir sampling; random pick with weight

```python
import random, bisect, itertools
def reservoir_sample(stream, k):        # uniform k items from a stream of unknown length
    res = []
    for i, x in enumerate(stream):
        if i < k: res.append(x)
        else:
            j = random.randint(0, i)    # inclusive: keep x with probability k/(i+1)
            if j < k: res[j] = x
    return res

class WeightedPicker:                   # random pick with weight
    def __init__(self, w):
        self.prefix = list(itertools.accumulate(w)); self.total = self.prefix[-1]
    def pick_index(self):
        r = random.random() * self.total            # in [0, total)
        return bisect.bisect_right(self.prefix, r)
```

- Reservoir: O(n) time, O(k) space, one pass. Proof sketch: after seeing i+1 items, each item is in the sample with probability k/(i+1) — show it by induction. Real use: sampling logs or events for debugging when you cannot hold the stream in memory.
- Weighted pick: O(n) build, O(log n) per pick. For many picks with static weights, Walker's alias method gives O(1) per pick. If weights change, use a Fenwick tree.

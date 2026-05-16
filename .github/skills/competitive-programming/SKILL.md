---
name: competitive-programming
description: "Competitive programming mastery from zero to Codeforces Master/Grandmaster. Covers algorithms, data structures, problem-solving patterns, math, and the mental discipline of Tourist-level thinking. Activates for any CP session — learning, problem solving, pattern recognition, or timed practice."
---

# Competitive Programming Mastery

A depth-first curriculum for going from beginner to elite competitive programmer. Built on the same learning loop used by top-rated Codeforces competitors: **concept → warm-up → apply → challenge → debrief → repeat**.

---

## What Separates Tourist from Everyone Else

Tourist (Gennady Korotkevich) does not just know more algorithms. He:

1. **Instantly identifies the problem class** — within 30 seconds of reading, he knows what it is.
2. **Has zero gaps in fundamentals** — every basic concept is so deeply wired that it takes zero mental effort.
3. **Thinks in complexity** — before writing, he knows the required complexity from constraints.
4. **Stays calm under pressure** — the skill is automated; the brain is free for the creative part.
5. **Has solved 10,000+ problems** — pattern recognition at this level only comes from volume.

This curriculum builds all five. It starts with zero gaps, not with impressive tricks.

---

## The Master Loop

Every learning session follows this loop exactly. Do not skip phases.

```
LEARN concept (intuition first, template second)
     ↓
WARM-UP: one direct application problem
     ↓
APPLY: one problem combining this + prior knowledge
     ↓
CHALLENGE: one problem one step above comfort
     ↓
DEBRIEF: lock in the key insight
     ↓
REPEAT with next concept
```

A concept is not learned until the learner can:

1. Explain the key insight in one plain sentence
2. Write the template from memory
3. State the time and space complexity
4. Name 2 problem types where it applies
5. Name 1 common mistake that breaks it

---

## Complexity Decision Framework

Before writing any code, always answer:

- What is N? (the input constraint)
- How much time do I have? (usually 1–2 seconds = ~10^8 simple operations)

```
Read N from the constraints — then pick your weapon:

  N ≤ 10          ████████████████  anything works     O(N!) or O(2^N)
  N ≤ 20          ██████████████    bitmask OK         O(2^N)
  N ≤ 500         ████████████      nested loops OK    O(N²)
  N ≤ 5,000       ██████████        nested, careful    O(N²)
  N ≤ 100,000     ████████          need a log factor  O(N log N)
  N ≤ 1,000,000   ██████            linear only        O(N)
  N ≤ 10^9        ████              math / binary      O(log N) or O(1)

  The bar shrinks → your algorithm must get smarter.
```

| N         | Max acceptable complexity | Algorithm class                      |
| --------- | ------------------------- | ------------------------------------ |
| N ≤ 10    | O(N! or 2^N)              | Brute force, permutations            |
| N ≤ 20    | O(2^N)                    | Bitmask DP, brute force with subsets |
| N ≤ 500   | O(N²)                     | Nested loops DP, Floyd-Warshall      |
| N ≤ 5,000 | O(N² / relaxed)           | Simple nested DP                     |
| N ≤ 10^5  | O(N log N)                | Sort, segment tree, binary search    |
| N ≤ 10^6  | O(N)                      | Linear scan, BFS, sieve              |
| N ≤ 10^9  | O(log N) or O(1)          | Math, binary search, formulas        |

**Rule**: Write the constraint check before writing the algorithm. If your approach doesn't fit — think again.

---

## Full Curriculum

### Level 0 — Entry Foundations (CF ~800–1000)

**Purpose**: Zero-error fundamentals. If these are shaky, everything above will crack.

| Concept               | Key Insight                                                                                         | Common Bug                                                         |
| --------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| I/O speed             | Read all stdin at once: `const lines = require('fs').readFileSync('/dev/stdin','utf8').split('\n')` | Using `readline` line-by-line in a loop causes TLE on large inputs |
| Integer overflow      | 10^5 × 10^5 = 10^10 > int range (2×10^9)                                                            | Not casting to `long long` before multiplication                   |
| Modular arithmetic    | (a + b) % m, (a × b) % m are safe. Division needs modular inverse                                   | Taking mod after very large multiplication                         |
| GCD / LCM             | gcd(a,b) via Euclidean algorithm. lcm(a,b) = a/gcd × b                                              | LCM overflow if not computed as (a/gcd)\*b                         |
| Sieve of Eratosthenes | Mark composites in O(N log log N). Fast prime checking up to N                                      | Off-by-one in sieve bounds                                         |
| Simulation            | Implement exactly what the problem says. No cleverness yet.                                         | Missing edge cases in state transitions                            |

**Exit gate**: Solve 5 CF 800-rated and 5 CF 1000-rated problems with no hints in < 10 min each.

**JavaScript / TypeScript CP foundations — memorize these before your first submission:**

| Gotcha                        | The problem                                              | Safe pattern                                                       |
| ----------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------ |
| Default sort is lexicographic | `[10, 9, 2].sort()` → `[10, 2, 9]` (wrong!)              | Always: `.sort((a, b) => a - b)` for numbers                       |
| `Array.shift()` is O(N)       | BFS with `queue.shift()` → entire BFS becomes O(N²)      | Use a head pointer: `let head = 0; const u = queue[head++]`        |
| Number precision above 2^53   | `9007199254740993 + 1` silently gives the wrong answer   | Use `BigInt` for modular arithmetic: `power(2n, 10n, 1000000007n)` |
| `%` is remainder, not modulo  | `-3 % 5` equals `-3` in JS, not `2`                      | For CP: `((a % m) + m) % m` to guarantee a positive result         |
| Integer division              | `7 / 2` equals `3.5` in JS                               | Use `Math.floor(7 / 2)` or `(7 / 2) \| 0` for positive numbers     |
| No native sorted map / set    | `Map` and `Set` are insertion-ordered, not sorted by key | Simulate with a sorted array + binary search                       |

---

### Level 1 — Bronze Toolkit (CF ~1000–1400)

| Concept                     | Key Insight                                                                                                            | Template Exists                                           |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Sorting + comparator        | Sort to impose order, then scan linearly. Always provide `(a, b) => a - b` — JS default sort is lexicographic          | Yes                                                       |
| Map / Set                   | `Map<K,V>` and `Set<T>` are O(1) avg but **not sorted**. For sorted iteration, maintain a sorted array + binary search | No native sorted container                                |
| Min-heap (priority queue)   | No built-in heap in JS/TS. Must implement a binary heap. Needed for Dijkstra and greedy problems.                      | Using `.sort()` each step is O(N log N) per op — too slow |
| Prefix sums (1D)            | precompute sum[i] = a[0]+...+a[i]. Range sum in O(1)                                                                   | Yes                                                       |
| Prefix sums (2D)            | sum[i][j] = sum above + sum left - diagonal + a[i][j]                                                                  | Yes                                                       |
| Two pointers                | Maintain a window [l,r]. Move r forward, shrink l when violated                                                        | Yes                                                       |
| Greedy: interval scheduling | Sort by end time, pick non-overlapping greedily                                                                        | Yes                                                       |
| Complete search             | Try all possibilities. Prune early when over limit                                                                     | No                                                        |

#### Prefix Sum Template (TypeScript)

```typescript
// Build prefix sum — O(N)
const pre: number[] = new Array(n + 1).fill(0);
for (let i = 0; i < n; i++) {
  pre[i + 1] = pre[i] + a[i];
}

// Range sum [l, r] inclusive (0-indexed) — O(1)
// Danger: pre has size n+1. pre[r+1] - pre[l]
const rangeSum: number = pre[r + 1] - pre[l];
```

```
Visual — how a prefix sum works:

  Original array a:   [  3,   1,   4,   1,   5  ]
  Index:                  0    1    2    3    4

  Prefix array pre:   [  0,   3,   4,   8,   9,  14  ]
  Index:                  0    1    2    3    4    5
                          ↑
                     pre[0] = 0 (always — the "empty prefix")

  Query: "What is the sum from index 1 to 3?"
         = pre[4] - pre[1]
         = 9 - 3
         = 6  ✓  (because 1 + 4 + 1 = 6)

  Think of it as:
    pre[r+1] = total up to and including r
    pre[l]   = total before l
    Difference = exactly the elements in between
```

#### Two Pointers Template (TypeScript)

```typescript
// Find longest subarray with sum ≤ target — O(N)
let l = 0;
let windowSum = 0;
let maxLen = 0;
for (let r = 0; r < n; r++) {
  windowSum += a[r];
  // Shrink from left until invariant holds
  while (windowSum > target) {
    windowSum -= a[l++];
  }
  maxLen = Math.max(maxLen, r - l + 1);
}
```

```
Visual — two pointers shrinking a window:
(finding the longest subarray with sum ≤ 7)

  Array:  [ 2,  3,  1,  4,  2 ]

  Step 1: [l=0, r=0]  window=[2]        sum=2  ≤ 7 ✓  length=1
  Step 2: [l=0, r=1]  window=[2,3]      sum=5  ≤ 7 ✓  length=2
  Step 3: [l=0, r=2]  window=[2,3,1]    sum=6  ≤ 7 ✓  length=3
  Step 4: [l=0, r=3]  window=[2,3,1,4]  sum=10 > 7 ✗  shrink left
          [l=1, r=3]  window=[3,1,4]    sum=8  > 7 ✗  shrink left
          [l=2, r=3]  window=[1,4]      sum=5  ≤ 7 ✓  length=2
  Step 5: [l=2, r=4]  window=[1,4,2]    sum=7  ≤ 7 ✓  length=3

  Answer: longest length = 3

  Key rule: left pointer only moves RIGHT. Right pointer only moves RIGHT.
  Total pointer moves ≤ 2N → the entire loop runs in O(N).
```

**Exit gate**: Solve CF Div3 A–D problems. Recognize "sort then scan" and "prefix sum" immediately on sight.

---

### Level 2 — Silver Algorithms (CF ~1400–1800)

| Concept                              | Key Insight                                               | Template Exists |
| ------------------------------------ | --------------------------------------------------------- | --------------- |
| Binary search on answer              | If a function is monotone, binary search for the boundary | Yes             |
| BFS (shortest path, unweighted)      | Explore layer by layer. Distance = #layers                | Yes             |
| DFS (connected components, cycles)   | Go deep. Track visited. Backtrack.                        | Yes             |
| Union-Find (DSU)                     | Track which component each node belongs to in near-O(1)   | Yes             |
| Dynamic programming 1D               | dp[i] = best way to reach state i                         | Yes             |
| LIS (Longest Increasing Subsequence) | O(N log N) with patience sorting / binary search          | Yes             |
| Trees: DFS order, subtree sizes      | DFS assigns in-time/out-time to compress subtree queries  | Yes             |
| Dijkstra's algorithm                 | Priority queue + relax edges greedily. O((V+E) log V)     | Yes             |

#### Binary Search on Answer Template (TypeScript)

```typescript
// Binary search for minimum value where check(mid) is true — O(log(hi-lo) * check_cost)
// Danger: ensure check(lo) = false, check(hi) = true for this template
// Danger: JS safe integer limit is 2^53 (~9×10^15). Use 10**15 as a safe upper bound.
let lo = 0,
  hi = 10 ** 15;
while (lo < hi) {
  const mid = Math.floor(lo + (hi - lo) / 2); // Math.floor avoids float drift
  if (check(mid)) hi = mid;
  else lo = mid + 1;
}
// lo === hi === answer
```

```
Visual — binary search on a monotone function:
("What is the minimum speed X where a task finishes in time?")

  All possible values of X:
  ──────────────────────────────────────────────
  check(X) result:   ✗  ✗  ✗  ✗  ✓  ✓  ✓  ✓
                     ↑                 ↑
                    lo                hi
                              ↑
                          boundary = answer

  Each step:
    mid = lo + (hi - lo) / 2
    if check(mid) is ✗  →  answer is HIGHER  →  lo = mid + 1
    if check(mid) is ✓  →  answer is HERE or LOWER  →  hi = mid

  After ~30 steps: lo == hi == the exact boundary.

  The trick: you are not searching for a value in a sorted array.
  You are searching for the BOUNDARY between "impossible" and "possible".
  Any problem with this shape can use binary search on the answer.
```

#### BFS Template (TypeScript)

```typescript
// BFS from source src — O(V + E)
// Danger: NEVER use queue.shift() — it is O(N), making the whole BFS O(N²).
// Use a head pointer for O(1) dequeue.
const dist: number[] = new Array(n).fill(-1);
const queue: number[] = [src];
let head = 0;
dist[src] = 0;
while (head < queue.length) {
  const u = queue[head++]; // O(1) — pointer moves forward, no array shift
  for (const v of adj[u]) {
    if (dist[v] === -1) {
      // not visited
      dist[v] = dist[u] + 1;
      queue.push(v);
    }
  }
}
```

```
Visual — BFS explores one full "layer" before going deeper:
(graph edges: A→B, A→C, B→D, B→E, C→F)

                   A            ← Layer 0  dist[A]=0  (start)
                 /   \
               B       C        ← Layer 1  dist[B]=1, dist[C]=1
              / \       \
             D   E        F     ← Layer 2  dist[D]=2, dist[E]=2, dist[F]=2

  Queue trace:
    queue=[A]         → pop A, push B and C
    queue=[B, C]      → pop B, push D and E
    queue=[C, D, E]   → pop C, push F
    queue=[D, E, F]   → pop D, E, F — no unvisited neighbors → done

  Rule: first time you reach a node = shortest path to it.
  Always mark nodes visited when you PUSH them, not when you pop them.
  (If you mark on pop, the same node gets pushed multiple times — wrong!)
```

#### Union-Find Template (TypeScript)

```typescript
// DSU with path compression + union by rank — O(α(N)) ≈ O(1) per operation
class DSU {
  parent: number[];
  rank: number[];

  constructor(n: number) {
    this.parent = Array.from({ length: n }, (_, i) => i);
    this.rank = new Array(n).fill(0);
  }

  find(x: number): number {
    // path compression
    if (this.parent[x] !== x) this.parent[x] = this.find(this.parent[x]);
    return this.parent[x];
  }

  unite(x: number, y: number): boolean {
    // union by rank. Returns true if merged.
    x = this.find(x);
    y = this.find(y);
    if (x === y) return false;
    if (this.rank[x] < this.rank[y]) [x, y] = [y, x];
    this.parent[y] = x;
    if (this.rank[x] === this.rank[y]) this.rank[x]++;
    return true;
  }

  same(x: number, y: number): boolean {
    return this.find(x) === this.find(y);
  }
}
```

```
Visual — Union-Find tracks which "group" each element belongs to:

  Start: every element is its own group (points to itself as root)
    (1)  (2)  (3)  (4)  (5)

  After unite(1, 2):    After unite(3, 4):
        1                     3
        |                     |
        2                     4

  After unite(1, 3):   (merges the whole group containing 3 under 1)
          1
         / \
        2   3
            |
            4

  find(4) → parent[4]=3 → parent[3]=1 → parent[1]=1 → ROOT = 1
  find(2) → parent[2]=1 → ROOT = 1
  same(2, 4)?  find(2)=1, find(4)=1  → same root → YES ✓

  Path compression: after find(4) walks 4→3→1,
  it rewires 4's parent directly to 1.
  Next call to find(4) is instant: 4→1.
```

**Exit gate**: Can write BFS, DFS, and DSU from memory. Solves CF Div2 A–C problems. Distinguishes DP from Greedy by reading the problem.

---

### Level 3 — Gold Structures (CF ~1800–2100)

| Concept            | Key Insight                                                           | Template Exists |
| ------------------ | --------------------------------------------------------------------- | --------------- |
| Segment tree       | Divide array into intervals. Range query + point update in O(log N)   | Yes             |
| Fenwick tree (BIT) | Prefix sums with updates. Simpler than segment tree but less flexible | Yes             |
| Lazy segment tree  | Defer updates to children. Enables range update + range query         | Yes             |
| Bitmask DP         | State = subset of elements. 2^N states, each transition O(1)          | Yes             |
| Knapsack DP        | Choose items to maximize value under weight constraint                | Yes             |
| Digit DP           | Count integers in [L,R] with a digit property                         | Yes             |
| String hashing     | Compute polynomial hash to compare substrings in O(1)                 | Yes             |
| KMP / Z-algorithm  | Find pattern in string without backtracking. O(N+M)                   | Yes             |
| Number theory      | Modular inverse, Euler's totient, CRT, fast exponentiation            | Yes             |
| Topological sort   | Order nodes so all edges point forward. DAG requirement               | Yes             |

#### Segment Tree Template (TypeScript)

```typescript
// Iterative segment tree: point update, range sum query — O(log N)
class SegTree {
  n: number;
  t: number[];

  constructor(n: number) {
    this.n = n;
    this.t = new Array(2 * n).fill(0);
  }

  update(i: number, v: number): void {
    // point set at position i
    this.t[(i += this.n)] = v;
    for (; i > 1; i >>= 1) this.t[i >> 1] = this.t[i] + this.t[i ^ 1]; // Danger: change + to Math.max/min for other queries
  }

  query(l: number, r: number): number {
    // sum in [l, r) — note: r is exclusive
    let res = 0;
    for (l += this.n, r += this.n; l < r; l >>= 1, r >>= 1) {
      if (l & 1) res += this.t[l++];
      if (r & 1) res += this.t[--r];
    }
    return res;
  }
}
```

```
Visual — a segment tree stores precomputed answers for every interval:

  Array:   [  3,   1,   4,   1,   5,   9,   2,   6  ]
  Index:      0    1    2    3    4    5    6    7

                        [ 31 ]                   ← sum of all 8 elements
                     /          \
               [  9 ]            [ 22 ]           ← left half, right half
              /       \'        /       \
           [ 4 ]    [ 5 ]   [ 14 ]    [ 8 ]       ← quarters
           /   \    /   \   /    \    /   \
          [3] [1]  [4] [1] [5]  [9] [2] [6]       ← leaves = original values

  Query "sum of index 2 to 5" (values 4,1,5,9 = 19):
    → tree already knows [4+1]=5 and [5+9]=14
    → combine just those two nodes: 5+14 = 19 in O(log N)
    → never touches the full array

  Update "set index 3 = 10" (was 1, now 10):
    → change the leaf [1] to [10]
    → fix only the parents going up: [5]→[14], [9]→[18], [31]→[40]
    → touches O(log N) nodes — does not rebuild the whole tree
```

#### Fast Modular Exponentiation (TypeScript)

```typescript
// a^b mod m — O(log b)
// Danger: MUST use BigInt. Regular JS numbers lose precision above 2^53.
// All arguments use the bigint type (suffix n). Example: power(2n, 10n, 1000000007n)
function power(a: bigint, b: bigint, m: bigint): bigint {
  let res = 1n;
  a = a % m;
  while (b > 0n) {
    if (b & 1n) res = (res * a) % m;
    a = (a * a) % m;
    b >>= 1n;
  }
  return res;
}

// Modular inverse (m must be prime): a^(m-2) mod m
const modinv = (a: bigint, m: bigint): bigint => power(a, m - 2n, m);
```

**Exit gate**: Writes segment tree from memory. Solves CF Div2 D problems. Recognizes "range query + update" as segment tree signal.

---

### Level 4 — Master Techniques (CF ~2100–2400)

| Concept                   | Key Insight                                                                 | When to Use                                   |
| ------------------------- | --------------------------------------------------------------------------- | --------------------------------------------- |
| Heavy-Light Decomposition | Decompose tree into O(log N) chains. Enables path queries with segment tree | Tree path queries (sum, max, LCA)             |
| Centroid Decomposition    | Divide tree at centroid. Enables counting paths through decomposition       | Counting paths with property P                |
| Convex Hull Trick         | DP transition: dp[i] = min over j of (f(j) + cost(i,j)). Optimized with CHT | DP where cost is linear in j                  |
| Divide & Conquer DP       | When transition optimal point is monotone                                   | DP with monotone optimal transitions          |
| Suffix Array + LCP        | Sort all suffixes. LCP array enables substring queries in O(1)              | Longest repeated substring, string comparison |
| Persistent segment tree   | Version history of segment tree. O(log N) per version                       | Kth element in range, offline range queries   |
| Dinic's max flow          | BFS builds level graph, DFS sends blocking flow. O(V² × E)                  | Network flow, bipartite matching              |
| Sprague-Grundy theorem    | Every impartial game position has a Grundy value (nimber)                   | Combinatorial game theory problems            |

**Exit gate**: Solves CF Div1 C problems. Reads CF Div1 D editorial and understands it without needing to look up the algorithm.

---

### Level 5 — Grandmaster Frontier (CF ~2400+)

These are studied one at a time, only after Level 4 is fully mastered.

| Concept                  | Resources                                                  |
| ------------------------ | ---------------------------------------------------------- |
| Suffix Automaton         | cp-algorithms.com/string/suffix-automaton                  |
| Aho-Corasick automaton   | cp-algorithms.com/string/aho_corasick                      |
| Min-cost max-flow (MCMF) | cp-algorithms.com/graph/min_cost_flow                      |
| Mo's algorithm           | cp-algorithms.com/data_structures/sqrt_decomposition       |
| Matrix exponentiation    | Reduce linear DP recurrence to matrix power in O(K³ log N) |
| Randomized algorithms    | Hashing, random pivots, Las Vegas algorithms               |

---

## Problem-Solving Mental Protocol

When you open a new problem, apply this protocol in order. Do NOT skip steps.

```
STEP 1 — Read the problem twice. Underline constraints.
STEP 2 — What is N? → What complexity do I need?
STEP 3 — Can I solve it in O(N²)? Yes → can I improve? No → it must be faster.
STEP 4 — What does brute force look like?
STEP 5 — What is the BOTTLENECK in brute force? What is slow?
STEP 6 — Which data structure or algorithm makes that bottleneck fast?
STEP 7 — State the approach in one sentence before coding.
STEP 8 — Code the approach. Test on given examples by hand.
STEP 9 — Test on edge cases: N=1, all same, sorted, reverse sorted, max values.
STEP 10 — Submit.
```

**If stuck after 15 min**: go to Step 4 and re-examine brute force. The key insight is almost always hiding in why brute force is slow.

---

## Common Mistake Catalog

Every learner hits these. Study them now so you don't lose contests to them.

| Mistake                              | Example                                                     | Fix                                                            |
| ------------------------------------ | ----------------------------------------------------------- | -------------------------------------------------------------- |
| Integer overflow                     | `int a = 1e5; a * a` = -1794967296                          | Use `long long`. Multiply as `(long long)a * a`                |
| Off-by-one in binary search          | `while (lo <= hi)` vs `while (lo < hi)`                     | Memorize one correct template. Never improvise.                |
| Modifying array while iterating      | Erasing elements from vector mid-loop                       | Collect indices first, erase after                             |
| BFS with visited check after push    | Nodes pushed multiple times                                 | Mark visited WHEN PUSHING, not when popping                    |
| Uninitialized DP array               | `dp[i]` starts with garbage                                 | Always initialize: `vector<long long> dp(n+1, 0)` or `INT_MAX` |
| Wrong base case in DP                | `dp[0] = 0` when it should be `dp[0] = 1`                   | Trace by hand for i=0,1,2 before coding the rest               |
| Signed/unsigned comparison           | `vector.size() - 1` when size is 0 → wraps to huge positive | Cast: `(int)v.size() - 1`                                      |
| Stack overflow from deep recursion   | DFS on N=10^5 depth chain                                   | Use iterative DFS or increase stack size                       |
| Not clearing data between test cases | Global arrays retaining values from previous test           | Clear explicitly or use local variables                        |
| Greedy without proof                 | Assuming greedy works without proving it                    | Always ask: "what exchange argument proves this?"              |

---

## Practice Routine (Daily)

Structure your practice sessions like this to maximize improvement:

### Daily Session (1–2 hours)

1. **10 min** — Solve 1 problem you KNOW you can solve (build speed and confidence)
2. **30 min** — Study 1 new concept from your current level (follow the teaching loop)
3. **40 min** — Solve 2 problems using today's concept
4. **10 min** — Read 1 editorial of a problem you couldn't solve recently

### Weekly

- 1 virtual contest (set a timer, no help, solve as many as you can in 2 hours)
- Review all problems you couldn't solve that week — understand the editorial, code it from scratch
- Track: which concepts caused you to fail? Those are your next study targets.

### Monthly

- Complete one full "topic sprint": spend a week on one concept, solve 20+ problems on it
- Review your contest history: what types of problems keep appearing in your weak category?

---

## Learning Resources (primary sources only)

| Resource                                      | What it covers                                      | Best for             |
| --------------------------------------------- | --------------------------------------------------- | -------------------- |
| cp-algorithms.com                             | Every algorithm with proofs and implementations     | Silver → Grandmaster |
| USACO Guide                                   | Structured curriculum from bronze to platinum       | Bronze → Gold        |
| Codeforces Problemset                         | The best problem database. Filter by rating and tag | All levels           |
| Atcoder Educational DP Contest                | 26 DP problems covering every DP type               | Silver → Gold        |
| Competitive Programmer's Handbook (Laaksonen) | Free book, covers most of curriculum                | Bronze → Master      |
| Tourist's Codeforces blog                     | Rare but gold. Read everything he's written         | Master+              |

---

## Mindset Rules

1. **Stuck is normal.** Getting stuck for 20 minutes on a problem is not failure — it is training.
2. **Never read the editorial before 20 minutes of genuine effort.** The struggle IS the learning.
3. **Reading editorial is a skill.** After reading: close it, and code the solution from scratch without looking.
4. **Volume beats intensity.** 5 problems a day beats 1 marathon session per week.
5. **Upsolve everything.** Every problem you fail in a contest must be solved within 24 hours.
6. **Track your weak points.** Keep a notebook: "I always mess up binary search when...". Fix those.
7. **Implement templates until they are reflexes.** You should be able to write DSU in your sleep.
8. **Compete, don't study forever.** Enter every rated CF round you can. Pressure accelerates growth.

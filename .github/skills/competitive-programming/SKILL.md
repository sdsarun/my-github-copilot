---
name: competitive-programming
description: "Competitive programming mastery from zero to Codeforces Master/Grandmaster. Covers algorithms, data structures, problem-solving patterns, math, and the mental discipline of Tourist-level thinking. Activates for any CP session — learning, problem solving, pattern recognition, or timed practice."
---

# Competitive Programming Mastery

Core loop: **concept → warm-up → apply → challenge → debrief → repeat**

---

## What Separates Tourist

1. Instantly identifies problem class within 30 seconds of reading
2. Zero gaps in fundamentals — basics take zero mental effort
3. Thinks in complexity first — knows required complexity from constraints before writing
4. Calm under pressure — skill automated; brain free for creativity
5. Pattern recognition from 10,000+ problems solved

---

## Complexity Decision Table

| N           | Max Complexity   | Algorithm class                   |
| ----------- | ---------------- | --------------------------------- |
| ≤ 10        | O(N! or 2^N)     | Brute force, permutations         |
| ≤ 20        | O(2^N)           | Bitmask DP                        |
| ≤ 500       | O(N²)            | Nested loops, Floyd-Warshall      |
| ≤ 5,000     | O(N²)            | Simple nested DP                  |
| ≤ 100,000   | O(N log N)       | Sort, segment tree, binary search |
| ≤ 1,000,000 | O(N)             | Linear scan, BFS, sieve           |
| ≤ 10^9      | O(log N) or O(1) | Math, binary search, formulas     |

**First question before any code**: What is N? → What complexity is needed?

---

## JS / TypeScript Gotchas

| Gotcha                      | Problem                              | Fix                           |
| --------------------------- | ------------------------------------ | ----------------------------- |
| Default sort lexicographic  | `[10,9,2].sort()` → `[10,2,9]`       | `.sort((a,b) => a-b)`         |
| `Array.shift()` is O(N)     | BFS becomes O(N²)                    | Head pointer: `queue[head++]` |
| Number precision > 2^53     | Silent wrong answers in mod problems | Use `BigInt`                  |
| `%` is remainder not modulo | `-3 % 5 === -3`                      | `((a % m) + m) % m`           |
| Integer division            | `7 / 2 === 3.5`                      | `Math.floor(7 / 2)`           |
| No sorted map/set           | `Map`/`Set` are insertion-ordered    | Sorted array + binary search  |

---

## Level Ladder

### Level 0 — Entry (CF 800–1000)

I/O speed, integer overflow, modular arithmetic, GCD/LCM, sieve of Eratosthenes, simulation
**Gate**: Solve CF Div4 A-C reliably in < 5 min each

### Level 1 — Bronze (CF 1000–1400)

Sorting + comparators, Map/Set, prefix sums (1D + 2D), two pointers, greedy (interval scheduling), complete search
**Gate**: Solve CF Div3 A-D

### Level 2 — Silver (CF 1400–1800)

Binary search on answer, BFS/DFS, connected components, 1D DP, LIS, sliding window, DSU, trees (DFS order/LCA), Dijkstra
**Gate**: Solve CF Div2 A-C; distinguish DP from Greedy by reading the problem

### Level 3 — Gold (CF 1800–2100)

Segment tree, Fenwick tree (BIT), lazy segment tree, bitmask DP, knapsack DP, digit DP, string hashing, KMP/Z-algorithm, number theory, topological sort
**Gate**: Solve CF Div2 D; write segment tree from memory

### Level 4 — Master (CF 2100–2400)

HLD, centroid decomposition, CHT, D&C DP, suffix array + LCP, persistent segment tree, Dinic's max flow, Sprague-Grundy
**Gate**: Solve CF Div1 C; read Div1 D editorial and understand it without looking up the algorithm

### Level 5 — Grandmaster (CF 2400+)

Suffix automaton, Aho-Corasick, MCMF, Mo's algorithm, matrix exponentiation, randomized algorithms
**Gate**: Consistently solve Div1 D/E; invent solutions rather than matching patterns

---

## Templates

### Fast I/O

```typescript
const lines = require("fs").readFileSync("/dev/stdin", "utf8").split("\n");
let ptr = 0;
const next = () => lines[ptr++].trim();
const nextInts = () => next().split(" ").map(Number);
```

### Prefix Sum (1D)

```typescript
const pre = new Array(n + 1).fill(0);
for (let i = 0; i < n; i++) pre[i + 1] = pre[i] + a[i];
// range sum [l, r] inclusive (0-indexed): pre[r+1] - pre[l]
```

### Two Pointers (longest window satisfying condition)

```typescript
let l = 0,
  windowSum = 0,
  best = 0;
for (let r = 0; r < n; r++) {
  windowSum += a[r];
  while (windowSum > target) windowSum -= a[l++]; // shrink until valid
  best = Math.max(best, r - l + 1);
}
```

### Binary Search on Answer

```typescript
// DANGER: check must be monotone (false...false...true...true)
// Find minimum X where check(X) is true
let lo = 0,
  hi = 10 ** 15;
while (lo < hi) {
  const mid = Math.floor(lo + (hi - lo) / 2);
  if (check(mid)) hi = mid;
  else lo = mid + 1;
}
// answer = lo
```

### BFS (never use .shift() — it is O(N))

```typescript
const dist = new Array(n).fill(-1);
const queue: number[] = [src];
let head = 0;
dist[src] = 0;
while (head < queue.length) {
  const u = queue[head++]; // O(1) head pointer
  for (const v of adj[u]) {
    if (dist[v] === -1) {
      dist[v] = dist[u] + 1;
      queue.push(v);
    }
  }
}
// Mark visited ON PUSH — not on pop. Otherwise same node pushed many times.
```

### Union-Find (DSU) — path compression + union by rank

```typescript
class DSU {
  p: number[];
  rank: number[];
  constructor(n: number) {
    this.p = Array.from({ length: n }, (_, i) => i);
    this.rank = new Array(n).fill(0);
  }
  find(x: number): number {
    if (this.p[x] !== x) this.p[x] = this.find(this.p[x]); // path compression
    return this.p[x];
  }
  unite(x: number, y: number): boolean {
    x = this.find(x);
    y = this.find(y);
    if (x === y) return false;
    if (this.rank[x] < this.rank[y]) [x, y] = [y, x];
    this.p[y] = x;
    if (this.rank[x] === this.rank[y]) this.rank[x]++;
    return true;
  }
  same(x: number, y: number): boolean {
    return this.find(x) === this.find(y);
  }
}
```

### Segment Tree — iterative, point update + range query

```typescript
class SegTree {
  n: number;
  t: number[];
  constructor(n: number) {
    this.n = n;
    this.t = new Array(2 * n).fill(0);
  }
  update(i: number, v: number): void {
    this.t[(i += this.n)] = v; // DANGER: 0-indexed — change + to merge op for max/min
    for (; i > 1; i >>= 1) this.t[i >> 1] = this.t[i] + this.t[i ^ 1];
  }
  query(l: number, r: number): number {
    // [l, r) — r is exclusive
    let res = 0;
    for (l += this.n, r += this.n; l < r; l >>= 1, r >>= 1) {
      if (l & 1) res += this.t[l++];
      if (r & 1) res += this.t[--r];
    }
    return res;
  }
}
```

### Modular Exponentiation — MUST use BigInt (numbers lose precision above 2^53)

```typescript
function power(a: bigint, b: bigint, m: bigint): bigint {
  let res = 1n;
  a %= m;
  while (b > 0n) {
    if (b & 1n) res = (res * a) % m;
    a = (a * a) % m;
    b >>= 1n;
  }
  return res;
}
const modinv = (a: bigint, m: bigint) => power(a, m - 2n, m); // m must be prime
```

---

## Problem-Solving Protocol

1. Read twice. Underline constraints.
2. What is N? → What complexity do I need?
3. What does brute force look like?
4. What is the BOTTLENECK in brute force?
5. Which data structure or algorithm fixes that bottleneck?
6. State the approach in ONE SENTENCE before coding.
7. Code. Test on given examples by hand.
8. Test edge cases: N=1, all same, sorted, reversed, max values.
9. Submit.

**If stuck > 15 min**: Return to step 3. The insight hides in why brute force is slow.

---

## Common Mistakes

| Mistake                         | Fix                                            |
| ------------------------------- | ---------------------------------------------- |
| Integer overflow                | Use `BigInt` for products > 2^53               |
| Off-by-one in binary search     | Memorize ONE template, never improvise         |
| BFS: visited check after pop    | Mark visited **when pushing**, not popping     |
| Uninitialized DP array          | `new Array(n + 1).fill(0)` or `fill(Infinity)` |
| `Array.shift()` in BFS          | Use head pointer: `queue[head++]`              |
| Global state between test cases | Clear globals or use local vars per test       |
| Greedy without proof            | Ask: "What exchange argument proves this?"     |
| Wrong segment tree merge op     | Swap `+` for `Math.max`/`Math.min` as needed   |

---

## Resources

| Resource                | Best for        |
| ----------------------- | --------------- |
| cp-algorithms.com       | Silver → GM     |
| USACO Guide             | Bronze → Gold   |
| Codeforces Problemset   | All levels      |
| AtCoder Educational DP  | 26 DP problems  |
| CP Handbook (Laaksonen) | Bronze → Master |

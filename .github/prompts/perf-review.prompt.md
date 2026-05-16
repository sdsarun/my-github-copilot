---
agent: performance-optimizer
description: Performance analysis — identify bottlenecks, algorithmic complexity, bundle size, and rendering issues
---

# Performance Review

Analyze the selected code for performance problems. Fix root causes — do not add `// TODO: optimize later` comments.

## Web Vitals Targets (flag if exceeded)

| Metric                   | Target  |
| ------------------------ | ------- |
| First Contentful Paint   | < 1.8s  |
| Largest Contentful Paint | < 2.5s  |
| Time to Interactive      | < 3.8s  |
| Cumulative Layout Shift  | < 0.1   |
| Total Blocking Time      | < 200ms |
| Bundle size (gzipped)    | < 200KB |

## Algorithmic Complexity

Flag patterns that have worse complexity than necessary:

| Pattern                                  | Issue         | Fix                                      |
| ---------------------------------------- | ------------- | ---------------------------------------- |
| Nested loops on same data                | O(n²)         | Use `Map` / `Set` for O(1) lookups       |
| `array.find()` called repeatedly in loop | O(n) per call | Convert to `Map` first                   |
| `Array.sort()` inside a loop             | O(n² log n)   | Sort once, outside the loop              |
| String concatenation in loop             | O(n²)         | Use `array.join()`                       |
| Deep-cloning large objects repeatedly    | O(n) each     | Shallow copy or `immer`                  |
| Recursion without memoization            | O(2^n)        | Add memoization / use iterative approach |

## Database Performance

- N+1 queries — one DB call per loop item
- Missing `WHERE` clause indexes — table scans on large tables
- `SELECT *` when only a few columns are needed
- No pagination on large result sets
- Synchronous queries where async/parallel is possible

## Frontend Performance

### Bundle Size

```bash
# Analyze bundle
npx source-map-explorer build/static/js/*.js
npx webpack-bundle-analyzer
```

- Large dependencies imported fully when only a part is needed (`import _ from 'lodash'` vs `import debounce from 'lodash/debounce'`)
- Third-party libraries not lazy-loaded
- Images not lazy-loaded or not using next-gen formats (WebP, AVIF)
- No code splitting at route level

### React Rendering

- Components re-rendering on every parent render — wrap with `React.memo`
- Expensive computations not memoized — use `useMemo`
- Callbacks recreated on every render — use `useCallback`
- State updates inside `useEffect` causing render loops
- Entire list re-rendering when only one item changes — add stable `key` props
- No virtualization for long lists (> 100 items) — use `react-window` or `@tanstack/virtual`

## Node.js / Server Performance

- `fs.readFileSync` or any synchronous blocking call in request handlers
- Missing caching for repeated identical computations or API calls
- No connection pooling for database connections
- Memory leaks: event listeners not removed, intervals not cleared, closures holding references

## Output Format

For each issue found:

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line if known]
Issue: [What the performance problem is]
Impact: [What it affects — load time, memory, CPU, etc.]
Fix: [Concrete suggestion]
```

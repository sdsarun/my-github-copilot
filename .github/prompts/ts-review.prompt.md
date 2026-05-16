---
mode: agent
description: TypeScript and JavaScript code review — type safety, async correctness, security, and idiomatic patterns
---

# TypeScript / JavaScript Code Review

Review the selected TypeScript or JavaScript code. Report findings only — do not rewrite or refactor.

## Review Priorities

### CRITICAL — Security

- `eval` / `new Function` with user input — never execute untrusted strings
- `innerHTML` / `dangerouslySetInnerHTML` with unsanitized input — XSS risk
- SQL/NoSQL string concatenation in queries — use parameterized queries or ORM
- User-controlled paths in `fs.readFile` or `path.join` — validate and normalize
- Hardcoded secrets — use environment variables
- `child_process` with user input — validate and allowlist

### HIGH — Type Safety

- `any` without justification — use `unknown` and narrow, or a precise type
- Non-null assertions `value!` without a preceding guard — add a runtime check
- `as` casts that bypass real type errors — fix the type instead
- Public functions missing explicit return types
- `tsconfig.json` changes that weaken strictness

### HIGH — Async Correctness

- `async` functions called without `await` or `.catch()` — unhandled rejections
- `await` inside loops when operations are independent — use `Promise.all`
- `array.forEach(async fn)` — does not await; use `for...of` or `Promise.all`
- Fire-and-forget in event handlers without error handling

### HIGH — Error Handling

- Empty `catch {}` blocks — swallowed errors
- `JSON.parse` without try/catch — throws on invalid input
- `throw "message"` — always `throw new Error("message")`
- Missing error boundaries around async/data-fetching React subtrees

### HIGH — Idiomatic Patterns

- Module-level mutable shared state — prefer immutable data
- `var` usage — use `const` by default, `let` when reassignment is needed
- `==` instead of `===` — use strict equality
- Callback-style async mixed with `async/await` — standardize on promises
- `fs.readFileSync` in request handlers — blocks the event loop; use async variants

### HIGH — Node.js Specifics

- Missing schema validation on external data at boundaries (Zod, Joi, Yup)
- `process.env` access without startup validation or fallback
- `require()` in ESM context — mixing module systems without clear intent

### MEDIUM — React / Next.js

- `useEffect` / `useCallback` / `useMemo` with incomplete dependency arrays
- State mutation instead of returning new objects

## Output Format

For each issue found:

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line if known]
Issue: [What is wrong]
Fix: [Concrete suggestion]
```

End with:

```
## Summary
- Critical: N
- High: N
- Medium: N
- Approved to ship: yes / no
```

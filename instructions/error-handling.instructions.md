---
applyTo: '**'
---

# Error Handling Conventions

## Core Rules

- **Never silently swallow errors** — always handle, log, or propagate
- **Fail fast at boundaries** — validate inputs at system entry points (API handlers, queue consumers, CLI args)
- **Two audiences**: users see friendly messages; developers see detailed logs with context

## Error Types

Distinguish between:

- **Expected errors** — the system anticipated this could happen (not found, validation failure, network timeout)
- **Unexpected errors** — bugs and invariant violations — log with full context and alert

## Pattern: User-Facing vs Developer Context

```ts
// User sees:    "Order not found"
// Logs contain: "OrderService.findById: order 42 not found for user 99, request-id: abc"
```

Never expose stack traces, internal paths, database schema, or dependency names to end users.

## Pattern: Result / Either

Prefer typed error returns over throwing for expected failures:

```ts
// TypeScript
type Result<T, E> = { ok: true; value: T } | { ok: false; error: E };

// Go
func FindOrder(id string) (*Order, error)

// Python
from result import Result, Ok, Err
```

Reserve exceptions/throws for unexpected errors that should propagate to a global handler.

## Pattern: Wrap with Context

When catching and rethrowing, always add context about what was happening:

```ts
try {
  await db.query(sql);
} catch (err) {
  throw new Error(`Loading user ${userId} failed: ${err.message}`, { cause: err });
}
```

## Logging

- Log errors at the level where they are **handled**, not where they are **created**
- Include: operation, relevant IDs, error message, and stack trace
- Use structured logging with consistent fields: `{ level, message, error, userId, requestId }`
- Do not log sensitive data (passwords, tokens, PII) even in error messages

## HTTP Status Codes for APIs

| Situation                       | Status          |
| ------------------------------- | --------------- |
| Success                         | 200 / 201 / 204 |
| Client sent invalid input       | 400             |
| Not authenticated               | 401             |
| Authenticated but not allowed   | 403             |
| Resource not found              | 404             |
| Conflict with existing state    | 409             |
| Server bug / unexpected failure | 500             |

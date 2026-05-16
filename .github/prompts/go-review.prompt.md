---
mode: agent
description: Go code review — idiomatic Go, concurrency, error handling, security, and performance
---

# Go Code Review

Review the selected Go code. Report findings only — do not rewrite.

## Before Reviewing

Run these if available:

```bash
go vet ./...
staticcheck ./...
golangci-lint run
go test -race ./...
```

If `go vet` fails, stop and report before reviewing.

## Review Priorities

### CRITICAL — Security

- SQL injection via string formatting in queries — use parameterized: `db.Query("WHERE id = ?", id)`
- Command injection — unvalidated input in `exec.Command`
- Path traversal — user-controlled paths without `filepath.Clean` + prefix validation
- Race conditions — shared state accessed from goroutines without synchronization
- `unsafe` package without justification comment
- Hardcoded secrets — use environment variables

### CRITICAL — Error Handling

- Ignored errors: `result, _ := someFunc()` — always handle
- Missing error wrapping — use `fmt.Errorf("context: %w", err)`
- `panic` for recoverable errors — only for true programmer errors
- Missing `errors.Is` / `errors.As` when checking error types

### HIGH — Concurrency

- Goroutine leaks — started but never has a path to exit
- Unbuffered channels that could deadlock
- Missing `sync.WaitGroup` when waiting for goroutines
- Mutex locked without `defer mu.Unlock()` — unlock on every return path
- `time.Sleep` used for synchronization instead of channels/sync primitives

### HIGH — Idiomatic Go

- Interface pollution — defining interfaces on the producer side instead of consumer side
- String concatenation in loops — use `strings.Builder`
- `interface{}` / `any` where a specific type or generic is appropriate
- Missing context propagation for cancellation

### HIGH — Code Quality

- Functions over 50 lines
- Nesting deeper than 4 levels — use early returns
- Package-level mutable variables — prefer dependency injection
- N+1 database queries in loops

## Output Format

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

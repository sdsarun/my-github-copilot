---
mode: agent
description: C# code review — .NET conventions, async/await, nullable reference types, security, and performance
---

# C# Code Review

Review the selected C# code. Report findings only.

## Before Reviewing

```bash
dotnet build
dotnet test
dotnet format --verify-no-changes
```

If build fails, stop and report.

## Review Priorities

### CRITICAL — Security

- SQL injection — string interpolation in `SqlCommand` — use parameterized queries or EF Core
- Command injection — unvalidated input in `Process.Start`
- Path traversal — user-controlled paths without `Path.GetFullPath` + base directory prefix check
- Insecure deserialization — `BinaryFormatter` or `JsonConvert` with `TypeNameHandling.All` on untrusted input
- Hardcoded secrets — use `IConfiguration`, environment variables, or Azure Key Vault
- Missing CSRF protection on state-changing endpoints
- Missing HTML output encoding — XSS risk

### CRITICAL — Error Handling

- Empty catch: `catch (Exception) {}` — always handle or rethrow with context
- Catching overly broad exceptions when a specific type should be caught
- `IDisposable` not wrapped in `using` / `await using` — resource leak

### HIGH — Async / Await

- Missing `CancellationToken` parameter on async I/O methods
- `async void` outside event handlers — use `async Task`
- `.Result` or `.Wait()` on a `Task` inside async context — deadlock risk in ASP.NET
- Missing `ConfigureAwait(false)` in library code (not needed in ASP.NET Core app code)
- `await` inside a regular `foreach` when `Task.WhenAll` would parallelize safely

### HIGH — Nullable Reference Types

- Null-forgiving operator `!` without a prior null guard — add a check
- Public API parameters typed as `T` that can realistically be null — use `T?`
- `string?` not checked before `.Length` or indexing

### HIGH — Code Quality

- Methods over 50 lines
- LINQ query evaluated multiple times — materialize with `.ToList()` once
- Mutable shared state in singleton services — thread-safety issue
- Missing `readonly` on fields that are never reassigned after construction

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

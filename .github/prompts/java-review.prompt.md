---
agent: code-reviewer
description: Java code review — Spring Boot / Quarkus, security, ORM correctness, async patterns, and code quality
---

# Java Code Review

Review the selected Java code. Auto-detect framework (Spring Boot vs Quarkus) and apply the relevant rules. Report findings only.

## Before Reviewing

```bash
mvn verify -q       # Maven
./gradlew check     # Gradle
```

If compilation fails, stop and report.

## Review Priorities

### CRITICAL — Security

- SQL injection — string concatenation in queries; always use `PreparedStatement` or JPA criteria/JPQL with named params
- Command injection — unvalidated input in `Runtime.exec()` or `ProcessBuilder`
- Path traversal — user-controlled paths without `Paths.get(...).normalize().startsWith(baseDir)`
- Hardcoded secrets — use environment variables or Spring Cloud Config / Quarkus config
- CSRF protection disabled: `csrf().disable()` — only acceptable for fully stateless APIs

### CRITICAL — Error Handling

- Empty catch blocks: `catch (Exception e) {}` — always handle or rethrow
- `.get()` on `Optional` without `.isPresent()` — use `.orElseThrow()`
- Missing `@ExceptionHandler` / `@ControllerAdvice` for centralized error responses
- Checked exceptions swallowed and re-thrown as unchecked without context

### CRITICAL — ORM / Database

- N+1 queries — lazy-loaded associations fetched inside a loop; use `JOIN FETCH` or `@EntityGraph`
- Missing `@Transactional` on service methods that perform multiple writes
- `findAll()` on large tables without pagination — use `Pageable`
- Mutable entity fields exposed directly — use DTOs for API responses

### HIGH — Async

- Blocking calls inside `@Async` methods or reactive pipelines
- Missing `.exceptionally()` or `.handle()` on `CompletableFuture`
- Unbounded thread pool submissions without back-pressure

### HIGH — Code Quality

- Methods over 50 lines
- Service classes with multiple domain responsibilities — split by domain
- Non-thread-safe mutable state in Spring singleton beans

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

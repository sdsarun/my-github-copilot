---
mode: agent
description: FastAPI code review — async correctness, dependency injection, Pydantic schemas, security, and production readiness
---

# FastAPI Code Review

Review the selected FastAPI code. Report findings only.

## Before Reviewing

```bash
python -m pytest --tb=short
python -m mypy .
bandit -r .
```

## Review Priorities

### CRITICAL — Security

- Hardcoded secrets — use `pydantic_settings.BaseSettings` with environment variables
- SQL injection via f-strings in queries — use SQLAlchemy bound parameters or ORM
- Auth / password fields in Pydantic response schemas — use separate response models excluding sensitive fields
- Auth dependency missing on protected routes — verify every router prefix and endpoint has a dependency
- `allow_origins=["*"]` combined with `allow_credentials=True` — invalid CORS config; browsers reject this

### CRITICAL — Async Correctness

- Blocking I/O (database queries, file reads, HTTP calls via `requests`) inside `async def` routes without using async libraries — use async drivers or `asyncio.run_in_executor`
- `time.sleep()` in an async route — use `await asyncio.sleep()`

### HIGH — Database Session Handling

- Creating database session inside route handler body instead of using a dependency — leads to session leaks on exceptions
- Missing `finally` / context manager to close session in manual management
- N+1 queries — use `selectinload` or `joinedload` in SQLAlchemy

### HIGH — Pydantic Schemas

- Missing `response_model` on endpoint — no response filtering; internal fields may leak
- Input schema missing validators for constrained strings (length, pattern)
- Using `.dict()` instead of `.model_dump()` on Pydantic v2 models

### HIGH — Dependency Injection

- Business logic inline in route handler — extract to service or dependency
- Test dependency overrides incorrect — use `app.dependency_overrides`

### MEDIUM — Production Readiness

- No rate limiting on public endpoints — add `slowapi` or reverse-proxy limits
- Missing `/health` endpoint
- No request ID header for distributed tracing

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

---
mode: agent
description: Production readiness audit — check if an application is safe to ship by inspecting auth, data, error handling, env config, CI, and rollback
---

# Production Audit

Audit this application for production readiness. Do not send repository contents to external services.

## What to Check

Work through each domain. Mark each item as: PASS / WARN / BLOCK.

---

### 1 — Build and CI

```bash
git log --oneline -10             # Recent changes
git status --short --branch       # Uncommitted work
```

- [ ] CI pipeline runs and passes on the main branch
- [ ] All tests pass (unit + integration + E2E)
- [ ] Build output is reproducible with pinned dependencies
- [ ] No `console.log` / `print` debug statements in critical paths

---

### 2 — Authentication and Authorization

- [ ] Every endpoint that mutates data requires authentication
- [ ] Authorization is checked server-side for every sensitive operation
- [ ] JWT expiry and rotation is configured
- [ ] Passwords hashed with bcrypt / argon2 (not MD5, SHA1, or plaintext)
- [ ] Session tokens are HttpOnly, Secure, SameSite=Strict

---

### 3 — Secrets and Configuration

```bash
grep -r "password\|secret\|api_key\|token" --include="*.env*" .
grep -r "hardcoded\|TODO.*secret" --include="*.ts" --include="*.js" --include="*.py" .
```

- [ ] No hardcoded secrets in source code
- [ ] All required environment variables are documented (`.env.example`)
- [ ] Production env vars are validated at startup
- [ ] `.env` files are in `.gitignore`

---

### 4 — Data and Database

- [ ] All user inputs are validated before DB writes
- [ ] No raw string interpolation in SQL queries
- [ ] Migrations have a `down` / rollback path
- [ ] Sensitive PII fields are not logged
- [ ] Pagination or limits on all list endpoints (no unbounded queries)

---

### 5 — Error Handling and Observability

- [ ] Errors are caught and logged at the boundary
- [ ] Error responses do not expose stack traces to end users
- [ ] Structured logging with request IDs for distributed tracing
- [ ] Health check endpoint exists (`/health` or equivalent)
- [ ] Alerts exist for 5xx error spikes

---

### 6 — Infrastructure and Deployment

- [ ] Rollback plan documented and tested
- [ ] Zero-downtime deploy strategy (rolling / blue-green / canary)
- [ ] Dependencies pinned (Docker image tags, package lock files committed)
- [ ] Resource limits set on containers (CPU, memory)
- [ ] Rate limiting on all public endpoints

---

### 7 — Security Headers (Web apps)

- [ ] `Content-Security-Policy` header set
- [ ] `X-Frame-Options: DENY` or `SAMEORIGIN`
- [ ] `Strict-Transport-Security` with long max-age
- [ ] CORS `Access-Control-Allow-Origin` is not `*` for authenticated APIs

---

## Output Format

```
## Production Audit Results

### BLOCK (must fix before shipping)
- [Issue description and fix]

### WARN (should fix, not blocking)
- [Issue description and recommendation]

### PASS
- [List of passing checks]

## Verdict: SHIP / HOLD
Reason: [One paragraph summary]
```

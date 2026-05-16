---
mode: agent
description: Full verification loop — build, types, lint, tests, and coverage gate before committing or opening a PR
---

# Verify

Run all quality gates and report the result. Use this before committing or opening a PR.

## Phase 1 — Build

```bash
# Node / TypeScript
npm run build 2>&1 | tail -20

# Python
python -m py_compile $(find . -name "*.py" | head -20) 2>&1

# Go
go build ./... 2>&1

# Rust
cargo build 2>&1 | tail -20
```

**Gate:** Build must pass. If it fails, stop here and fix before continuing.

---

## Phase 2 — Type Check

```bash
# TypeScript
npx tsc --noEmit 2>&1 | head -40

# Python (if pyright configured)
pyright . 2>&1 | head -30

# Go (included in build)
go vet ./... 2>&1
```

Report all type errors. CRITICAL errors must be fixed.

---

## Phase 3 — Lint

```bash
# JS / TS
npx eslint . --max-warnings 0 2>&1 | head -40

# Python
ruff check . 2>&1 | head -30

# Go
golangci-lint run 2>&1 | head -30

# Rust
cargo clippy -- -D warnings 2>&1 | head -30
```

Report errors vs warnings. Fix errors; warnings are advisory.

---

## Phase 4 — Tests

```bash
# Node
npm test -- --coverage 2>&1 | tail -40

# Python
python -m pytest --tb=short --cov . 2>&1 | tail -40

# Go
go test ./... -count=1 2>&1 | tail -40

# Rust
cargo test 2>&1 | tail -30
```

**Coverage gate:** Minimum 80% line coverage.

---

## Phase 5 — Security Quick Check

```bash
# Node — known vulnerable packages
npm audit --audit-level=high 2>&1 | tail -20

# Python
pip-audit 2>&1 | tail -20
```

---

## Output Format

```
## Verification Report

### Build     ✓ PASS | ✗ FAIL
[Output if failed]

### Types     ✓ PASS | ✗ FAIL | ⚠ WARN
[Issues found]

### Lint      ✓ PASS | ✗ FAIL | ⚠ WARN
[Issues found]

### Tests     ✓ PASS | ✗ FAIL
Coverage: X% (target: 80%)
[Failing tests if any]

### Security  ✓ PASS | ✗ FAIL
[Vulnerabilities found]

---
## Overall: READY TO COMMIT / NEEDS FIXES
[Specific blockers if any]
```

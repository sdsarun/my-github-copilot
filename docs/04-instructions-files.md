# 04 — Instructions Files: Context-Specific Rules

## What They Are

`.instructions.md` files are Markdown files with a YAML frontmatter block that includes an `applyTo` pattern. When you open a file that matches the pattern, VS Code automatically loads the instructions into Copilot's context.

This means you can have different rules activate automatically depending on which file you are editing — without manually attaching anything.

---

## How the `applyTo` Pattern Works

The pattern is a glob. Examples:

```yaml
# Apply to ALL files in the project
applyTo: "**"

# Apply only to test files
applyTo: "**/*.test.ts,**/*.test.js,**/*.spec.ts"

# Apply only to files in the routes folder
applyTo: "**/routes/**,**/api/**"

# Apply only to specific files
applyTo: "src/auth/**,src/payment/**"

# Apply to a specific language
applyTo: "**/*.py"
```

---

## Where to Put Them

You can put `.instructions.md` files anywhere in your project. VS Code discovers them by scanning for files that match the pattern relative to your workspace root.

The convention used in this toolkit is to put them in an `instructions/` folder:

```
your-project/
├── instructions/
│   ├── general.instructions.md      ← applies to all files
│   ├── testing.instructions.md      ← applies to *.test.* files
│   ├── api.instructions.md          ← applies to routes/ and api/ folders
│   └── security.instructions.md     ← applies to auth/ and payment/
```

But you can also put them in the root:

```
your-project/
├── general.instructions.md
├── testing.instructions.md
```

Or co-located with the code:

```
your-project/
├── src/
│   ├── auth/
│   │   ├── .instructions.md         ← only loads when editing auth files
│   │   └── authService.ts
```

---

## The Instructions Files in This Toolkit

### `general.instructions.md`

**Pattern:** `**` (all files)

Always loaded. Contains the baseline coding standards:

- Immutability rules with code examples
- Error handling rules with bad/good patterns
- Input validation requirements
- Naming conventions
- Size limits (50-line functions, 800-line files)
- No hardcoded values rule

### `testing.instructions.md`

**Pattern:** `**/*.test.ts, **/*.test.js, **/*.spec.ts, **/*.spec.js, **/__tests__/**`

Loads when you open any test file. Contains:

- AAA (Arrange-Act-Assert) structure with examples
- Test naming rules
- Coverage requirements
- TDD order (test first, then implementation)
- What not to do (skip, comment-out, always-passing assertions)
- Mocking rules (mock at system boundaries, not internals)

### `api.instructions.md`

**Pattern:** `**/routes/**, **/api/**, **/controllers/**, **/handlers/**`

Loads when you open any route or handler file. Contains:

- URL naming rules (nouns, plural, kebab-case)
- HTTP status code reference table
- Input validation requirement for every handler
- Consistent response envelope pattern
- Pagination format
- Security checklist

### `security.instructions.md`

**Pattern:** `**/auth/**, **/payment/**, **/billing/**, **/admin/**, **/middleware/**`

Loads when you open sensitive security-relevant files. Contains:

- Secret management rules with bad/good examples
- Authentication patterns (never trust client-supplied IDs)
- Authorization check examples
- Parameterized query enforcement
- Error response scrubbing rules
- Pre-commit security checklist

### `backend.instructions.md`

**Pattern:** `**/services/**, **/repositories/**, **/middleware/**, **/server/**, **/lib/**`

Loads when you edit service or repository files. Contains:

- Layer boundary rules (route → service → repository → DB, no crossing)
- Repository pattern interface example
- Service layer rules (one service per domain, no raw DB calls)
- Middleware pattern (auth middleware, never trust client-supplied user IDs)
- Error propagation (let errors bubble up, handle at route level)
- Caching rules (always set TTL, cache at service layer, invalidate on write)

### `frontend.instructions.md`

**Pattern:** `**/components/**, **/*.tsx, **/pages/**, **/app/**, **/hooks/**`

Loads when you edit React components, pages, or custom hooks. Contains:

- Composition over monolithic components
- State rules (useState vs useReducer, no derived state in state, no mutation)
- Data fetching patterns (React Query / SWR over raw useEffect)
- Performance rules (React.memo, useMemo, useCallback, no inline objects as props)
- Virtualization for long lists
- Custom hooks pattern
- Accessibility baseline

### `e2e.instructions.md`

**Pattern:** `**/e2e/**, **/*.spec.ts, **/*.spec.js, **/playwright/**, **/tests/e2e/**`

Loads when you edit Playwright E2E test files. Contains:

- Page Object Model (POM) structure with full example
- Selector priority (data-testid → ARIA roles → text → CSS)
- Waiting rules (never `waitForTimeout`, always wait for specific conditions)
- Test independence requirements (no shared state between tests)
- What to test in E2E vs unit/integration
- Flaky test handling

### `git.instructions.md`

**Pattern:** `**` (all files)

Always loaded alongside `general.instructions.md`. Contains:

- Conventional commit format (feat/fix/refactor/test/docs/chore/perf/ci)
- Branch naming convention (`<type>/<short-description>`)
- Pull request rules (one logical change, all CI checks pass)
- History hygiene (interactive rebase before PR, no force-push to shared branches)

### `error-handling.instructions.md`

**Pattern:** `**` (all files)

Always loaded. Contains:

- Never silently swallow errors
- Two-audience rule: users see friendly messages, developers see full context
- Result / Either pattern examples
- Wrapping errors with context on rethrow
- Logging rules (log where handled, include structured fields, never log sensitive data)
- HTTP status code reference for API error responses

### `golang.instructions.md`

**Pattern:** `**/*.go`

Loads when you edit any Go file. Contains:

- Accept interfaces, return concrete types pattern
- Interface definition location (in the consumer package)
- Error wrapping with `fmt.Errorf("context: %w", err)`
- Sentinel errors and custom error types
- Goroutine ownership and cancellation with `context.Context`
- `gofmt` and zero-value design principles

### `rust.instructions.md`

**Pattern:** `**/*.rs`

Loads when you edit any Rust file. Contains:

- Prefer references over cloning
- `anyhow` for applications, `thiserror` for libraries
- Use `?` for error propagation, never unwrap in production
- `impl Trait` vs `Box<dyn Trait>` decision guide
- Async with tokio, `spawn_blocking` for CPU-bound work
- `unsafe` block scope discipline

### `python.instructions.md`

**Pattern:** `**/*.py`

Loads when you edit any Python file. Contains:

- PEP 8 with `ruff` or `flake8`
- Type annotations on all function signatures
- EAFP (try/except) over LBYL (check before access)
- Mutable default argument anti-pattern
- Explicit logging configuration with `logging.getLogger(__name__)`
- `pytest` with fixtures and `parametrize`

### `docker.instructions.md`

**Pattern:** `**/Dockerfile, **/Dockerfile.*, **/docker-compose*.yml, **/.dockerignore`

Loads when you edit Docker or Compose files. Contains:

- Pinned base image versions (never `latest`)
- Multi-stage build pattern
- Non-root user for runtime stage
- Named volumes (never anonymous)
- Health checks on service dependencies
- No secrets in Dockerfile or Compose — use `.env` files and secrets managers

### `migrations.instructions.md`

**Pattern:** `**/migrations/**, **/db/migrations/**, **/database/migrations/**, **/alembic/**`

Loads when you edit database migration files. Contains:

- Forward-only migrations (never edit merged migration files)
- Reversible migrations with `down` / `downgrade`
- Expand-contract pattern for column removal and renames
- `CREATE INDEX CONCURRENTLY` for production databases
- One logical change per migration file
- Migration testing in CI

### `deployment.instructions.md`

**Pattern:** `**/deploy/**, **/k8s/**, **/kubernetes/**, **/terraform/**, **/.github/workflows/**, **/helm/**`

Loads when you edit deployment and infrastructure files. Contains:

- Deployment strategy guide (Rolling / Blue-Green / Canary / Feature Flags)
- Kubernetes resource limits, readiness/liveness probes, PodDisruptionBudget
- CI/CD rules (never deploy without passing tests, immutable artifact tags)
- Terraform remote state and module conventions
- Secrets manager usage (never in source control or CI plain text)

---

## How to Create Your Own

### Step 1 — Create the file

```markdown
---
applyTo: '**/services/**'
---

# Service Layer Standards

Rules that apply when editing service files.

## Dependency Injection

Always inject dependencies through the constructor...

## Transaction Handling

Wrap multiple database writes in a transaction...
```

### Step 2 — Name it descriptively

Use the format `description.instructions.md`. Good names:

- `services.instructions.md`
- `database.instructions.md`
- `components.instructions.md`
- `python.instructions.md`

### Step 3 — Verify it loads

Open a file that matches your pattern, then ask Copilot a question. The answer should reflect your instructions.

---

## Tips

- **Be specific in the `applyTo` pattern** — if you use `**` for everything, you can get conflicts. Use the general instructions file for universal rules and specific files for narrower concerns.
- **Include code examples** — rules with `// WRONG` / `// CORRECT` examples are much more effective than prose descriptions alone.
- **Keep each file focused** — one topic per instructions file is easier to maintain.
- **Use multiple patterns** — separate patterns with commas: `applyTo: "**/auth/**,**/session/**"`

---

## Combining All Three Layers

When Copilot answers a question, it combines:

1. `copilot-instructions.md` — always loaded
2. Any `.instructions.md` files whose patterns match the current file
3. The prompt file (if you invoked a slash command)
4. Any files or context you explicitly attached

This layering means the more context you give Copilot about what you are doing, the more precisely it can follow your project's rules.

---

## Next Steps

Continue to [05-everyday-workflows.md](05-everyday-workflows.md) for real-world developer workflows showing how to combine all three layers.

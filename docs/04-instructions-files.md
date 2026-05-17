# 04 — Instructions Files: Context-Specific Rules

## What They Are

`.instructions.md` files are Markdown files with a YAML frontmatter block that includes an `applyTo` pattern. When you open a file that matches the pattern, VS Code automatically loads the instructions into Copilot's context.

This means you can have different rules activate automatically depending on which file you are editing — without manually attaching anything.

---

## Where to Put Them

**Default location:** `.github/instructions/` — VS Code searches this folder **recursively**.

This is the only folder VS Code looks in by default. Do not put them at the project root or in random locations — they will not be discovered.

```
your-project/
└── .github/
    └── instructions/
        ├── core/
        │   ├── general.instructions.md       ← applyTo: **
        │   └── error-handling.instructions.md
        ├── testing/
        │   └── testing.instructions.md       ← applyTo: **/*.test.ts
        └── security/
            └── security.instructions.md      ← applyTo: **/auth/**
```

VS Code explicitly supports — and recommends — organizing instructions in subdirectories. The recursion means any nesting structure works.

> **If you want to add other locations**, configure `chat.instructionsFilesLocations` in VS Code settings. But `.github/instructions/` is always discovered by default without any extra config.

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

# Apply only to specific folders
applyTo: "src/auth/**,src/payment/**"

# Apply to a specific language
applyTo: "**/*.go"
```

---

## The Instructions Files in This Toolkit

### `core/general.instructions.md`

**Pattern:** `**` (all files)

Always loaded. Contains the baseline coding standards:

- Immutability rules with code examples
- Error handling rules with bad/good patterns
- Input validation requirements
- Naming conventions
- Size limits (50-line functions, 800-line files)
- No hardcoded values rule

### `core/error-handling.instructions.md`

**Pattern:** `**` (all files)

Always loaded alongside `general.instructions.md`. Contains:

- Never silently swallow errors
- Two-audience rule: users see friendly messages, developers see full context
- Result / Either pattern examples
- Wrapping errors with context on rethrow
- Logging rules (log where handled, include structured fields, never log sensitive data)
- HTTP status code reference for API error responses

### `git/git.instructions.md`

**Pattern:** `**` (all files)

Always loaded. Contains:

- Conventional commit format (feat/fix/refactor/test/docs/chore/perf/ci)
- Branch naming convention (`<type>/<short-description>`)
- Pull request rules (one logical change, all CI checks pass)
- History hygiene (interactive rebase before PR, no force-push to shared branches)

### `testing/testing.instructions.md`

**Pattern:** `**/*.test.ts, **/*.test.js, **/*.spec.ts, **/*.spec.js, **/__tests__/**`

Loads when you open any test file. Contains:

- AAA (Arrange-Act-Assert) structure with examples
- Test naming rules
- Coverage requirements
- TDD order (test first, then implementation)
- What not to do (skip, comment-out, always-passing assertions)
- Mocking rules (mock at system boundaries, not internals)

### `testing/e2e.instructions.md`

**Pattern:** `**/e2e/**, **/*.spec.ts, **/*.spec.js, **/playwright/**, **/tests/e2e/**`

Loads when you edit Playwright E2E test files. Contains:

- Page Object Model (POM) structure with full example
- Selector priority (data-testid → ARIA roles → text → CSS)
- Waiting rules (never `waitForTimeout`, always wait for specific conditions)
- Test independence requirements (no shared state between tests)

### `architecture/api.instructions.md`

**Pattern:** `**/routes/**, **/api/**, **/controllers/**, **/handlers/**`

Loads when you open any route or handler file. Contains:

- URL naming rules (nouns, plural, kebab-case)
- HTTP status code reference table
- Input validation requirement for every handler
- Consistent response envelope pattern
- Pagination format
- Security checklist

### `security/security.instructions.md`

**Pattern:** `**/auth/**, **/payment/**, **/billing/**, **/admin/**, **/middleware/**`

Loads when you open sensitive security-relevant files. Contains:

- Secret management rules with bad/good examples
- Authentication patterns (never trust client-supplied IDs)
- Authorization check examples
- Parameterized query enforcement
- Error response scrubbing rules
- Pre-commit security checklist

### `backend/backend.instructions.md`

**Pattern:** `**/services/**, **/repositories/**, **/middleware/**, **/server/**, **/lib/**`

Loads when you edit service or repository files. Contains:

- Layer boundary rules (route → service → repository → DB, no crossing)
- Repository pattern interface example
- Service layer rules (one service per domain, no raw DB calls)
- Error propagation (let errors bubble up, handle at route level)
- Caching rules (always set TTL, cache at service layer, invalidate on write)

### `frontend/frontend.instructions.md`

**Pattern:** `**/components/**, **/*.tsx, **/pages/**, **/app/**, **/hooks/**`

Loads when you edit React components, pages, or custom hooks. Contains:

- Composition over monolithic components
- State rules (useState vs useReducer, no derived state in state, no mutation)
- Data fetching patterns (React Query / SWR over raw useEffect)
- Performance rules (React.memo, useMemo, useCallback, no inline objects as props)
- Custom hooks pattern
- Accessibility baseline

### `languages/go/golang.instructions.md`

**Pattern:** `**/*.go`

Loads when you edit any Go file. Contains:

- Accept interfaces, return concrete types pattern
- Interface definition location (in the consumer package)
- Error wrapping with `fmt.Errorf("context: %w", err)`
- Sentinel errors and custom error types
- Goroutine ownership and cancellation with `context.Context`

### `devops/docker.instructions.md`

**Pattern:** `**/Dockerfile, **/Dockerfile.*, **/docker-compose*.yml, **/.dockerignore`

Loads when you edit Docker or Compose files. Contains:

- Pinned base image versions (never `latest`)
- Multi-stage build pattern
- Non-root user for runtime stage
- Named volumes (never anonymous)
- No secrets in Dockerfile — use `.env` files and secrets managers

### `devops/deployment.instructions.md`

**Pattern:** `**/k8s/**, **/.github/workflows/**, **/terraform/**`

Loads when you edit Kubernetes, GitHub Actions, or Terraform files.

### `database/postgres.instructions.md`

**Pattern:** `**/*.sql, **/migrations/**, **/db/**`

### `database/prisma.instructions.md`

**Pattern:** `**/schema.prisma, **/prisma/**`

### `database/migrations.instructions.md`

**Pattern:** `**/migrations/**`

Contains: forward-only migrations, expand-contract pattern, `CREATE INDEX CONCURRENTLY`.

### `database/redis.instructions.md`

**Pattern:** `**/redis/**, **/cache/**`

### `code-quality/vite.instructions.md`

**Pattern:** `**/vite.config.*`

### Framework instructions

| File | Pattern |
|---|---|
| `frameworks/nestjs/nestjs.instructions.md` | `**/*.module.ts, **/*.controller.ts, **/*.service.ts` |
| `frameworks/nextjs/nextjs.instructions.md` | `**/app/**, **/pages/**, **/layout.tsx` |
| `frameworks/angular/angular.instructions.md` | `**/*.component.ts, **/*.service.ts` |

---

## File Format

```yaml
---
name: "Testing Standards"
description: "Test structure and TDD rules for test files"
applyTo: "**/*.test.ts,**/*.spec.ts"
---

# Testing Standards

Always use AAA structure...
```

| Field | Required | What it does |
|---|---|---|
| `name` | No | Display name in the UI. Defaults to filename |
| `description` | No | Short description shown on hover |
| `applyTo` | No | Glob pattern for auto-application. If omitted, the file is never applied automatically |

---

## Verification

To check which instruction files are loaded:

1. Open Copilot Chat
2. Right-click in the chat panel and select **Diagnostics**
3. You will see all loaded instruction files and any errors

---

## Next Steps

Continue to [05-everyday-workflows.md](05-everyday-workflows.md) for real developer workflows using these tools together.

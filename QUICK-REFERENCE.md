# Quick Reference — GitHub Copilot Supercharged

Pin this. Open it when you sit down to work.

---

## Every Day Checklist

```
Starting a task?            → /plan first, then code
Writing code?               → /tdd to write tests first
Done writing code?          → /code-review before committing
All gates before commit?    → /verify (build + types + lint + tests)
Ready to ship?              → /prod-audit
New to a codebase?          → /onboard
TS/JS project?              → /ts-review for type-safety check
Python project?             → /py-review
Touching database?          → /db-review
Touching auth/secrets?      → /security-review
Errors being swallowed?     → /error-audit
Build is broken?            → /build-fix
Code messy?                 → /refactor or /simplify
Something is slow?          → /perf-review
Designing the system?       → /arch-review or /hex-arch
Building an API?            → /api-design
Need E2E tests?             → /e2e
Updating documentation?     → /update-docs
Preparing a PR?             → /git-pr
Reviewing a PR?             → /pr-review
Types need design review?   → /type-design
Accessibility check?        → /a11y-review
SEO check?                  → /seo-review
ML pipeline review?         → /mle-review
Recording a design choice?  → /adr
GitHub issues/PRs/CI?       → /github-ops
```

```

---

## Slash Commands (type in Copilot Chat)

### Planning & Writing

| Command        | When to Use                     | What It Does                               |
| -------------- | ------------------------------- | ------------------------------------------ |
| `/plan`        | Before any feature > 1 function | Creates a phased implementation plan       |
| `/tdd`         | Before writing any new code     | Guides you through RED → GREEN → IMPROVE   |
| `/e2e`         | Need E2E tests for a user flow  | Generates Playwright tests with POM        |
| `/update-docs` | After shipping a feature        | Updates README, JSDoc, changelog           |
| `/git-pr`      | Preparing a branch for PR       | Commit messages, PR description, checklist |
| `/onboard`     | Joining an unfamiliar codebase  | Maps stack, entry points, and conventions  |
| `/adr`         | After a significant design choice | Structured Architecture Decision Record  |
| `/verify`      | Before committing / opening PR  | Build + types + lint + tests + coverage    |
| `/prod-audit`  | Before shipping to production   | Auth, secrets, errors, CI, rollback check  |

### Code Review

| Command            | When to Use                    | What It Does                             |
| ------------------ | ------------------------------ | ---------------------------------------- |
| `/code-review`     | After writing code             | Reviews security, quality, errors, tests |
| `/pr-review`       | Reviewing a pull request       | Behavioral coverage, logic gaps, risk    |
| `/security-review` | Auth, payments, sensitive code | Deep OWASP security scan                 |
| `/error-audit`     | Suspect error handling         | Hunts swallowed errors and bad fallbacks |
| `/perf-review`     | Something is slow or heavy     | Complexity, bundle size, N+1, rendering  |
| `/arch-review`     | Designing a system             | Layers, scalability, trade-offs          |
| `/hex-arch`        | Ports & Adapters design        | Domain independence, clean boundaries    |
| `/api-design`      | Designing endpoints            | Naming, status codes, structure          |
| `/db-review`       | SQL, schema, migrations        | Indexes, injection, schema design        |
| `/type-design`     | Type or data model review      | Illegal states, primitive obsession      |
| `/a11y-review`     | UI / frontend code             | WCAG 2.2, keyboard nav, screen readers   |
| `/seo-review`      | Web pages                      | Crawlability, meta, Core Web Vitals      |
| `/mle-review`      | ML pipeline code               | Leakage, eval correctness, monitoring    |

### Language Reviews

| Command              | Language / Framework | What It Checks                          |
| -------------------- | -------------------- | --------------------------------------- |
| `/ts-review`         | TypeScript / JS      | Type safety, async, idiomatic           |
| `/py-review`         | Python               | PEP 8, type hints, security             |
| `/go-review`         | Go                   | Goroutines, interfaces, error wrapping  |
| `/rust-review`       | Rust                 | Ownership, unsafe, Result types         |
| `/java-review`       | Java / Spring Boot   | N+1, @Transactional, injection          |
| `/kotlin-review`     | Kotlin / Android     | Coroutines, Compose, clean arch         |
| `/swift-review`      | Swift / iOS          | Force unwrap, ARC, Swift Concurrency    |
| `/csharp-review`     | C# / .NET            | Async void, nullable, injection         |
| `/cpp-review`        | C++                  | Memory safety, smart pointers, races    |
| `/angular-review`    | Angular              | Signals, OnPush, guards, forms          |
| `/nestjs-review`     | NestJS               | Modules, guards, validation, DI         |
| `/laravel-review`    | Laravel / PHP        | N+1 ORM, Form Requests, policies        |
| `/django-review`     | Django / DRF         | N+1, migrations, mark_safe, security    |
| `/fastapi-review`    | FastAPI              | Async I/O, deps, Pydantic, CORS         |
| `/flutter-review`    | Flutter / Dart       | Null safety, state, widget arch         |
| `/fsharp-review`     | F#                   | Functional idioms, DUs, pattern match   |

### Build Fixers

| Command          | Language     | What It Does                              |
| ---------------- | ------------ | ----------------------------------------- |
| `/build-fix`     | Any          | Diagnoses and fixes build errors          |
| `/go-build`      | Go           | Undefined symbols, import cycles          |
| `/rust-build`    | Rust         | Borrow checker, lifetime, trait errors    |
| `/java-build`    | Java         | Maven/Gradle, cannot find symbol          |
| `/kotlin-build`  | Kotlin       | Gradle, unresolved reference, coroutines  |
| `/dart-build`    | Dart/Flutter | Null safety, pub, codegen errors          |

### Fixing, Cleanup & Ops

| Command         | When to Use           | What It Does                              |
| --------------- | --------------------- | ----------------------------------------- |
| `/refactor`     | Code is messy         | Dead code removal, simplification         |
| `/simplify`     | Too much nesting      | Early returns, shorter functions          |
| `/github-ops`   | GitHub issue/PR/CI    | Triage, merge, release, CI debug via `gh` |

---

## Specialist Agents (agent picker or `@agent-name`)

Agents are focused personas with specific tools. Select from the agent picker (`@`) in Copilot Chat.

| Agent                       | When to Use                               |
| --------------------------- | ----------------------------------------- |
| `@planner`                  | Plan a feature before writing code        |
| `@architect`                | System design, trade-offs, ADRs           |
| `@code-reviewer`            | Deep code review with git diff context    |
| `@tdd-guide`                | Red-Green-Refactor TDD cycle              |
| `@security-reviewer`        | OWASP Top 10 + secrets + injection audit  |
| `@build-error-resolver`     | Fix build/type errors with minimal diffs  |
| `@refactor-cleaner`         | Dead code, simplification, deduplication  |
| `@performance-optimizer`    | Profile, N+1 queries, bundle size         |
| `@e2e-runner`               | Write and run Playwright tests            |
| `@doc-updater`              | Keep docs in sync with code               |
| `@silent-failure-hunter`    | Find swallowed exceptions and bad catch   |
| `@type-design-analyzer`     | Evaluate type design and illegal states   |
| `@code-explorer`            | Map how a feature works end-to-end        |
| `@code-simplifier`          | Clean up newly written code               |
| `@pr-test-analyzer`         | Check if PR tests actually catch bugs     |
| `@database-reviewer`        | PostgreSQL schema, indexes, SQL security  |
| `@a11y-architect`           | WCAG 2.2 accessibility review             |
| `@mle-reviewer`             | ML pipeline, leakage, serving, monitoring |

---

## Skills (auto-loaded or `/skill-name`)

Skills are loaded automatically when relevant, or type `/` to invoke manually.

| Skill                  | Description                                     |
| ---------------------- | ----------------------------------------------- |
| `coding-standards`     | Naming, immutability, DRY, KISS baselines       |
| `tdd-workflow`         | Full TDD cycle with coverage requirements       |
| `security-review`      | Security patterns, OWASP checklist              |
| `e2e-testing`          | Playwright Page Object Model, CI integration    |
| `api-design`           | REST naming, status codes, pagination, errors   |
| `backend-patterns`     | Repository, service, validation, error handling |
| `frontend-patterns`    | React, state, forms, performance                |
| `verification-loop`    | Build + types + lint + tests quality gate       |
| `bun-runtime`          | Bun vs Node, migration, Vercel support          |
| `documentation-lookup` | Fetch live library docs via Context7            |

---

## Copilot Chat Tips

```

# Attach a file for context

Type @ then the filename

# Ask about your whole codebase

@workspace what does the auth module do?

# Use agent mode for multi-step work

Switch to "Agent" mode in the chat panel

```

---

## Coding Rules (always enforced by copilot-instructions.md)

```

✓ Never mutate objects — always return a new copy
✓ Functions under 50 lines
✓ Files under 800 lines
✓ No secrets in code — use env vars
✓ Validate all user input at boundaries
✓ Handle errors explicitly — never swallow them
✓ Conventional commits: feat/fix/docs/test/chore/refactor

```

---

## Commit Format

```

feat: add user authentication endpoint
fix: correct null check in payment processor
test: add unit tests for order validation
docs: update API reference for /users endpoint
refactor: extract validation logic into helper
chore: upgrade dependencies

```

---

## When Something Goes Wrong

| Problem                       | Action                                                      |
| ----------------------------- | ----------------------------------------------------------- |
| Copilot ignores rules         | Check `.github/copilot-instructions.md` exists              |
| Slash command not showing     | Check file is in `.github/prompts/` and named `*.prompt.md` |
| Rules not applying to a file  | Check `applyTo` pattern in `.instructions.md` frontmatter   |
| Copilot gives generic answers | Add more context with `@workspace` or attach files          |
```

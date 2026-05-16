# 03 — Prompt Files: Slash Commands for Deep Workflows

## What They Are

Prompt files are Markdown files inside `.github/prompts/` that end in `.prompt.md`. GitHub Copilot turns each one into a slash command that you can invoke from Copilot Chat.

When you type `/` in Copilot Chat, you will see a dropdown list of your prompt files. Selecting one sends the full contents of the file to Copilot as the prompt for that conversation.

---

## How to Use Them

1. Open **Copilot Chat** in VS Code (the chat panel on the left sidebar, or `Ctrl+Shift+I` / `Cmd+Shift+I`).
2. Type `/` — a list of available prompts appears.
3. Select or type the name of the prompt you want.
4. Add any additional context if needed (e.g., select some code first, then invoke `/code-review`).

---

## The Prompts in This Toolkit

### `/plan` — Implementation Planner

**Use this before writing any code for a feature.**

It produces a structured plan with:

- One-sentence goal statement
- Reuse opportunities (existing code you can leverage)
- External dependencies
- Phased task breakdown (data model → business logic → API → UI)
- Identified risks
- Definition of done

**When to use:** Starting any feature that involves more than one function or file.

**Example:**

```
You: /plan
     I need to add email verification to the sign-up flow.
     Users should get a verification email and cannot log in until verified.

Copilot: ## Goal
         Add email verification to sign-up: send link on registration,
         block login until verified.

         ## Reuse Opportunities
         - Existing sendEmail() utility in src/lib/email.ts
         - User model already has id field for token generation

         ## Dependencies
         - VERIFICATION_TOKEN_SECRET env var (needs to be added)
         - Email template for verification link

         ## Phases
         ### Phase 1 — Data Model
         - [ ] Add verifiedAt column to users table
         - [ ] Add verificationToken column

         ### Phase 2 — Business Logic + Unit Tests
         ...
```

---

### `/tdd` — Test-Driven Development

**Use this whenever you are writing new code.**

It enforces the RED → GREEN → IMPROVE cycle:

1. Write the failing test first
2. Write minimal implementation to make it pass
3. Refactor while keeping tests green

**When to use:** Starting any new function, endpoint, component, or bug fix.

**Example:**

```
You: /tdd
     I need a function that validates an email address and
     returns a structured result object.

Copilot: ## Step 1 — RED: Write the failing test first

         test('returns valid:true for a properly formatted email', () => {
           const result = validateEmail('user@example.com')
           expect(result.valid).toBe(true)
           expect(result.error).toBeNull()
         })

         test('returns valid:false with error message for missing @', () => {
           const result = validateEmail('notanemail')
           expect(result.valid).toBe(false)
           expect(result.error).toBe('Email must contain @')
         })

         Run these — they should fail. Then continue to Step 2...
```

---

### `/code-review` — Code Quality & Security Review

**Use this after writing or modifying code, before committing.**

It reviews your code across four dimensions:

1. Security (hardcoded secrets, injection risks, missing auth)
2. Code quality (mutation, function length, naming, duplication)
3. Error handling (swallowed errors, missing async error handling)
4. Test coverage (missing tests, always-passing assertions)

**When to use:** After completing a feature, before pushing a PR.

**Example:** Select the code you want reviewed, then type `/code-review`.

```
You: [selects auth middleware code]
     /code-review

Copilot: **[CRITICAL]** — middleware/auth.ts:14
         Issue: User role read from req.body.role — client can forge this.
         Fix: Read role from req.session or decoded JWT, not request body.

         **[HIGH]** — middleware/auth.ts:28
         Issue: Error silently swallowed in catch block.
         Fix: Re-throw or return error response.

         ## Summary
         - Critical: 1
         - High: 1
         - Medium: 0
         - Approved to ship: no (fix CRITICAL and HIGH first)
```

---

### `/security-review` — Deep Security Analysis

**Use this before shipping any feature that touches auth, payments, or sensitive data.**

It runs through the OWASP Top 10 and checks for:

- Secrets in source code
- Input validation and injection risks
- Auth and authz gaps
- Data exposure in responses and logs
- Vulnerable dependencies
- Infrastructure concerns

**When to use:** Before merging any PR that touches sensitive areas.

---

### `/build-fix` — Build Error Resolution

**Use this when your build or CI is broken.**

It guides you through:

1. Categorizing the type of error (type error, import error, test failure, lint, etc.)
2. Picking the right fix strategy for that category
3. Verifying the fix
4. Checking for related issues caused by the same root problem

**When to use:** When `npm run build`, `tsc`, or CI fails and you want systematic help.

**Example:**

```
You: /build-fix
     TS2322: Type 'string | undefined' is not assignable to type 'string'.
     File: src/lib/config.ts:12

Copilot: Category: Type error — optional field used where required is expected.

         The issue: process.env.API_KEY is typed as string | undefined
         because env vars can be missing at runtime.

         Fix options:
         1. Add a startup validation that throws if API_KEY is missing,
            allowing you to assert the type: process.env.API_KEY!
         2. Handle the undefined case explicitly...
```

---

### `/refactor` — Code Cleanup

**Use this when code has gotten messy and needs cleanup without behavior changes.**

It targets:

- Dead code (unused imports, commented-out blocks, unreachable branches)
- Duplication (repeated logic that should be extracted)
- Structure (functions too long, files too large, too much nesting)
- Naming (variables that do not describe what they hold)

**When to use:** After a feature is working but the code is messy. Or during a dedicated cleanup sprint.

---

### `/api-design` — API Endpoint Review

**Use this when designing or reviewing REST API endpoints.**

It checks:

- URL naming (nouns, plural, kebab-case)
- HTTP method semantics
- Status code correctness
- Request validation
- Response structure consistency
- Security (auth, rate limiting)

**When to use:** When designing new endpoints or reviewing an API PR.

---

### `/ts-review` — TypeScript / JavaScript Review

**Use this for a language-specific review of TypeScript or JavaScript code.**

Covers things the general `/code-review` does not go as deep on:

- Type safety (`any`, non-null assertions, bad `as` casts)
- Async correctness (unhandled rejections, `forEach` + async, floating promises)
- Node.js specifics (sync file I/O in handlers, missing env var validation)
- React patterns (missing deps in hooks, state mutation)

**When to use:** Before merging any TypeScript or JavaScript PR.

---

### `/py-review` — Python Review

**Use this for a language-specific review of Python code.**

Covers:

- Security (SQL injection via f-strings, `eval`, `unsafe yaml.load`, hardcoded secrets)
- Pythonic patterns (mutable default arguments, `type() ==`, bare `except`, `print` vs `logging`)
- Type hints on public functions
- PEP 8 compliance

**When to use:** Before merging any Python PR.

---

### `/db-review` — Database Review

**Use this when reviewing SQL, schema files, or migrations.**

Covers:

- Missing indexes on WHERE/JOIN columns
- N+1 query patterns
- Parameterized queries (no string interpolation = injection risk)
- Schema type choices (bigint for IDs, timestamptz, numeric for money)
- Migration safety (locking large tables, missing down migrations)
- Row Level Security for multi-tenant data

**When to use:** Before merging any database schema change, migration, or complex query.

---

### `/error-audit` — Silent Failure Hunt

**Use this to find errors that are being hidden instead of handled.**

Hunts for:

- Empty catch blocks (`catch {}`)
- Functions that return `null` or `[]` when they should throw
- Fire-and-forget async calls with no error handling
- `async forEach` that drops errors silently
- Lost stack traces (re-throwing without `cause`)
- Missing try/catch around network, DB, and file operations

**When to use:** When code seems to "work" but things go wrong silently in production.

---

### `/perf-review` — Performance Review

**Use this when something is slow, the bundle is large, or the app feels sluggish.**

Covers:

- Algorithmic complexity (O(n²) patterns, nested loops, repeated array searches)
- Database N+1 queries and missing indexes
- Bundle size (large imports, missing code splitting)
- React rendering (missing memo, expensive inline objects, no virtualization)
- Node.js blocking calls in request handlers

**When to use:** After a performance complaint, before a launch, or during optimization work.

---

### `/arch-review` — Architecture Review

**Use this when planning a new system or reviewing a significant structural change.**

Covers:

- Layer separation (route handler → service → repository → DB)
- Modularity and coupling
- Scalability (stateless design, horizontal scaling, caching strategy)
- Common problems: fat controllers, god services, circular dependencies
- Trade-off analysis for design decisions

**When to use:** Before starting a significant new feature, or when reviewing an architectural PR.

---

### `/e2e` — E2E Test Generator

**Use this to generate Playwright end-to-end tests for a user flow.**

Generates tests using:

- Page Object Model (POM) structure
- `data-testid` selectors as first preference
- Explicit waits (no `waitForTimeout`)
- Independent, self-contained tests

**When to use:** When a critical user flow needs E2E coverage. Describe the flow and Copilot writes the test.

---

### `/update-docs` — Documentation Updater

**Use this to update or generate documentation after shipping code.**

Covers:

- README structure (what is this, how to run, how to test)
- JSDoc (only document what the code cannot say for itself)
- Changelog entries (Keep a Changelog format)
- Codemaps (project structure overview for `docs/CODEMAPS/`)

**When to use:** After a feature is complete, before opening the PR.

---

### `/git-pr` — PR Preparation

**Use this to prepare a branch for a pull request.**

Guides you through:

1. Analyzing all commits on the branch
2. Writing or improving commit messages in conventional format
3. Drafting a PR title and description with what/why/how/test-plan
4. Running a pre-flight checklist (tests pass, no secrets, correct scope)

**When to use:** When you are ready to open a PR and want everything polished.

---

### `/pr-review` — Pull Request Review

**Use this to review a pull request from a behavioral coverage angle.**

Covers:

- Identifying every behavior that was added, changed, or removed
- Assessing test coverage for each behavior (Full / Partial / Missing)
- Finding logic errors and edge cases not handled
- Security spot check on the changed code
- Overall risk rating (Low / Medium / High) with recommendation

**When to use:** When reviewing someone else's PR, or self-reviewing before merge.

---

### `/simplify` — Code Simplification

**Use this when code is working but unnecessarily complex.**

Targets:

- Deep nesting — converts to early returns and guard clauses
- Long functions — suggests extraction points
- Complex conditionals — extracts to named variables
- Unnecessary abstractions — inline one-off helpers
- Dead code and commented-out blocks

**When to use:** When you need more surgical simplification than a full refactor. Good for legacy code.

---

### `/type-design` — Type & Data Model Review

**Use this to review type definitions, interfaces, and data models.**

Checks for:

- Illegal states that the type allows but the domain does not
- Primitive obsession (raw `string`/`number` where a branded type prevents bugs)
- Constructors that do not validate invariants
- `Partial<T>` or many optionals where a discriminated union is more precise
- Mutable public fields that should be controlled

**When to use:** When designing a new domain model, or reviewing a PR that introduces new types.

---

### `/a11y-review` — Accessibility Review

**Use this to check frontend code against WCAG 2.2 Level AA.**

Covers:

- Alt text on images and icons
- Color contrast ratios (4.5:1 text, 3:1 UI components)
- Keyboard navigation and focus indicators
- Form inputs with associated labels and error messages
- Touch target sizes (44×44 CSS pixels)
- ARIA attributes and live regions

**When to use:** Before shipping any frontend feature, or when reviewing a UI PR.

---

### `/seo-review` — SEO Review

**Use this to audit web pages for technical SEO issues.**

Covers:

- `noindex` misconfigurations and robots.txt
- Canonical URL setup
- Title and meta description presence and length
- Core Web Vitals (LCP, CLS, INP)
- Structured data (JSON-LD, Open Graph, Twitter Card)
- XML sitemap

**When to use:** Before launching a web page or when SEO is a concern.

---

### `/mle-review` — ML Engineering Review

**Use this to review machine learning pipeline code.**

Covers:

- Data leakage (future data in features, preprocessing fitted before split)
- Evaluation correctness (wrong metric, test set used during tuning)
- Training reproducibility (random seeds, experiment tracking)
- Serving safety (input validation, latency guards, fallbacks)
- Monitoring (prediction drift, data drift, alerting)

**When to use:** Before deploying an ML model to production, or when reviewing ML pipeline code.

---

### Language Reviews

These follow the same pattern as `/ts-review` and `/py-review` but are tuned for specific languages and frameworks:

| Command           | Language           | Key Focus Areas                                             |
| ----------------- | ------------------ | ----------------------------------------------------------- |
| `/go-review`      | Go                 | Goroutines, import cycles, error wrapping, interface design |
| `/rust-review`    | Rust               | Ownership, unsafe blocks, Result/Option, async              |
| `/java-review`    | Java / Spring Boot | N+1, @Transactional, injection, checked exceptions          |
| `/kotlin-review`  | Kotlin / Android   | Coroutines, GlobalScope, Compose, clean architecture        |
| `/swift-review`   | Swift / iOS        | Force unwrap, ARC cycles, Swift Concurrency, actors         |
| `/csharp-review`  | C# / .NET          | async void, .Result deadlock, nullable, LINQ                |
| `/cpp-review`     | C++                | Smart pointers, buffer overflow, race conditions, RAII      |
| `/django-review`  | Django / DRF       | N+1 ORM, migrations, mark_safe XSS, settings security       |
| `/fastapi-review` | FastAPI            | Blocking async, response_model, CORS, DB sessions           |
| `/flutter-review` | Flutter / Dart     | Null assertion, SharedPreferences secrets, setState         |
| `/fsharp-review`  | F#                 | Pattern match coverage, mutable state, Railway patterns     |

---

### Build Fixers

These help fix compilation and build errors for specific toolchains:

| Command         | Toolchain             | Key Errors Covered                                |
| --------------- | --------------------- | ------------------------------------------------- |
| `/build-fix`    | Any                   | Generic — error categorization and fix strategy   |
| `/go-build`     | Go                    | Undefined symbols, import cycles, missing modules |
| `/rust-build`   | Rust                  | Borrow checker (E0382, E0502), lifetimes, traits  |
| `/java-build`   | Java / Maven / Gradle | cannot find symbol, annotation processing         |
| `/kotlin-build` | Kotlin / Gradle       | Unresolved reference, suspend context, Android    |
| `/dart-build`   | Dart / Flutter        | Null safety, pub conflicts, generated code        |

---

## How to Add Your Own Prompt

Create a file at `.github/prompts/my-workflow.prompt.md`:

```markdown
---
mode: agent
description: Short description of what this command does
---

# My Workflow Name

Instructions for Copilot when this command is invoked.

## Step 1

Do this first...

## Step 2

Then do this...
```

The filename becomes the slash command. `my-workflow.prompt.md` → `/my-workflow`.

---

## Tips

- **Attach context before invoking** — select the relevant code first, or use `@workspace` to give Copilot access to your codebase.
- **Be specific after the slash command** — type `/plan I need to add X feature that does Y` rather than just `/plan`.
- **Use `mode: agent`** — this lets Copilot use tools like reading files, running searches, and accessing your workspace.

---

## Next Steps

Continue to [04-instructions-files.md](04-instructions-files.md) to learn how context-specific rules work.

# 03 — Prompt Files: Slash Commands for Deep Workflows

## What They Are

Prompt files are Markdown files inside `.github/prompts/` that end in `.prompt.md`. GitHub Copilot turns each one into a slash command you can invoke from Copilot Chat.

When you type `/` in Copilot Chat, you will see a dropdown list of your prompt files (and skills). Selecting one sends the full contents of the file to Copilot as the prompt for that conversation.

> **Note:** Agent skills also appear as `/` commands alongside prompt files. The difference is that a skill can include scripts and resources; a prompt file is instructions only.

---

## How to Use Them

1. Open **Copilot Chat** in VS Code (`Cmd+Shift+I`).
2. Type `/` — a list of available prompts appears.
3. Select or type the name of the prompt you want.
4. Add any additional context if needed (e.g., select some code first, then invoke `/code-review`).

---

## The Prompts in This Toolkit

### Planning & Architecture

| Command | What it does |
|---|---|
| `/plan` | Phased implementation plan before writing any code |
| `/arch-review` | Reviews architectural decisions and tradeoffs |
| `/adr` | Generates an Architecture Decision Record |
| `/hex-arch` | Reviews for hexagonal architecture / clean separation |
| `/mle-review` | Reviews machine learning engineering decisions |
| `/api-design` | Reviews REST API endpoint design, naming, status codes |

### Code Quality

| Command | What it does |
|---|---|
| `/code-review` | Reviews code for security, quality, error handling, tests |
| `/pr-review` | Full pull request review |
| `/refactor` | Cleans up messy code without changing behavior |
| `/simplify` | Reduces complexity in a piece of code |
| `/error-audit` | Hunts for silently swallowed errors and missing error handling |
| `/build-fix` | Diagnoses and fixes broken builds systematically |
| `/type-design` | Reviews TypeScript type design |

### Testing

| Command | What it does |
|---|---|
| `/tdd` | Walks through Red → Green → Improve TDD cycle |
| `/e2e` | Creates Playwright E2E tests for a user flow |
| `/verify` | Runs a quality gate check before merging |

### Language-Specific Reviews

| Command | What it does |
|---|---|
| `/ts-review` | TypeScript/JavaScript deep review (types, async, React patterns) |
| `/go-review` | Go code review (interfaces, error handling, concurrency) |
| `/go-build` | Diagnoses Go build and compilation errors |
| `/nestjs-review` | NestJS-specific review (modules, controllers, services) |
| `/angular-review` | Angular-specific review (components, services, change detection) |

### Security

| Command | What it does |
|---|---|
| `/security-review` | Full OWASP Top 10 analysis |
| `/prod-audit` | Production readiness audit before shipping |

### Database

| Command | What it does |
|---|---|
| `/db-review` | Reviews database schema, queries, migrations |

### Git & DevOps

| Command | What it does |
|---|---|
| `/git-pr` | Generates a conventional commit message and PR description |
| `/github-ops` | GitHub Actions workflow review and generation |

### Performance & Accessibility

| Command | What it does |
|---|---|
| `/perf-review` | Performance analysis and optimization suggestions |
| `/a11y-review` | Accessibility review (WCAG, ARIA, keyboard navigation) |

### Documentation

| Command | What it does |
|---|---|
| `/onboard` | Generates onboarding documentation for new developers |
| `/update-docs` | Updates existing documentation to match current code |
| `/seo-review` | Reviews SEO metadata and structure |

### Education

| Command | What it does |
|---|---|
| `/learn` | Explains a concept with examples in your codebase context |
| `/cp` | Competitive programming problem-solving guidance |

---

## Deep Dive: The Most Used Prompts

### `/plan` — Implementation Planner

**Use this before writing any code for a feature.**

```
/plan
Add email verification to the sign-up flow.
Users get a verification email and cannot log in until verified.
We use Next.js + Prisma + PostgreSQL.
```

Produces: goal statement, reuse opportunities, dependencies, phased task breakdown, risks, definition of done.

---

### `/tdd` — Test-Driven Development

**Use this whenever you are writing new code.**

Enforces RED → GREEN → IMPROVE:

1. Write the failing test first
2. Write minimal implementation to make it pass
3. Refactor while keeping tests green

```
/tdd
I need a function that validates an email address and returns a
structured result object.
```

---

### `/code-review` — Code Quality & Security Review

**Use this after writing or modifying code, before committing.**

Reviews across four dimensions:
1. Security (hardcoded secrets, injection risks, missing auth)
2. Code quality (mutation, function length, naming, duplication)
3. Error handling (swallowed errors, missing async error handling)
4. Test coverage (missing tests, always-passing assertions)

Select the code you want reviewed, then type `/code-review`.

---

### `/build-fix` — Build Error Resolution

**Use this when your build or CI is broken.**

Guides you through:
1. Categorizing the error type (type error, import, test failure, lint)
2. Picking the right fix strategy
3. Verifying the fix
4. Checking for related issues from the same root cause

---

## Prompt File Format

Each prompt file is Markdown with an optional YAML frontmatter header:

```yaml
---
description: Reviews code for security vulnerabilities and quality issues
name: code-review
agent: agent
tools: [read_file, search_files]
---

# Code Review

Review the selected code for...
```

| Field | What it does |
|---|---|
| `description` | Short description shown in the UI |
| `name` | The `/name` used to invoke it. Defaults to filename |
| `agent` | Which agent mode to run in: `ask`, `agent`, `plan`, or a custom agent name |
| `model` | Override the language model for this prompt |
| `tools` | Restrict which tools are available when this prompt runs |

---

## Next Steps

Continue to [04-instructions-files.md](04-instructions-files.md) to learn how context-specific rules work.

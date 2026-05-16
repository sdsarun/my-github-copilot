# GitHub Copilot Supercharged — Developer Toolkit

> Everything from the ECC project, adapted for **GitHub Copilot in VS Code**.  
> Copy the `.github/` folder into your repo, read the docs below, and start getting better results today.

---

## What Is This?

This folder contains a set of files that make GitHub Copilot smarter and more consistent when working in your codebase. It is adapted from the [Everything Claude Code (ECC)](https://github.com/everything-claude-code/ecc) project — which was originally built for Claude Code — but all of it works natively with GitHub Copilot in VS Code.

There are **five types of files** and each one does a different job:

| File Type                 | Where It Lives       | What It Does                         | When It Activates                             |
| ------------------------- | -------------------- | ------------------------------------ | --------------------------------------------- |
| `copilot-instructions.md` | `.github/`           | Always-on rules and context          | Every single Copilot interaction              |
| `*.prompt.md`             | `.github/prompts/`   | Slash commands for deep workflows    | When you type `/name` in Copilot Chat         |
| `*.instructions.md`       | Root or any folder   | Context-specific rules per file type | Automatically when editing matching files     |
| `*.agent.md`              | `.github/agents/`    | Specialist agents with focused tools | Pick from agent list or invoke as subagent    |
| `SKILL.md`                | `.github/skills/<n>` | On-demand workflow knowledge         | Type `/skill-name` or auto-loaded when needed |

---

## Quick Setup (5 minutes)

```bash
# 1. Copy the .github folder into your project root
cp -r github-copilot-sd/.github /your-project/.github

# 2. (Optional) Copy .instructions.md files for context-specific rules
cp github-copilot-sd/instructions/*.instructions.md /your-project/

# 3. Open VS Code, open Copilot Chat — rules are now active
```

---

## Folder Structure

```
github-copilot-sd/
├── README.md                              ← You are here
├── QUICK-REFERENCE.md                     ← Daily cheatsheet
│
├── .github/
│   ├── copilot-instructions.md            ← Always-on rules (auto-loaded)
│   └── prompts/
│       │
│       │  — Core Workflows —
│       ├── plan.prompt.md                 ← /plan
│       ├── tdd.prompt.md                  ← /tdd
│       ├── verify.prompt.md               ← /verify
│       ├── prod-audit.prompt.md           ← /prod-audit
│       ├── onboard.prompt.md              ← /onboard
│       ├── code-review.prompt.md          ← /code-review
│       ├── security-review.prompt.md      ← /security-review
│       ├── build-fix.prompt.md            ← /build-fix
│       ├── refactor.prompt.md             ← /refactor
│       ├── simplify.prompt.md             ← /simplify
│       ├── error-audit.prompt.md          ← /error-audit
│       ├── arch-review.prompt.md          ← /arch-review
│       ├── hex-arch.prompt.md             ← /hex-arch
│       ├── perf-review.prompt.md          ← /perf-review
│       ├── api-design.prompt.md           ← /api-design
│       ├── db-review.prompt.md            ← /db-review
│       ├── e2e.prompt.md                  ← /e2e
│       ├── update-docs.prompt.md          ← /update-docs
│       ├── pr-review.prompt.md            ← /pr-review
│       ├── git-pr.prompt.md               ← /git-pr
│       ├── adr.prompt.md                  ← /adr
│       ├── github-ops.prompt.md           ← /github-ops
│       ├── type-design.prompt.md          ← /type-design
│       ├── a11y-review.prompt.md          ← /a11y-review
│       ├── seo-review.prompt.md           ← /seo-review
│       ├── mle-review.prompt.md           ← /mle-review
│       │
│       │  — Language Reviews —
│       ├── ts-review.prompt.md            ← /ts-review
│       ├── py-review.prompt.md            ← /py-review
│       ├── go-review.prompt.md            ← /go-review
│       ├── rust-review.prompt.md          ← /rust-review
│       ├── java-review.prompt.md          ← /java-review
│       ├── kotlin-review.prompt.md        ← /kotlin-review
│       ├── swift-review.prompt.md         ← /swift-review
│       ├── csharp-review.prompt.md        ← /csharp-review
│       ├── cpp-review.prompt.md           ← /cpp-review
│       ├── angular-review.prompt.md       ← /angular-review
│       ├── nestjs-review.prompt.md        ← /nestjs-review
│       ├── laravel-review.prompt.md       ← /laravel-review
│       ├── django-review.prompt.md        ← /django-review
│       ├── fastapi-review.prompt.md       ← /fastapi-review
│       ├── flutter-review.prompt.md       ← /flutter-review
│       └── fsharp-review.prompt.md        ← /fsharp-review
│
│   ├── agents/
│   │   │  — Core Workflow Agents —
│   │   ├── planner.agent.md               ← Plan features and refactors
│   │   ├── architect.agent.md             ← System design and trade-offs
│   │   ├── code-reviewer.agent.md         ← Git-diff-aware code review
│   │   ├── tdd-guide.agent.md             ← Red-Green-Refactor guide
│   │   ├── security-reviewer.agent.md     ← OWASP Top 10 + secrets scan
│   │   ├── build-error-resolver.agent.md  ← Fix build/type errors fast
│   │   ├── refactor-cleaner.agent.md      ← Dead code and cleanup
│   │   ├── performance-optimizer.agent.md ← Profiling and bottlenecks
│   │   ├── e2e-runner.agent.md            ← Playwright E2E tests
│   │   ├── doc-updater.agent.md           ← Keep docs in sync
│   │   │  — Analysis Agents —
│   │   ├── silent-failure-hunter.agent.md ← Find swallowed errors
│   │   ├── type-design-analyzer.agent.md  ← Type design quality
│   │   ├── code-explorer.agent.md         ← Map feature execution paths
│   │   ├── code-simplifier.agent.md       ← Simplify after implementation
│   │   ├── pr-test-analyzer.agent.md      ← PR test coverage quality
│   │   │  — Specialist Agents —
│   │   ├── database-reviewer.agent.md     ← PostgreSQL query and schema
│   │   ├── a11y-architect.agent.md        ← WCAG 2.2 accessibility
│   │   └── mle-reviewer.agent.md          ← ML pipeline production review
│   │
│   ├── skills/
│   │   ├── coding-standards/SKILL.md      ← Cross-project conventions
│   │   ├── tdd-workflow/SKILL.md          ← Full TDD cycle reference
│   │   ├── security-review/SKILL.md       ← Security review playbook
│   │   ├── e2e-testing/SKILL.md           ← Playwright patterns
│   │   ├── api-design/SKILL.md            ← REST API design patterns
│   │   ├── backend-patterns/SKILL.md      ← Node.js/Express patterns
│   │   ├── frontend-patterns/SKILL.md     ← React/Next.js patterns
│   │   ├── verification-loop/SKILL.md     ← Quality gate automation
│   │   ├── bun-runtime/SKILL.md           ← Bun vs Node tradeoffs
│   │   └── documentation-lookup/SKILL.md  ← Fetch live library docs
│   │
│   │  — Build Fixers —
│       ├── go-build.prompt.md             ← /go-build
│       ├── rust-build.prompt.md           ← /rust-build
│       ├── java-build.prompt.md           ← /java-build
│       ├── kotlin-build.prompt.md         ← /kotlin-build
│       └── dart-build.prompt.md           ← /dart-build
│
├── instructions/
│   │  — Always Active —
│   ├── general.instructions.md            ← Coding standards (all files)
│   ├── git.instructions.md                ← Git conventions (all files)
│   ├── error-handling.instructions.md     ← Error handling patterns (all files)
│   │
│   │  — File-Type Specific —
│   ├── testing.instructions.md            ← Unit test rules (*.test.*)
│   ├── e2e.instructions.md                ← E2E test rules (*.spec.*)
│   ├── api.instructions.md                ← API rules (routes/, api/)
│   ├── backend.instructions.md            ← Service/repo layer
│   ├── frontend.instructions.md           ← React rules (components/, *.tsx)
│   ├── security.instructions.md           ← Security rules (auth/, payment/)
│   │
│   │  — Language / Framework —
│   ├── golang.instructions.md             ← Go conventions (*.go)
│   ├── rust.instructions.md               ← Rust conventions (*.rs)
│   ├── python.instructions.md             ← Python conventions (*.py)
│   ├── kotlin.instructions.md             ← Kotlin conventions (*.kt, *.kts)
│   ├── swiftui.instructions.md            ← SwiftUI / Swift (*.swift)
│   ├── angular.instructions.md            ← Angular components/services
│   ├── nestjs.instructions.md             ← NestJS modules/controllers/services
│   ├── nextjs.instructions.md             ← Next.js App Router (app/, pages/)
│   ├── springboot.instructions.md         ← Spring Boot (*.java)
│   ├── dotnet.instructions.md             ← .NET / C# (*.cs)
│   ├── laravel.instructions.md            ← Laravel (*.php)
│   ├── prisma.instructions.md             ← Prisma ORM (schema.prisma)
│   ├── vite.instructions.md               ← Vite (vite.config.*)
│   │
│   │  — Data / Infrastructure —
│   ├── postgres.instructions.md           ← PostgreSQL (*.sql, migrations/)
│   ├── redis.instructions.md              ← Redis (cache/, redis/)
│   ├── docker.instructions.md             ← Docker conventions
│   ├── migrations.instructions.md         ← DB migration rules
│   └── deployment.instructions.md         ← Deploy/k8s/terraform
│
└── docs/
    ├── 01-overview.md                     ← What is this and why
    ├── 02-copilot-instructions.md         ← How the always-on file works
    ├── 03-prompt-files.md                 ← How slash commands work
    ├── 04-instructions-files.md           ← How context rules work
    ├── 05-everyday-workflows.md           ← Day-to-day developer playbook
    └── 06-advanced.md                     ← Advanced patterns
```

---

## Start Here

If you are new, read the docs in order:

1. [docs/01-overview.md](docs/01-overview.md) — What this is and why it helps
2. [docs/02-copilot-instructions.md](docs/02-copilot-instructions.md) — The always-on rules
3. [docs/03-prompt-files.md](docs/03-prompt-files.md) — Slash commands
4. [docs/04-instructions-files.md](docs/04-instructions-files.md) — Context rules
5. [docs/05-everyday-workflows.md](docs/05-everyday-workflows.md) — Real workflows for daily work
6. [QUICK-REFERENCE.md](QUICK-REFERENCE.md) — Pin this, use it every day

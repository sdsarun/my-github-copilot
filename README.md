# GitHub Copilot Supercharged — Developer Toolkit

> A ready-to-use `.github/` folder that makes GitHub Copilot smarter and more consistent in your codebase.  
> Copy it into your project. VS Code auto-discovers everything. No config needed.

---

## What Is This?

GitHub Copilot in VS Code supports five types of customization files. Each one teaches Copilot something different about your project. This toolkit gives you a production-ready set of all five types that you can drop into any project.

---

## The 5 File Types — What Each One Does

### 1. `copilot-instructions.md` — Always-On Rules

**File:** `.github/copilot-instructions.md`  
**Activates:** Every single Copilot chat request, automatically, no trigger needed.

This is the project briefing. Write your coding standards, architecture decisions, security rules, and naming conventions here once — Copilot carries them into every conversation without you repeating yourself.

```
VS Code reads this file → loads it into every chat → your rules always apply
```

**Use for:** Coding standards, tech stack declaration, naming conventions, security requirements, forbidden patterns.

---

### 2. `*.instructions.md` — File-Specific Rules

**Folder:** `.github/instructions/**` (VS Code searches recursively)  
**Activates:** Automatically when the file you are editing matches the `applyTo` glob pattern in the frontmatter.

```yaml
---
applyTo: "**/*.test.ts,**/*.spec.ts"
---
# Testing rules
Always use the AAA structure. Write the test before the implementation...
```

Different rules for different parts of the codebase — testing rules load when you open a test file, API rules load when you open a route handler, security rules load when you open auth code.

**Use for:** Language-specific conventions, framework patterns, rules that only apply to certain folders or file types.

---

### 3. `*.prompt.md` — Slash Commands

**Folder:** `.github/prompts/` (flat — all files at the root)  
**Activates:** When you type `/name` in Copilot Chat — manual invocation only.

These are saved workflows. Each file becomes a `/command` you can invoke to kick off a structured, multi-step task.

```
/plan      → generates a phased implementation plan before you write any code
/tdd       → walks through Red → Green → Improve cycle
/code-review → reviews code across security, quality, error handling, tests
/security-review → full OWASP analysis before shipping
```

**Use for:** Repeatable tasks you run often — scaffolding, code review, PR prep, debugging workflows.

---

### 4. `*.agent.md` — Custom Agents

**Folder:** `.github/agents/` (flat — all files at the root)  
**Activates:** When you select the agent from the dropdown in Copilot Chat, or when another agent invokes it as a subagent.

An agent is a persistent persona with its own instructions, tool restrictions, and model preferences. A planning agent might have read-only tools to prevent accidental changes. A security reviewer agent might be locked to security-specific instructions.

```yaml
---
description: Reviews code for security vulnerabilities
tools: [read_file, search_files] # read-only, no editing
---
```

**Use for:** Specialized roles — security reviewer, architect, planner, TDD guide. Switch to the right agent for the task.

---

### 5. `SKILL.md` — Agent Skills

**Folder:** `.github/skills/<skill-name>/SKILL.md` (one directory per skill)  
**Activates:** Copilot loads it when a request matches the skill's description, or you type `/skill-name` to invoke it manually.

Skills are the most powerful type. Unlike instructions (text only), a skill directory can contain scripts, example files, templates, and other resources alongside the `SKILL.md` instructions. Copilot loads only what it needs — name and description first, full instructions only when relevant, supporting files only when referenced.

Skills are also an **open standard** ([agentskills.io](https://agentskills.io)) — they work in VS Code, Copilot CLI, and the cloud agent.

```
.github/skills/
└── tdd-workflow/
    ├── SKILL.md          ← instructions + description
    └── test-template.ts  ← referenced resource
```

**Use for:** Complex, reusable workflows that benefit from scripts or examples — TDD cycle, E2E testing, security review playbooks, deployment procedures.

---

## How They Differ — Quick Reference

| Type                    | File                      | Loads                    | Scope                | Can include scripts? |
| ----------------------- | ------------------------- | ------------------------ | -------------------- | -------------------- |
| Always-on instructions  | `copilot-instructions.md` | Every request            | Project-wide         | No                   |
| File-based instructions | `*.instructions.md`       | When `applyTo` matches   | Per file type/folder | No                   |
| Prompt / slash command  | `*.prompt.md`             | On `/name` invocation    | One-off task         | No                   |
| Custom agent            | `*.agent.md`              | When you switch agents   | Persistent persona   | No                   |
| Skill                   | `SKILL.md`                | On demand / by relevance | Reusable capability  | **Yes**              |

---

## Setup (2 minutes)

```bash
# Clone this repo
git clone https://github.com/your-org/my-github-copilot

# Copy the whole .github/ folder into your project
cp -r my-github-copilot/.github your-project/

# Open your project in VS Code — everything is auto-discovered, no config needed
```

That is it. Open Copilot Chat, type `/` and you will see all your prompts and skills. Switch agents from the dropdown. Instructions load automatically as you edit files.

---

## Folder Structure

```
.github/
├── copilot-instructions.md              ← Always-on rules — auto-loaded every request
│
├── agents/                              ← Custom agents — flat, one file per agent
│   ├── a11y-architect.agent.md
│   ├── architect.agent.md
│   ├── build-error-resolver.agent.md
│   ├── code-explorer.agent.md
│   ├── code-reviewer.agent.md
│   ├── code-simplifier.agent.md
│   ├── cp-mentor.agent.md
│   ├── database-reviewer.agent.md
│   ├── doc-updater.agent.md
│   ├── e2e-runner.agent.md
│   ├── mle-reviewer.agent.md
│   ├── performance-optimizer.agent.md
│   ├── planner.agent.md
│   ├── pr-test-analyzer.agent.md
│   ├── refactor-cleaner.agent.md
│   ├── security-reviewer.agent.md
│   ├── silent-failure-hunter.agent.md
│   ├── software-mentor.agent.md
│   ├── tdd-guide.agent.md
│   └── type-design-analyzer.agent.md
│
├── instructions/                        ← File-based rules — searched recursively
│   ├── core/
│   │   ├── general.instructions.md      ← applyTo: ** (all files)
│   │   └── error-handling.instructions.md
│   ├── git/
│   │   └── git.instructions.md          ← applyTo: ** (all files)
│   ├── backend/
│   │   └── backend.instructions.md      ← applyTo: **/services/**, **/repositories/**
│   ├── frontend/
│   │   └── frontend.instructions.md     ← applyTo: **/*.tsx, **/components/**
│   ├── architecture/
│   │   └── api.instructions.md          ← applyTo: **/routes/**, **/api/**
│   ├── testing/
│   │   ├── testing.instructions.md      ← applyTo: **/*.test.ts
│   │   └── e2e.instructions.md          ← applyTo: **/e2e/**
│   ├── security/
│   │   └── security.instructions.md     ← applyTo: **/auth/**, **/payment/**
│   ├── database/
│   │   ├── postgres.instructions.md
│   │   ├── prisma.instructions.md
│   │   ├── migrations.instructions.md
│   │   └── redis.instructions.md
│   ├── devops/
│   │   ├── docker.instructions.md       ← applyTo: **/Dockerfile
│   │   └── deployment.instructions.md   ← applyTo: **/k8s/**, **/.github/workflows/**
│   ├── code-quality/
│   │   └── vite.instructions.md         ← applyTo: **/vite.config.*
│   ├── languages/go/
│   │   └── golang.instructions.md       ← applyTo: **/*.go
│   └── frameworks/
│       ├── nestjs/nestjs.instructions.md
│       ├── nextjs/nextjs.instructions.md
│       └── angular/angular.instructions.md
│
├── prompts/                             ← Slash commands — flat, one file per command
│   ├── a11y-review.prompt.md            ← /a11y-review
│   ├── adr.prompt.md                    ← /adr
│   ├── angular-review.prompt.md         ← /angular-review
│   ├── api-design.prompt.md             ← /api-design
│   ├── arch-review.prompt.md            ← /arch-review
│   ├── build-fix.prompt.md              ← /build-fix
│   ├── code-review.prompt.md            ← /code-review
│   ├── cp.prompt.md                     ← /cp
│   ├── db-review.prompt.md              ← /db-review
│   ├── e2e.prompt.md                    ← /e2e
│   ├── error-audit.prompt.md            ← /error-audit
│   ├── git-pr.prompt.md                 ← /git-pr
│   ├── github-ops.prompt.md             ← /github-ops
│   ├── go-build.prompt.md               ← /go-build
│   ├── go-review.prompt.md              ← /go-review
│   ├── hex-arch.prompt.md               ← /hex-arch
│   ├── learn.prompt.md                  ← /learn
│   ├── mle-review.prompt.md             ← /mle-review
│   ├── nestjs-review.prompt.md          ← /nestjs-review
│   ├── onboard.prompt.md                ← /onboard
│   ├── perf-review.prompt.md            ← /perf-review
│   ├── plan.prompt.md                   ← /plan
│   ├── pr-review.prompt.md              ← /pr-review
│   ├── prod-audit.prompt.md             ← /prod-audit
│   ├── refactor.prompt.md               ← /refactor
│   ├── security-review.prompt.md        ← /security-review
│   ├── seo-review.prompt.md             ← /seo-review
│   ├── simplify.prompt.md               ← /simplify
│   ├── tdd.prompt.md                    ← /tdd
│   ├── ts-review.prompt.md              ← /ts-review
│   ├── type-design.prompt.md            ← /type-design
│   ├── update-docs.prompt.md            ← /update-docs
│   └── verify.prompt.md                 ← /verify
│
├── skills/                              ← Agent skills — one directory per skill
│   ├── api-design/SKILL.md
│   ├── backend-patterns/SKILL.md
│   ├── bun-runtime/SKILL.md
│   ├── coding-standards/SKILL.md
│   ├── competitive-programming/SKILL.md
│   ├── documentation-lookup/SKILL.md
│   ├── e2e-testing/SKILL.md
│   ├── engineering-mastery/SKILL.md
│   ├── frontend-patterns/SKILL.md
│   ├── security-review/SKILL.md
│   ├── tdd-workflow/SKILL.md
│   └── verification-loop/SKILL.md
│
└── docs/
    ├── 01-overview.md
    ├── 02-copilot-instructions.md
    ├── 03-prompt-files.md
    ├── 04-instructions-files.md
    ├── 05-everyday-workflows.md
    └── 06-advanced.md
```

---

## Read the Docs

1. [docs/01-overview.md](docs/01-overview.md) — What this is and why it helps
2. [docs/02-copilot-instructions.md](docs/02-copilot-instructions.md) — The always-on rules file
3. [docs/03-prompt-files.md](docs/03-prompt-files.md) — Slash commands
4. [docs/04-instructions-files.md](docs/04-instructions-files.md) — File-based context rules
5. [docs/05-everyday-workflows.md](docs/05-everyday-workflows.md) — Real workflows for daily work
6. [docs/06-advanced.md](docs/06-advanced.md) — Agents, skills, and advanced patterns
7. [QUICK-REFERENCE.md](QUICK-REFERENCE.md) — Pin this, use it every day

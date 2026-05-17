# 06 — Advanced Patterns

Advanced techniques for getting even more out of GitHub Copilot with this toolkit.

---

## Custom Agents — Switching Personas

Custom agents let you switch to a different operating mode — different instructions, different tools, different restrictions — without manually configuring anything.

### How to switch agents

In the Copilot Chat panel, click the agent dropdown and select from the list. All `.agent.md` files in `.github/agents/` appear here automatically.

### Handoffs — chaining agents for a workflow

Agents can hand off to other agents with pre-filled context. For example, the planner agent can produce a plan and show a "Start Implementation" button. Clicking it switches to the implementation agent with the plan already in context.

This enables guided multi-step workflows:

```
planner    → produces implementation plan
           → [Start Implementation button]
code-reviewer → reviews the result
           → [Security Check button]
security-reviewer → final security pass
```

### Agents in this toolkit

| Agent                   | Persona                             | Typical use                              |
| ----------------------- | ----------------------------------- | ---------------------------------------- |
| `planner`               | Research + plan, read-only tools    | Planning any feature before writing code |
| `architect`             | System design and tradeoff analysis | Architecture decisions, ADRs             |
| `code-reviewer`         | Quality + security review           | Reviewing code before a PR               |
| `security-reviewer`     | OWASP-focused, no write tools       | Reviewing auth, payments, sensitive code |
| `tdd-guide`             | Test-first enforcement              | TDD sessions                             |
| `e2e-runner`            | Playwright patterns                 | Writing E2E tests                        |
| `refactor-cleaner`      | Structural cleanup                  | Cleaning up messy code                   |
| `database-reviewer`     | Schema, query, migration review     | Database changes                         |
| `performance-optimizer` | Performance analysis                | Optimization tasks                       |
| `a11y-architect`        | Accessibility review                | UI/component accessibility               |
| `build-error-resolver`  | Build and CI debugging              | Broken builds                            |
| `silent-failure-hunter` | Finding swallowed errors            | Error handling audit                     |
| `type-design-analyzer`  | TypeScript type architecture        | Type system design                       |
| `doc-updater`           | Documentation maintenance           | Keeping docs in sync                     |
| `code-explorer`         | Codebase exploration                | Understanding unfamiliar code            |
| `code-simplifier`       | Complexity reduction                | Simplifying over-engineered code         |
| `mle-reviewer`          | ML engineering review               | ML system design                         |
| `pr-test-analyzer`      | PR test coverage analysis           | Pre-merge test review                    |
| `cp-mentor`             | Competitive programming             | Algorithm problems                       |
| `software-mentor`       | Engineering education               | Learning software engineering concepts   |

---

## Agent Skills — On-Demand Capabilities

Skills are different from instructions. An instruction file is text-only. A skill is a directory that can contain instructions, scripts, templates, and examples.

### How skills load

Copilot loads skills in three stages:

1. **Discovery** — reads `name` and `description` from `SKILL.md` frontmatter for every skill
2. **Instructions loading** — when you invoke `/skill-name` or when Copilot determines the skill is relevant, it loads the full `SKILL.md` body
3. **Resource access** — loads supporting files (scripts, templates, examples) only when the instructions reference them

This means you can have many skills installed without bloating every conversation — only relevant content loads.

### Skills in this toolkit

| Skill                     | What it provides                                        |
| ------------------------- | ------------------------------------------------------- |
| `tdd-workflow`            | Full TDD cycle — Red, Green, Improve — with examples    |
| `e2e-testing`             | Playwright patterns, POM structure, selector priorities |
| `verification-loop`       | Quality gate automation before merging                  |
| `api-design`              | REST API design patterns and review checklist           |
| `backend-patterns`        | Node.js/Express patterns, layered architecture          |
| `bun-runtime`             | Bun vs Node.js tradeoffs and migration guidance         |
| `frontend-patterns`       | React/Next.js patterns, hooks, state management         |
| `coding-standards`        | Cross-project coding conventions                        |
| `security-review`         | Security review playbook and OWASP checklist            |
| `competitive-programming` | Algorithms, data structures, contest strategies         |
| `engineering-mastery`     | Software engineering curriculum                         |
| `documentation-lookup`    | Fetch live library docs for accurate answers            |

### Invoking a skill

Type `/skill-name` in chat to invoke any skill directly:

```
/tdd-workflow   ← loads the full TDD cycle instructions
/api-design     ← loads API design patterns
/security-review ← loads the security review playbook
```

Or describe what you want and Copilot will load the relevant skill automatically.

---

## Combining Context for Precision

The more context Copilot has, the better its answers. Stack context intentionally:

### Pattern 1 — File + Instruction + Prompt

1. Open the file you are working on (instructions auto-load)
2. Select the relevant code
3. Invoke a prompt command

```
[Open src/api/routes/orders.ts]           ← api.instructions.md auto-loads
[Select the POST /orders handler]          ← gives Copilot the code to review
/code-review                               ← applies the review workflow
```

Result: Copilot reviews with full awareness of your API standards, your general coding rules, and the specific code.

### Pattern 2 — @workspace + Prompt

For questions about the whole codebase:

```
@workspace
/plan
Add rate limiting to all public endpoints.
What middleware patterns are already in use in this codebase?
```

Copilot searches the workspace to find existing patterns before making recommendations.

### Pattern 3 — Attach Multiple Files

When a change spans multiple files:

```
[Attach: src/services/orderService.ts]
[Attach: src/db/schema.ts]
[Attach: src/api/routes/orders.ts]
/code-review
Review these three files together — they implement order creation end-to-end.
```

---

## Customizing the Copilot Instructions for Your Stack

The default `copilot-instructions.md` is generic. After setting it up, customize it for your stack:

### For a Next.js + TypeScript project

Add to `.github/copilot-instructions.md`:

```markdown
## Stack

- Next.js 14 (App Router)
- TypeScript strict mode
- Prisma (PostgreSQL)
- Zod for validation
- Vitest + React Testing Library
- Tailwind CSS

## Project-Specific Conventions

- Server Actions live in `src/app/actions/`
- Database queries go through repository functions in `src/db/`
- Never query the database directly in route handlers or components
- Use `src/lib/env.ts` for all environment variable access
- Form validation uses Zod schemas defined in `src/lib/schemas/`

## Forbidden Patterns

- Never use `any` type — fix the type properly
- Never use inline styles — use Tailwind classes
- Never import from `@prisma/client` outside of `src/db/`
- Never call `process.env` directly — use `src/lib/env.ts`
```

### For a Node.js + Express API project

````markdown
## Stack

- Node.js 20 (ESM)
- TypeScript strict mode
- Express 5
- Knex (PostgreSQL)
- Joi for validation
- Jest for tests

## Project-Specific Conventions

- Routes registered in `src/routes/`
- Business logic in `src/services/`
- Database access in `src/repositories/`
- Shared middleware in `src/middleware/`
- Route handlers call services only — never touch the database directly

## Error Handling Pattern

Always use the AppError class for operational errors:

```typescript
throw new AppError("User not found", 404);
```
````

Never throw raw Error objects from service or repository layers.

````

---

## Writing Better Prompt Files

When adding your own workflow prompts, these patterns produce better results:

### Include explicit output format

```markdown
## Output Format

List each issue as:

**[SEVERITY]** — Location
Issue: what is wrong
Fix: exactly what to do
````

Without an output format, Copilot produces different formats every time, making it hard to scan quickly.

### Define scope boundaries

```markdown
## Scope

Only review the selected code. Do not suggest changes to unrelated code.
Flag uncertainty explicitly — do not guess.
```

### Use numbered steps for workflows

```markdown
## Steps

1. First do X
2. Then verify Y
3. Only proceed to Z after Y passes
```

Numbered steps give Copilot a clear execution order.

---

## Creating Framework-Specific Instruction Files

Extend the toolkit with framework-specific rules:

### React component instructions

Create `instructions/react-components.instructions.md`:

```markdown
---
applyTo: "**/components/**,**/*.tsx"
---

# React Component Standards

## Component Structure

1. Types/interfaces
2. Component function
3. Subcomponents (if any)
4. Exports

## Rules

- Prefer functional components — no class components
- Extract repeated JSX into named subcomponents
- Keep component files under 200 lines
- Avoid useEffect for data fetching — use React Query or SWR
- Never store derived state in useState — compute it
```

### Database instructions

Create `instructions/database.instructions.md`:

```markdown
---
applyTo: "**/db/**,**/repositories/**,**/migrations/**"
---

# Database Standards

## Query Rules

- Always use parameterized queries — never string interpolation
- Wrap multiple writes in a transaction
- Never fetch more columns than needed — avoid SELECT \*
- Add indexes for columns used in WHERE clauses on large tables

## Migration Rules

- Migrations are append-only — never edit an existing migration
- Always include a down migration
- Never drop columns in production — mark as deprecated first
```

---

## Sharing This Toolkit Across Multiple Projects

Instead of copying files into each project, create a shared repository and reference it:

### Option 1 — Shared `.github/` folder

Keep one canonical version in a shared repo. When starting a new project:

```bash
git clone git@github.com:your-org/copilot-config .copilot-config
cp -r .copilot-config/.github ./
cp -r .copilot-config/instructions/ ./
```

### Option 2 — Starter template

Create a project template with the toolkit pre-installed. Every new project starts from the template.

### Option 3 — VS Code User Instructions

Copilot also supports user-level instructions stored in VS Code settings. These apply across all projects:

```
Settings → GitHub Copilot → Instructions → User Instructions
```

Use this for personal preferences (your preferred test framework, your naming style) that apply to all your projects regardless of team conventions.

---

## Measuring Whether It Is Working

Signs the toolkit is working well:

- Copilot suggests code that matches your naming conventions without being told
- Code review findings are consistent and actionable
- Test files written with Copilot follow AAA structure by default
- Commit messages from Copilot suggestions follow conventional commit format

Signs it needs tuning:

- Copilot contradicts your rules frequently → rules are too vague or conflicting
- Review findings are always the same → rules are being applied without reading the actual code
- Instructions are ignored → check the file location and `applyTo` patterns

---

## What This Toolkit Uses vs What It Doesn't

VS Code Copilot has more customization options than this toolkit covers. Here is the full picture:

| Feature                         | In this toolkit? | Notes                                                                                   |
| ------------------------------- | ---------------- | --------------------------------------------------------------------------------------- |
| `copilot-instructions.md`       | ✅ Yes           | Always-on project rules                                                                 |
| `*.instructions.md`             | ✅ Yes           | File-based context rules                                                                |
| `*.prompt.md` (slash commands)  | ✅ Yes           | 33 prompt workflows                                                                     |
| `*.agent.md` (custom agents)    | ✅ Yes           | 20 specialized agents                                                                   |
| `SKILL.md` (agent skills)       | ✅ Yes           | 12 reusable skill packages                                                              |
| `AGENTS.md`                     | Not included     | Alternative to `copilot-instructions.md` — use when working with multiple AI agents     |
| Hooks                           | Not included     | Run shell commands at agent lifecycle events — add `.github/hooks/hooks.json` if needed |
| MCP servers                     | Not included     | Connect external tools/databases — add `.github/mcp.json` if needed                     |
| Agent plugins                   | Not included     | Pre-packaged bundles from marketplaces — install via Extensions view                    |
| Organization-level instructions | Not included     | Defined at GitHub org level, outside the repo                                           |
| User-level instructions         | Not included     | Stored in your VS Code profile, apply to all workspaces                                 |

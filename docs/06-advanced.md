# 06 — Advanced Patterns

Advanced techniques for getting even more out of GitHub Copilot with this toolkit.

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
throw new AppError('User not found', 404);
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
applyTo: '**/components/**,**/*.tsx'
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
applyTo: '**/db/**,**/repositories/**,**/migrations/**'
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

## What This Toolkit Does Not Cover

Things that are part of ECC but do not have direct Copilot equivalents:

| ECC Feature              | Copilot Equivalent             | Notes                                    |
| ------------------------ | ------------------------------ | ---------------------------------------- |
| 60 specialized agents    | Copilot Chat with prompt files | Prompt files approximate agent workflows |
| Hooks (pre/post tool)    | Not available                  | Claude Code-specific                     |
| Session persistence      | Not available                  | Copilot does not persist session state   |
| MCP server configs       | VS Code MCP config             | Supported, configured separately         |
| `@workspace` search      | `@workspace`                   | Copilot has this natively                |
| Parallel agent execution | Not available                  | Copilot is single-session                |

If you need agent orchestration, parallel execution, or hook-based automation, Claude Code or a multi-agent system is the right tool for those specific tasks. Copilot excels at inline assistance and structured chat workflows.

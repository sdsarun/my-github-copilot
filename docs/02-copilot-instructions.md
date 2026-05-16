# 02 — copilot-instructions.md: The Always-On Rules

## What It Is

`.github/copilot-instructions.md` is a special file that GitHub Copilot reads automatically on every chat interaction in your repository.

You do not need to attach it, mention it, or remind Copilot about it. It just works.

---

## How It Works

When you open Copilot Chat in a repository that has this file, Copilot loads it into every conversation. The rules inside become the baseline for all answers Copilot gives you.

This is the VS Code / GitHub Copilot equivalent of briefing your team:

> "In this project, we always validate inputs with Zod, we never hardcode secrets, we write tests before code, and we use conventional commits."

---

## What Ours Contains

The file in this toolkit covers five areas:

### 1. Core Workflow

The five steps every developer follows in this project:

1. Research before writing new code
2. Plan before coding anything bigger than one function
3. Write tests first (TDD)
4. Review before committing
5. Use conventional commits

### 2. Coding Standards

- Immutability — always return new objects, never mutate
- File organization — small files, organized by feature
- Error handling — never swallow errors silently
- Input validation — validate at system boundaries
- Naming conventions — consistent across the whole codebase

### 3. Security Checklist

Seven mandatory checks before every commit. These appear in Copilot's answers so it reminds you when they are relevant.

### 4. Testing Requirements

- Minimum 80% coverage
- Three layers: unit, integration, E2E
- TDD cycle enforced

### 5. Git Workflow

Conventional commit format and PR checklist.

---

## How to Customize It

Open `.github/copilot-instructions.md` and edit it for your project. Common customizations:

### Add your stack

```markdown
## Stack

This is a Next.js 14 project using:

- TypeScript (strict mode)
- Prisma for database access
- Zod for validation
- Vitest for unit tests
- Playwright for E2E tests
```

### Add your naming conventions

```markdown
## Naming Conventions (Project-Specific)

- API routes live in `src/app/api/`
- Database models are in `src/db/schema.ts`
- Shared utilities are in `src/lib/`
- Feature components live next to their routes
```

### Add your forbidden patterns

```markdown
## Forbidden Patterns

- Never use `var` — use `const` or `let`
- Never use `any` type — fix the type properly
- Never use `// @ts-ignore` without a comment
- Never use inline styles — use Tailwind classes
```

### Add your review priorities

```markdown
## Review Priorities

When reviewing code in this project, always check:

1. Prisma queries are inside transactions when multiple writes happen
2. Server actions validate with Zod before touching the database
3. Environment variables are accessed through `src/lib/env.ts` only
```

---

## Where to Put the File

The file must live at exactly:

```
your-project/
└── .github/
    └── copilot-instructions.md   ← here
```

If your project does not have a `.github` folder, create it.

---

## Verification

To verify it is working:

1. Open Copilot Chat in your project
2. Ask: "What coding standards do you follow in this project?"
3. Copilot should answer based on your rules, not generic advice

If it does not, check that the file is at `.github/copilot-instructions.md` (not in a subfolder).

---

## Tips

- **Keep it focused** — do not dump your entire documentation in here. Copilot loads this every time, so keep it to the rules you actually want enforced.
- **Be specific** — vague rules like "write good code" have no effect. Specific rules like "never use `any` type — fix the type properly" work.
- **Update it when your conventions change** — this is a living document. Commit changes to it the same way you commit code changes.

---

## Next Steps

Continue to [03-prompt-files.md](03-prompt-files.md) to learn how slash commands work.

# 01 — Overview: What Is This and Why Does It Help?

## The Problem with Default Copilot

When you open a fresh project and ask GitHub Copilot a question, it does not know:

- What coding standards your team follows
- Whether you want tests before or after implementation
- How you name things, structure files, or handle errors
- What security rules you care about
- What kind of commit messages you use

So it gives you generic answers. Sometimes great, sometimes completely wrong for your context.

---

## What This Toolkit Does

This toolkit gives Copilot **persistent context** — information about your project that it carries into every single conversation, without you needing to explain it every time.

It also gives you **reusable workflows** — slash commands that trigger deep, structured processes for common developer tasks like planning a feature, writing tests first, reviewing code, or fixing a broken build.

---

## Where This Came From

This is adapted from [Everything Claude Code (ECC)](https://github.com/everything-claude-code/ecc) — a production plugin originally built for Claude Code. ECC has 60 specialized agents, 230 skills, and 75 slash commands.

Most of those things are Claude Code-specific and do not work in Copilot. But the core ideas — always-on rules, prompt workflows, context-specific instructions — translate directly.

This toolkit takes the ECC concepts that work in Copilot and makes them easy to drop into any project.

---

## The Three Layers

### Layer 1 — Always-On Rules (`.github/copilot-instructions.md`)

This file is automatically loaded by GitHub Copilot on every chat interaction in your repo. You write your rules once and they apply everywhere, forever.

Think of it as the instructions you would give a new developer joining your team — but instead of a human, you are briefing Copilot.

**You get this for free just by having the file.** No extra steps.

### Layer 2 — Slash Commands (`.github/prompts/*.prompt.md`)

These are prompt files that become slash commands in Copilot Chat. Type `/` and you will see a list of commands to choose from.

Each command is a structured workflow for a specific task:

- `/plan` — creates a phased implementation plan before you write any code
- `/tdd` — walks you through test-driven development
- `/code-review` — reviews your code for security, quality, and test coverage
- `/security-review` — deep security analysis before shipping
- `/build-fix` — diagnoses and fixes broken builds
- `/refactor` — cleans up code without changing behavior
- `/api-design` — reviews API endpoints for naming, status codes, structure

### Layer 3 — Context Rules (`.instructions.md` files)

These files let you define rules that only activate for certain file types. For example:

- Security rules that only appear when you are editing auth or payment code
- Testing rules that only appear when you are editing test files
- API rules that only appear when you are editing route handlers

Copilot reads the `applyTo` pattern in the frontmatter and loads the right rules automatically.

---

## How They Work Together

```
You open a test file
→ Copilot loads copilot-instructions.md (always-on)
→ Copilot also loads testing.instructions.md (because the file matches *.test.ts)
→ You type /tdd in chat
→ Copilot follows the TDD workflow prompt with full project context
```

---

## What You Will Get

After setting this up, Copilot will:

- Apply your coding standards without you asking
- Remind you of security checks when working on sensitive code
- Follow TDD when you invoke `/tdd` — write failing test first, then implementation
- Give structured, consistent code reviews instead of vague feedback
- Fix build errors systematically instead of guessing
- Write commit messages in the right format

---

## Next Steps

Continue to [02-copilot-instructions.md](02-copilot-instructions.md) to learn exactly how the always-on file works and how to customize it.

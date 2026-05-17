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

This toolkit gives Copilot **persistent context** — information about your project that it carries into every conversation, without you needing to explain it every time.

It also gives you **reusable workflows** — slash commands and agents that trigger structured, deep processes for common developer tasks like planning, TDD, code review, or debugging.

---

## The Five Types of Customization

GitHub Copilot in VS Code supports five types of files. Each teaches it something different.

---

### Type 1 — Always-On Rules

**File:** `.github/copilot-instructions.md`
**When it loads:** Every single chat request — no trigger, no invocation.

This is your project briefing. Write your coding standards, tech stack, naming conventions, security requirements, and forbidden patterns here. Copilot carries them into every conversation automatically.

```
You open Copilot Chat
→ Copilot loads copilot-instructions.md
→ Your rules are in effect before you type anything
```

**Think of it as:** the onboarding doc you would give a new developer — except this one is loaded every time.

---

### Type 2 — File-Based Rules (`.instructions.md`)

**Folder:** `.github/instructions/` (VS Code searches this folder recursively)
**When it loads:** Automatically, when the file you are editing matches the `applyTo` glob in the frontmatter.

```yaml
---
applyTo: "**/*.test.ts,**/*.spec.ts"
---
Always use AAA structure. Write the test before implementation...
```

Different rules for different parts of the codebase:
- Testing rules activate when you open a test file
- API rules activate when you open a route handler
- Security rules activate when you open auth or payment code

**Think of it as:** an automatic context switch — the right rules appear for the right file without you doing anything.

---

### Type 3 — Slash Commands (`.prompt.md`)

**Folder:** `.github/prompts/` (flat — all files at the root)
**When it loads:** When you type `/name` in Copilot Chat — manual invocation only.

Each prompt file is a saved workflow. Type `/` and you get a list of commands. Select one to run that workflow.

```
/plan          → generates a phased implementation plan before writing code
/tdd           → walks through Red → Green → Improve TDD cycle
/code-review   → reviews code for security, quality, error handling, tests
/build-fix     → diagnoses and fixes broken builds systematically
/security-review → full OWASP analysis before shipping
```

**Think of it as:** keyboard shortcuts for your most common deep-work tasks.

---

### Type 4 — Custom Agents (`.agent.md`)

**Folder:** `.github/agents/` (flat — all files at the root)
**When it loads:** When you select the agent from the dropdown in Copilot Chat.

An agent is a persistent persona with its own instructions, available tools, and model preferences. You switch to an agent when you need its specialized behavior for a task.

- A **planner agent** has read-only tools — it researches and plans without accidentally editing anything
- A **security reviewer agent** focuses specifically on vulnerabilities and has no write tools
- A **TDD guide agent** enforces the test-first cycle

**Think of it as:** switching hats — each agent is optimized for a specific kind of work.

---

### Type 5 — Agent Skills (`SKILL.md`)

**Folder:** `.github/skills/<skill-name>/SKILL.md`
**When it loads:** Copilot detects relevance from the skill description and loads it automatically, or you type `/skill-name` to invoke it directly.

Skills are the most powerful type. A skill is a directory containing a `SKILL.md` plus any supporting scripts, templates, or examples. Copilot loads the skill progressively — description first, full instructions only when relevant, supporting files only when referenced.

Skills are also portable — they work across VS Code, Copilot CLI, and the cloud agent (open standard: agentskills.io).

**Think of it as:** a fully packaged capability that includes everything needed to execute a complex workflow.

---

## How They Work Together

```
You open a test file
→ Copilot loads copilot-instructions.md      (always — project-wide rules)
→ Copilot loads testing.instructions.md      (file match — test-specific rules)
→ You switch to the tdd-guide agent          (specialized persona, test-first tools)
→ You type /tdd in chat                      (invokes the TDD workflow prompt)
→ Copilot loads the tdd-workflow skill       (detailed cycle instructions)
```

Each layer adds more specific context. The rules compound.

---

## What You Will Get

After setting this up, Copilot will:

- Apply your coding standards without you asking — they are always loaded
- Switch to the right rules automatically as you move between files
- Run structured workflows when you invoke `/plan`, `/tdd`, `/code-review`
- Adopt specialized personas (planner, security reviewer, architect) on demand
- Load complex, scripted workflows from skills when the task requires it

---

## Next Steps

Continue to [02-copilot-instructions.md](02-copilot-instructions.md) to learn exactly how the always-on file works and how to customize it for your project.

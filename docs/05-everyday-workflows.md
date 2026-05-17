# 05 — Everyday Developer Workflows

Real scenarios from the daily life of a developer. Each shows exactly what to do, step by step.

---

## Scenario 1 — Starting a New Feature

**Situation:** You have a ticket: "Add order status tracking — users should be able to see the current status of their order and a history of status changes."

### Step 1 — Plan before you code

Open Copilot Chat. Type:

```
/plan
Add order status tracking. Users need to see current status and history.
We are using Next.js + Prisma + PostgreSQL. The orders table already exists.
```

Copilot will produce a phased plan. Save it as a comment in your ticket or in a planning doc. Now you know what to build and in what order.

### Step 2 — Write tests first

Start with Phase 1 (data model). Before touching the schema, open Copilot Chat:

```
/tdd
Write tests for the order status service. It should:
- Return the current status for an order
- Return status history in reverse chronological order
- Throw when the order does not exist
```

Copilot writes the failing tests. Run them — they fail. Good. Now implement the service to make them pass.

### Step 3 — Review before committing

When each phase is done, open the changed files and invoke:

```
/code-review
```

Address anything CRITICAL or HIGH before moving to the next phase.

### Step 4 — Commit with the right format

```
feat: add order status tracking with history

- Add OrderStatus model with status and timestamp fields
- Add getOrderStatus() and getStatusHistory() service methods
- Add /api/orders/:id/status endpoint
- 94% test coverage
```

---

## Scenario 2 — Fixing a Bug

**Situation:** Users report that the search returns no results when they use uppercase letters.

### Step 1 — Write a failing test first

Do not touch the implementation yet. Open the test file for the search function and ask Copilot:

```
/tdd
Add a test that catches this bug: search('APPLE') should return the same
results as search('apple'). The test should fail right now.
```

Copilot writes the test. Run it — it fails. Now you have a regression test that proves the bug.

### Step 2 — Fix the implementation

Ask Copilot to fix the search function so the test passes:

```
The search function in src/lib/search.ts is case-sensitive.
Make it case-insensitive without changing any other behavior.
The test already exists and is failing.
```

Run the tests — all pass.

### Step 3 — Commit

```
fix: make search case-insensitive

Fixes issue where search('APPLE') returned no results.
Added regression test to prevent future regressions.
```

---

## Scenario 3 — Reviewing Code Before a PR

**Situation:** You have finished a feature and want to review your own code before requesting a PR review from a teammate.

### Step 1 — Select changed files

In VS Code, open each changed file. Select all the code you changed (or the entire file).

### Step 2 — Run the code review prompt

```
/code-review
```

Read through every finding. Address CRITICAL and HIGH issues before anything else.

### Step 3 — Run security review if the code is sensitive

If you touched auth, payments, user data, or admin functionality:

```
/security-review
```

### Step 4 — Push only when clean

Do not push if there are unresolved CRITICAL issues. The code review is a gate, not a suggestion.

---

## Scenario 4 — Build is Broken

**Situation:** You pull from main and `npm run build` fails with a wall of TypeScript errors.

### Step 1 — Get the full error

```bash
npm run build 2>&1 | head -50
```

Copy the full error output.

### Step 2 — Invoke the build fix workflow

Paste the error into Copilot Chat:

```
/build-fix

Error:
src/lib/auth.ts:23:5 - error TS2322:
Type 'string | undefined' is not assignable to type 'string'.
```

Copilot will categorize the error and give you a targeted fix strategy.

### Step 3 — Fix, then verify

Apply the fix. Run `npm run build` again. If a new error appears, repeat.

---

## Scenario 5 — Code Has Gotten Messy

**Situation:** You have been adding features to `userService.ts` for three months. It is now 600 lines, has five concerns mixed together, and nobody wants to touch it.

### Step 1 — Run tests first

Make sure tests are passing before you touch anything:

```bash
npm test
```

If tests are failing, fix them first. Refactoring code with broken tests is dangerous.

### Step 2 — Invoke the refactor workflow

Select the entire file (or the messiest section) and type:

```
/refactor
```

Copilot will identify:

- Unused imports and dead code
- Repeated logic that should be extracted
- Functions that are too long
- Naming issues

### Step 3 — Apply changes incrementally

Do not accept everything at once. Make one change at a time, run tests after each, commit when clean.

```
refactor: extract user validation logic into userValidator.ts

Moved 80 lines of validation logic out of userService.ts.
No behavior changes. All tests passing.
```

---

## Scenario 6 — Designing a New API

**Situation:** You are building a new feature and need to design REST endpoints before implementation starts.

### Step 1 — Write draft endpoints

Sketch out what you think the endpoints should look like in a Markdown file or comment:

```
GET /api/v1/subscriptions
GET /api/v1/subscriptions/:id
POST /api/v1/subscriptions
POST /api/v1/subscriptions/:id/cancel
DELETE /api/v1/subscriptions/:id
```

### Step 2 — Review with the API design prompt

Select your draft and type:

```
/api-design
Review these endpoints for naming, method choice, and structure.
```

Copilot will flag issues like wrong status codes, verb-in-URL problems, missing pagination, or auth gaps.

### Step 3 — Implement with tests

Once the design is reviewed, use `/tdd` to write tests for each endpoint before implementing.

---

## Scenario 7 — Onboarding to an Existing Codebase

**Situation:** You just joined a project and need to understand how it works.

### Use `@workspace` to ask questions

Copilot can read your entire workspace when you use `@workspace`:

```
@workspace What does the authentication flow look like?
Walk me through what happens from when a user submits the login form
to when they are logged in.
```

```
@workspace Where are database queries made?
Are they in services, repositories, or in the route handlers directly?
```

```
@workspace What test framework is used and where are the tests located?
```

This is much faster than reading through the code manually.

---

## Scenario 8 — Writing Documentation

**Situation:** You have built a function and need to document it.

### Ask Copilot to generate docs in context

Select the function and ask:

```
Document this function. Explain:
- What it does
- What each parameter is
- What it returns
- What errors it can throw
- One usage example

Follow the existing JSDoc style in this file.
```

Because your `copilot-instructions.md` is loaded, Copilot knows your project's conventions and will write documentation that fits the existing style.

---

## Scenario 9 — Switching to a Specialized Agent

**Situation:** You need focused help for a specific role — planning a feature, doing a security review, or guiding a TDD session — without manually managing which tools and instructions are active.

### Step 1 — Open the agent dropdown

In the Copilot Chat panel, click the agent dropdown (shows the current agent name). You will see all custom agents from `.github/agents/`.

### Step 2 — Switch to the right agent for the task

| Task                   | Switch to                                                       |
| ---------------------- | --------------------------------------------------------------- |
| Planning a feature     | **planner** — read-only tools, focuses on research and planning |
| Deep code review       | **code-reviewer** — systematic quality and security review      |
| Security analysis      | **security-reviewer** — OWASP-focused, no editing tools         |
| TDD session            | **tdd-guide** — enforces test-first, guides the cycle           |
| Architecture decisions | **architect** — high-level design and tradeoff analysis         |
| E2E test writing       | **e2e-runner** — Playwright patterns, POM structure             |

### Step 3 — Work in context

The agent's instructions and tool restrictions are now in effect. When you are done, switch back to the default agent or pick another.

```
Example: Switch to planner → /plan my feature → switch to tdd-guide → /tdd for each component
```

---

## Daily Habit Checklist

Pin this to your desk or set it as a browser tab:

```
Starting a feature?    → switch to planner agent, then /plan
Writing any code?      → switch to tdd-guide, then /tdd
Done writing?          → switch to code-reviewer, then /code-review
Sensitive code?        → switch to security-reviewer
Build broken?          → /build-fix
Code messy?            → /refactor
New API endpoints?     → /api-design
```

---

## Next Steps

Continue to [06-advanced.md](06-advanced.md) for advanced patterns including agents, skills, and combining context for precision.

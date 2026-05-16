---
mode: agent
description: Capture an architectural decision as a structured ADR — records context, alternatives, rationale, and consequences
---

# Architecture Decision Record

Capture this architectural decision as a structured ADR document.

## When to Use

- You just chose between two significant alternatives (framework, library, pattern, database, API approach)
- You want to document _why_ the codebase is shaped a certain way
- Someone asks "why did we choose X?" and the answer should live in the repo

## Step 1 — Gather Information

Answer these questions before writing:

1. What is the decision being made?
2. What context forced this decision (constraints, requirements, events)?
3. What alternatives were considered?
4. Why was each alternative rejected?
5. What are the consequences of the chosen approach?

## Step 2 — Determine the ADR Number

```bash
ls docs/adr/ 2>/dev/null || ls decisions/ 2>/dev/null || echo "No ADR folder found — create docs/adr/"
```

The next ADR number is one higher than the highest existing number. If no folder exists, create `docs/adr/` and start at `0001`.

## Step 3 — Write the ADR

Use this template:

```markdown
# ADR-NNNN: [Decision Title — short noun phrase]

**Date**: YYYY-MM-DD
**Status**: accepted
**Deciders**: [names or roles]

## Context

[2-5 sentences: What is the situation? What forces, constraints, or requirements are in play?
What problem are we trying to solve?]

## Decision

[1-3 sentences stating the decision clearly and directly.]

## Alternatives Considered

### [Alternative 1 Name]

- **Pros**: [benefits]
- **Cons**: [drawbacks]
- **Why not chosen**: [specific reason]

### [Alternative 2 Name]

- **Pros**: [benefits]
- **Cons**: [drawbacks]
- **Why not chosen**: [specific reason]

## Consequences

### Positive

- [What becomes easier]
- [What becomes cheaper]

### Negative

- [What becomes harder]
- [What becomes more constrained]

### Neutral

- [Technical debt accepted]
- [Future decisions this constrains]
```

## Step 4 — Save the File

Save as: `docs/adr/NNNN-kebab-case-title.md`

Examples:

- `docs/adr/0001-use-postgresql-over-mysql.md`
- `docs/adr/0002-adopt-hexagonal-architecture.md`
- `docs/adr/0003-nextjs-for-frontend.md`

## Step 5 — Update the Index (if it exists)

If `docs/adr/README.md` or `docs/adr/index.md` exists, add a row:

```markdown
| ADR-NNNN | [Title] | accepted | YYYY-MM-DD |
```

## Output

Write the complete ADR file content, ready to save.

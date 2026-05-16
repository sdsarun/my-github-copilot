# GitHub Copilot — Project Instructions

These rules are active on every Copilot interaction in this repository.

---

## Core Workflow

1. **Research first** — search for existing implementations before writing anything new.
2. **Plan before coding** — for features larger than a single function, outline phases and dependencies first.
3. **Test-driven** — write the test before the implementation; target 80%+ coverage.
4. **Review before committing** — check for security issues, code quality, and regressions.
5. **Conventional commits** — `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`.

---

## Coding Standards

### Immutability (CRITICAL)

ALWAYS create new objects, NEVER mutate in place:

```
// WRONG — mutates existing state
modify(original, field, value)

// CORRECT — returns a new copy
update(original, field, value)
```

### File Organization

- Prefer many small focused files over large ones — 200–400 lines typical, 800 max.
- Organize by feature/domain, not by type.
- Extract helpers when a file exceeds 200 lines.

### Error Handling

- Handle errors explicitly at every level — never swallow silently.
- Surface user-friendly messages in the UI; log detailed context server-side.
- Fail fast with clear messages at system boundaries (user input, external APIs).

### Input Validation

- Validate all user input before processing.
- Use schema-based validation where available.
- Never trust external data (API responses, file content, query params).

### Naming Conventions

- Variables and functions: `camelCase` — descriptive names
- Booleans: prefer `is`, `has`, `should`, or `can` prefixes
- Types, interfaces, classes: `PascalCase`
- Constants: `UPPER_SNAKE_CASE`
- Custom hooks: `camelCase` with a `use` prefix

---

## Security (mandatory before every commit)

- [ ] No hardcoded secrets, API keys, passwords, or tokens
- [ ] All user inputs validated and sanitized
- [ ] Parameterized queries for all database writes — no string interpolation
- [ ] HTML output sanitized where applicable
- [ ] Auth/authz checked server-side for every sensitive path
- [ ] Rate limiting on all public endpoints
- [ ] Error messages scrubbed of sensitive internals
- [ ] Required env vars validated at startup

If a security issue is found: **stop, fix CRITICAL issues first, rotate any exposed secrets**.

---

## Testing Requirements

Minimum **80% coverage**. All three layers required:

| Layer       | Scope                                       |
| ----------- | ------------------------------------------- |
| Unit        | Individual functions, utilities, components |
| Integration | API endpoints, database operations          |
| E2E         | Critical user flows                         |

**TDD cycle:** Write test (RED) → implement minimally (GREEN) → refactor (IMPROVE) → verify coverage.

Use AAA structure (Arrange / Act / Assert) and descriptive test names.

---

## Git Workflow

```
<type>: <description>

<optional body>
```

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`

---

## Code Quality Checklist

Before marking work complete:

- [ ] Readable, well-named identifiers
- [ ] Functions under 50 lines
- [ ] Files under 800 lines
- [ ] No nesting deeper than 4 levels
- [ ] Comprehensive error handling
- [ ] No hardcoded values — use constants or env config
- [ ] No in-place mutation

---

## Prompt Files Available

Use these in Copilot Chat by typing `/`:

| Prompt             | Purpose                                                |
| ------------------ | ------------------------------------------------------ |
| `/plan`            | Create a phased implementation plan before coding      |
| `/tdd`             | Test-driven development — write the failing test first |
| `/code-review`     | Quality and security review of selected code           |
| `/security-review` | Deep OWASP security analysis                           |
| `/build-fix`       | Systematically diagnose and fix build errors           |
| `/refactor`        | Clean up dead code and reduce duplication              |
| `/api-design`      | Review API endpoint design and naming                  |

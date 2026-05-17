---
applyTo: '**'
---

# Git Conventions

## Commits

Use conventional commit format for every commit:

```
<type>(<scope>): <short description>
```

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`, `ci`

- Subject line ≤ 72 characters, lowercase, no trailing period
- Body explains _why_, not _what_
- Breaking change: add `BREAKING CHANGE:` in the commit footer

## Branching

- Branch from `main` or `develop` — never commit directly to `main`
- Branch naming: `<type>/<short-description>` — e.g., `feat/user-auth`, `fix/null-check`
- One logical change per branch — keep PRs focused

## Pull Requests

- PR title matches the conventional commit format of the primary change
- PR description includes: what, why, how, and a test plan
- All CI checks must pass before requesting review
- No force-pushes to shared branches

## History

- Interactive rebase to clean up work-in-progress commits before opening a PR
- Squash only when individual commits have no meaning — preserve meaningful history
- Never rewrite history on `main` or any shared branch

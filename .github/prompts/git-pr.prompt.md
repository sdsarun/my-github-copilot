---
agent: code-reviewer
description: Git workflow and PR preparation — write commit messages, draft PR descriptions, analyze branch changes, and prepare for review
---

# Git PR Workflow

Prepare this branch for pull request submission.

## Step 1 — Analyze Changes

```bash
git log main..HEAD --oneline          # Commits on this branch
git diff main..HEAD --stat            # Files changed
git diff main..HEAD                   # Full diff
```

## Step 2 — Write Commit Messages (if needed)

If commits have unclear messages, suggest rewrites using conventional commit format:

```
<type>(<scope>): <short description>

<optional body — what and why, not how>
```

**Types**: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`, `ci`

**Rules**:

- Subject line ≤ 72 characters, lowercase, no period
- Body explains _why_, not _what_ — the diff shows what
- Breaking change: add `BREAKING CHANGE:` in footer

**Examples**:

```
feat(auth): add refresh token rotation

fix(api): return 404 when user not found instead of 500

refactor(db): extract query builder to separate module
```

## Step 3 — Draft PR Description

Use this template:

```markdown
## What

[1-3 sentence summary of what this PR does]

## Why

[Context: why this change is needed, what problem it solves]

## How

[Key implementation decisions — only if non-obvious]

## Testing

- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual verification steps:
  1. [Step 1]
  2. [Step 2]

## Screenshots / Output

[If applicable — before/after, terminal output, UI screenshot]

## Checklist

- [ ] No hardcoded secrets
- [ ] Error handling reviewed
- [ ] Breaking changes documented
```

## Step 4 — Pre-Flight Check

Before submitting:

```bash
git diff main..HEAD -- "*.test.*" "*.spec.*"   # Verify tests included
git log main..HEAD --oneline | wc -l            # Commit count
```

- [ ] All tests pass locally
- [ ] Branch is up to date with the base branch
- [ ] No unintended files included (check `git status`)
- [ ] PR is scoped — does one thing, not ten

## Output

Provide:

1. Suggested commit message rewrites (if any)
2. Draft PR title: `<type>(<scope>): <description>`
3. Draft PR description (filled in template)
4. Any pre-flight issues found

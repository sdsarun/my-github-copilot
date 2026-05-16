---
agent: code-explorer
description: GitHub operations — triage issues, manage PRs, debug CI, prepare releases using the gh CLI
---

# GitHub Operations

Manage GitHub repository tasks using the `gh` CLI.

**Requires:** `gh` CLI installed and authenticated (`gh auth login`)

---

## Issue Triage

Classify each open issue by type and priority:

**Types:** `bug`, `feature-request`, `question`, `documentation`, `enhancement`, `duplicate`, `invalid`, `good-first-issue`

**Priority:** `critical` (security/breaking), `high` (significant impact), `medium`, `low`

```bash
# List open issues
gh issue list --limit 30

# View a specific issue
gh issue view <number>

# Search for duplicates
gh issue list --search "keyword" --state all --limit 20

# Apply labels
gh issue edit <number> --add-label "bug,high-priority"

# Post a comment
gh issue comment <number> --body "Thanks for the report. Can you share reproduction steps?"

# Close as duplicate
gh issue close <number> --comment "Duplicate of #<original>"
```

---

## PR Management

```bash
# List open PRs
gh pr list

# Check CI status
gh pr checks <number>

# View PR diff summary
gh pr diff <number> --stat

# Request review
gh pr edit <number> --add-reviewer username

# Merge when ready
gh pr merge <number> --squash --delete-branch

# Check for stale PRs (no activity > 14 days)
gh pr list --json number,title,updatedAt | jq '.[] | select(.updatedAt < (now - 1209600 | todate))'
```

---

## CI/CD Debugging

```bash
# View latest workflow runs
gh run list --limit 10

# View a specific run's logs
gh run view <run-id> --log-failed

# Re-run a failed job
gh run rerun <run-id> --failed

# Check workflow file
cat .github/workflows/ci.yml
```

---

## Release Preparation

```bash
# List recent tags
git tag --sort=-version:refname | head -10

# Create a release
gh release create v1.2.3 \
  --title "Release 1.2.3" \
  --notes "$(git log v1.2.2..HEAD --oneline --no-merges)"

# View existing release
gh release view v1.2.3
```

**Changelog workflow:**

1. `git log <prev-tag>..HEAD --oneline --no-merges` — list commits
2. Group into: Breaking Changes, New Features, Bug Fixes, Chores
3. Draft release notes from grouped commits

---

## Dependabot / Security Alerts

```bash
# List security alerts (requires admin access)
gh api repos/:owner/:repo/vulnerability-alerts

# View Dependabot PRs
gh pr list --label "dependencies"

# Merge safe patch updates
gh pr list --label "dependencies" --json number,title | \
  jq '.[] | select(.title | contains("patch"))'
```

---

## Output

For triage: produce a prioritized issue list with recommended actions.
For CI: identify the failing step and suggest a fix.
For releases: produce draft release notes grouped by type.

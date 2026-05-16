---
mode: agent
description: Pull request review — behavioral coverage analysis, missing tests, logic gaps, and risk assessment
---

# PR Review

Analyze this pull request for correctness, test coverage, and risk.

## Step 1 — Understand the Change

1. Identify what behavior was added, changed, or removed
2. List the files changed and their roles
3. Note any dependencies introduced or updated

## Step 2 — Code Correctness

Check the changed code for:

- Logic errors — off-by-one, incorrect conditions, wrong operator
- Edge cases not handled — empty input, null/nil/None, zero, negative, max value
- Race conditions in concurrent code
- Error paths that silently succeed or swallow errors
- Changed API contracts that callers depend on

## Step 3 — Test Coverage

For each behavior introduced or changed:

1. Find the corresponding test file(s)
2. Assess whether the test exercises the actual logic path
3. Check:
   - Happy path tested?
   - Error / failure path tested?
   - Edge cases tested (empty, null, boundary values)?
   - Integration test if it crosses a module or service boundary?

Rate coverage per changed behavior:

- **Full** — happy path + error path + edge cases
- **Partial** — some paths tested, gaps noted
- **Missing** — no test for this behavior

### Common Coverage Gaps

- New validation logic — tested with invalid input?
- New branch — both true and false paths tested?
- New error message — tested that the right error is returned?
- Changed return value — test asserts on the new value?

## Step 4 — Security Spot Check

- New input from user or external source validated?
- New query parameterized?
- New endpoint behind auth?
- New secret uses environment variable?

## Step 5 — Risk Assessment

Rate overall PR risk:

- **Low** — small change, full tests, no external dependencies touched
- **Medium** — logic change, partial tests, or cross-service interaction
- **High** — broad impact, missing tests, or auth/security change

## Output Format

```
## Changed Behaviors
1. [Description of behavior]
   - Test coverage: Full / Partial / Missing
   - Gap: [If partial/missing, what is not tested]

## Code Issues
**[CRITICAL|HIGH|MEDIUM|LOW]**: [Issue description]

## Security
[Any concerns or "No concerns found"]

## Risk: [Low / Medium / High]
Reason: [One sentence]

## Recommendation
[Approve / Request changes] — [Brief justification]
```

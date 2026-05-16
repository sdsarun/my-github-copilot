---
agent: code-simplifier
description: Simplify overly complex code — deep nesting, long functions, complex conditionals, and unnecessary abstractions
---

# Simplify Code

Analyze the selected code for unnecessary complexity. Suggest or apply simplifications.

## What to Look For

### Deep Nesting

Nesting deeper than 3 levels — replace with early returns or guard clauses:

```js
// Before
function process(user) {
  if (user) {
    if (user.active) {
      if (user.role === 'admin') {
        doWork(user);
      }
    }
  }
}

// After
function process(user) {
  if (!user) return;
  if (!user.active) return;
  if (user.role !== 'admin') return;
  doWork(user);
}
```

### Long Functions

Functions over 50 lines — extract to named helper functions:

- Each extracted function should do one thing
- Name the function for what it does, not how it does it

### Complex Conditionals

- Boolean expression with 3+ conditions — extract to a named function or variable:

  ```js
  // Before: hard to read
  if (user.age >= 18 && user.country !== 'restricted' && !user.banned && account.verified) {
  }

  // After
  const canAccess = user.age >= 18 && user.country !== 'restricted' && !user.banned && account.verified;
  if (canAccess) {
  }
  ```

- Inverted conditions that are easier to read un-negated

### Unnecessary Abstraction

- Abstract class, interface, or helper used by only one caller — inline it
- Wrapper function that only delegates with no transformation
- Parameters passed through 3+ layers untouched — consider if architecture needs revision

### Dead Code

- Commented-out code blocks — remove (version control preserves history)
- Variables declared but never read
- Imports never used
- Functions never called

### Repeated Code

- Same logic block copy-pasted in 2+ places — extract to a shared function

## Output Format

For each issue found:

```
**Issue**: [Short description]
**Location**: [File:Line if known]
**Before**:
[original code snippet]
**After**:
[simplified version]
**Why**: [One sentence explanation]
```

End with:

```
## Summary
- Issues found: N
- Estimated complexity reduction: [Low / Medium / High]
```

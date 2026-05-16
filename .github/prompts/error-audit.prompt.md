---
mode: agent
description: Hunt for silent failures — swallowed errors, bad fallbacks, and missing error propagation
---

# Silent Failure Hunt

Review the selected code with zero tolerance for silent failures. These are bugs waiting to happen — code that appears to work but hides real problems.

## Hunt Targets

### 1. Empty or Useless Catch Blocks

```typescript
// CRITICAL — error completely lost
try {
  await saveUser(data);
} catch (e) {}

// CRITICAL — error converted to silent null
try {
  return await fetchUser(id);
} catch {
  return null; // caller has no idea something went wrong
}
```

### 2. Swallowed Promises

```typescript
// HIGH — fire-and-forget, no error handling
sendEmail(user); // if this throws, nothing happens

// HIGH — async forEach does not await, errors lost
users.forEach(async user => {
  await processUser(user); // errors silently dropped
});
```

### 3. Dangerous Fallbacks That Hide Failures

```typescript
// HIGH — empty array makes caller think "no results" not "request failed"
async function getOrders(userId: string) {
  try {
    return await db.orders.findAll(userId);
  } catch {
    return []; // downstream code can't distinguish "empty" from "broken"
  }
}

// HIGH — false makes caller think "user has no permission" not "check failed"
async function hasPermission(userId: string, action: string) {
  try {
    return await permissionService.check(userId, action);
  } catch {
    return false;
  }
}
```

### 4. Lost Stack Traces

```typescript
// MEDIUM — original error context lost
try {
  await doWork();
} catch (error) {
  throw new Error('Work failed'); // original stack trace gone
}

// CORRECT — wrap without losing original
throw new Error('Work failed', { cause: error });
```

### 5. Missing Error Handling on Network / DB / File Paths

```typescript
// HIGH — no try/catch, no timeout, no error handling
const response = await fetch('/api/data');
const data = await response.json(); // throws if not valid JSON

// HIGH — no transaction rollback
await db.orders.create(order);
await db.inventory.deduct(order.items); // if this fails, order exists but inventory not deducted
```

## Output Format

For each finding:

```
**[CRITICAL|HIGH|MEDIUM]** — [File:Line if known]
Type: [empty catch | swallowed promise | dangerous fallback | lost stack | missing error handling]
Issue: [What is hidden or lost]
Impact: [What goes wrong downstream]
Fix: [Concrete suggestion — what the catch block should actually do]
```

End with count of findings by severity.

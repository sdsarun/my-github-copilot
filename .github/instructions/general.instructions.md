---
applyTo: '**'
---

# General Coding Standards

These rules apply to all files in the project.

## Immutability

ALWAYS create new objects, NEVER mutate in place:

```typescript
// WRONG
user.name = 'Alice';
array.push(item);

// CORRECT
const updatedUser = { ...user, name: 'Alice' };
const updatedArray = [...array, item];
```

## Error Handling

NEVER swallow errors silently:

```typescript
// WRONG
try {
  await doSomething();
} catch (e) {}

// CORRECT
try {
  await doSomething();
} catch (error) {
  logger.error('doSomething failed', { error });
  throw error;
}
```

## Input Validation

Validate at system boundaries — user input, API responses, file content:

```typescript
// WRONG
function createUser(data: any) {
  return db.users.create(data);
}

// CORRECT
function createUser(data: unknown) {
  const validated = CreateUserSchema.parse(data);
  return db.users.create(validated);
}
```

## Naming

- Variables and functions: `camelCase`
- Booleans: `isActive`, `hasPermission`, `canEdit`
- Types, interfaces, classes: `PascalCase`
- Constants: `UPPER_SNAKE_CASE`
- Hooks: `useFeatureName`

## Size Limits

- Functions: 50 lines max
- Files: 800 lines max — extract helpers when approaching this
- Nesting: 4 levels max — use early returns

## No Hardcoded Values

```typescript
// WRONG
setTimeout(fn, 3000);
if (role === 'admin')
  // CORRECT
  const REFRESH_INTERVAL_MS = 3000;
const ADMIN_ROLE = 'admin';
```

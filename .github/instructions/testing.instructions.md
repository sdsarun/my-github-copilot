---
applyTo: '**/*.test.ts,**/*.test.js,**/*.spec.ts,**/*.spec.js,**/__tests__/**'
---

# Testing Standards

These rules apply whenever you are editing test files.

## Test Structure — AAA Pattern

Every test must follow Arrange → Act → Assert:

```typescript
test('returns empty array when no items match filter', () => {
  // Arrange
  const items = [{ id: 1, status: 'active' }];

  // Act
  const result = filterByStatus(items, 'inactive');

  // Assert
  expect(result).toEqual([]);
});
```

## Test Naming

Names must describe behavior, not implementation:

```typescript
// WRONG
test('test1', () => {});
test('should work', () => {});
test('getUserById', () => {});

// CORRECT
test('returns null when user does not exist', () => {});
test('throws error when API key is missing', () => {});
test('falls back to cache when database is unavailable', () => {});
```

## Coverage Requirements

- Minimum 80% overall
- Happy path covered
- Error paths covered
- Edge cases: empty input, nulls, boundary values

## TDD Order

1. Write the test FIRST — it must FAIL
2. Write minimal implementation — test must PASS
3. Refactor — tests must stay GREEN

## Do Not

- Skip tests (`test.skip`, `xit`) without a documented reason
- Comment out assertions
- Use `expect(true).toBe(true)` — this always passes and tests nothing
- Mock everything — prefer integration tests over deep mocking
- Test implementation details — test behavior and outcomes

## Mock Rules

```typescript
// WRONG — mocking private implementation details
jest.mock('./utils/internalHelper');

// CORRECT — mocking at system boundaries
jest.mock('./adapters/emailService');
jest.mock('./repositories/userRepository');
```

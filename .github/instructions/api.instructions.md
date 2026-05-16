---
applyTo: '**/routes/**,**/api/**,**/controllers/**,**/handlers/**'
---

# API Design Standards

These rules apply whenever you are editing route or API handler files.

## URL Naming

```
# CORRECT — nouns, plural, kebab-case
GET  /api/v1/users
GET  /api/v1/users/:id
POST /api/v1/users
GET  /api/v1/team-members

# WRONG
GET  /api/v1/getUsers         ← verb in URL
GET  /api/v1/user             ← singular
GET  /api/v1/teamMembers      ← camelCase in URL
```

## HTTP Status Codes

Always use the semantically correct status code:

| Situation                       | Code |
| ------------------------------- | ---- |
| Success with body (GET)         | 200  |
| Created (POST)                  | 201  |
| Success, no body (DELETE)       | 204  |
| Bad input / validation fail     | 400  |
| Not authenticated               | 401  |
| Authenticated but no permission | 403  |
| Resource not found              | 404  |
| Duplicate / conflict            | 409  |
| Rate limit exceeded             | 429  |
| Unexpected server error         | 500  |

## Validate All Inputs

Every route handler must validate inputs before processing:

```typescript
// WRONG
app.post('/users', async (req, res) => {
  const user = await db.users.create(req.body);
  res.json(user);
});

// CORRECT
app.post('/users', async (req, res) => {
  const result = CreateUserSchema.safeParse(req.body);
  if (!result.success) {
    return res.status(400).json({ error: result.error.flatten() });
  }
  const user = await db.users.create(result.data);
  res.status(201).json(user);
});
```

## Consistent Response Envelope

```typescript
// Success
res.json({ success: true, data: user })

// Error
res.status(400).json({ success: false, error: 'Validation failed', details: [...] })

// List with pagination
res.json({
  success: true,
  data: users,
  meta: { page: 1, totalPages: 5, totalCount: 47 }
})
```

## Security Rules

- [ ] Auth middleware applied to all non-public routes
- [ ] Never trust `req.body` without schema validation
- [ ] Do not expose stack traces or internal paths in error responses
- [ ] Apply rate limiting on all public-facing endpoints

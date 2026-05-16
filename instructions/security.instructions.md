---
applyTo: '**/auth/**,**/payment/**,**/billing/**,**/admin/**,**/middleware/**'
---

# Security Standards

These rules apply whenever you are editing authentication, payment, billing, admin, or middleware files.

## Secrets — Never Hardcode

```typescript
// CRITICAL — NEVER do this
const apiKey = 'sk-proj-abc123';
const dbPassword = 'SuperSecret!';

// CORRECT — always use env vars
const apiKey = process.env.OPENAI_API_KEY;
if (!apiKey) throw new Error('OPENAI_API_KEY is required');
```

## Authentication

- NEVER trust user-supplied IDs or roles from the request body or query string.
- ALWAYS read the user identity from the verified session or token.
- Implement token expiry.

```typescript
// WRONG — user can pass any userId
async function getProfile(req) {
  return db.users.findById(req.body.userId);
}

// CORRECT — read from verified session
async function getProfile(req) {
  return db.users.findById(req.session.userId);
}
```

## Authorization

Check permissions server-side for every sensitive operation:

```typescript
async function deleteOrder(req, res) {
  const order = await db.orders.findById(req.params.id);
  if (!order) return res.status(404).json({ error: 'Not found' });

  // Authorization check
  if (order.userId !== req.session.userId && req.session.role !== 'admin') {
    return res.status(403).json({ error: 'Forbidden' });
  }

  await db.orders.delete(order.id);
  res.status(204).send();
}
```

## Database Queries — No String Interpolation

```typescript
// CRITICAL — SQL injection risk
db.query(`SELECT * FROM users WHERE id = ${req.params.id}`);

// CORRECT — parameterized query
db.query('SELECT * FROM users WHERE id = $1', [req.params.id]);
```

## Error Responses — Scrub Internal Details

```typescript
// WRONG — leaks stack trace and internal paths
res.status(500).json({ error: error.stack });

// CORRECT — user-friendly, no internals
res.status(500).json({ error: 'An unexpected error occurred' });
// Log the detail server-side
logger.error('Request failed', { error, path: req.path });
```

## Checklist Before Committing

- [ ] No hardcoded secrets, tokens, or passwords
- [ ] Env vars validated at startup
- [ ] Auth checked server-side (not client-side)
- [ ] Authz checked for every sensitive operation
- [ ] Parameterized queries only — no string interpolation
- [ ] Error messages scrubbed of internal details
- [ ] Rate limiting on public endpoints

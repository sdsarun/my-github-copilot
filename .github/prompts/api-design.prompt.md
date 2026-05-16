---
mode: agent
description: Review API endpoint design — naming, HTTP methods, status codes, request/response structure
---

# API Design Review

Review the selected API endpoints or route definitions for consistency, correctness, and developer experience.

## URL Structure

```
# Good — nouns, plural, lowercase, kebab-case
GET    /api/v1/users
GET    /api/v1/users/:id
POST   /api/v1/users
PUT    /api/v1/users/:id
PATCH  /api/v1/users/:id
DELETE /api/v1/users/:id

# Sub-resources for relationships
GET    /api/v1/users/:id/orders
POST   /api/v1/users/:id/orders

# Actions that don't map to CRUD — verbs sparingly
POST   /api/v1/orders/:id/cancel
POST   /api/v1/auth/refresh
```

### Naming Checklist

- [ ] Resources are nouns, not verbs (`/users` not `/getUsers`)
- [ ] Plural form used (`/users` not `/user`)
- [ ] kebab-case for multi-word paths (`/team-members` not `/teamMembers`)
- [ ] Query params used for filtering, sorting, pagination — not path segments
- [ ] Nested paths only used for true ownership relationships

## HTTP Methods & Status Codes

### Status Code Checklist

- [ ] `200 OK` — successful GET, PUT, PATCH
- [ ] `201 Created` — successful POST; include `Location` header
- [ ] `204 No Content` — successful DELETE or PUT with no body
- [ ] `400 Bad Request` — validation failure, malformed JSON
- [ ] `401 Unauthorized` — missing or invalid auth
- [ ] `403 Forbidden` — authenticated but not authorized
- [ ] `404 Not Found` — resource does not exist
- [ ] `409 Conflict` — state conflict (duplicate, version mismatch)
- [ ] `422 Unprocessable Entity` — semantic validation failure
- [ ] `429 Too Many Requests` — rate limit exceeded
- [ ] `500 Internal Server Error` — unexpected server failure

## Request & Response Structure

### Request validation

- [ ] All inputs validated before processing (use Zod, Joi, class-validator, etc.)
- [ ] Clear error messages returned for invalid inputs
- [ ] Required vs optional fields documented

### Response envelope

```json
{
  "success": true,
  "data": {},
  "error": null,
  "meta": {
    "page": 1,
    "totalPages": 10,
    "totalCount": 97
  }
}
```

### Pagination

- [ ] Cursor-based pagination preferred for large datasets
- [ ] Page + limit supported for smaller datasets
- [ ] Total count included in meta
- [ ] Default page size set (e.g., 20); max page size enforced

## Security

- [ ] Auth required on all non-public endpoints
- [ ] Rate limiting applied
- [ ] Sensitive fields not included in responses
- [ ] Input sanitized against injection

## Output Format

For each issue found:

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [Endpoint]
Issue: [What is wrong]
Fix: [Concrete suggestion]
```

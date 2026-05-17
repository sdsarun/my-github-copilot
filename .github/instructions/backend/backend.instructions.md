---
applyTo: '**/services/**,**/repositories/**,**/middleware/**,**/server/**,**/lib/**'
---

# Backend Development Standards

These rules apply when editing server-side service, repository, or middleware files.

## Layering — Never Cross These Boundaries

```
Route Handler
  ↓ validates input, calls service, returns response
Service Layer
  ↓ business logic, orchestration, NO direct DB calls
Repository Layer
  ↓ database access ONLY, no business logic
Database
```

```typescript
// WRONG — DB query inside route handler
app.get('/users/:id', async (req, res) => {
  const user = await db.query('SELECT * FROM users WHERE id = $1', [req.params.id]);
  res.json(user);
});

// CORRECT — layered
app.get('/users/:id', async (req, res) => {
  const user = await userService.getById(req.params.id);
  if (!user) return res.status(404).json({ error: 'Not found' });
  res.json(user);
});
```

## Repository Pattern

Abstract all data access behind a consistent interface:

```typescript
interface UserRepository {
  findById(id: string): Promise<User | null>;
  findAll(filters?: UserFilters): Promise<User[]>;
  create(data: CreateUserDto): Promise<User>;
  update(id: string, data: UpdateUserDto): Promise<User>;
  delete(id: string): Promise<void>;
}
```

Business logic depends on the interface, not on the database driver directly. This makes it testable.

## Service Layer Rules

- One service per domain (UserService, OrderService, PaymentService)
- Services call repositories — never call raw database queries
- Services can call other services, but avoid deep call chains (> 3 levels)
- Return domain objects, not raw database rows

## Middleware Pattern

```typescript
// Auth middleware — verify token, attach user to request
export function requireAuth(req: Request, res: Response, next: NextFunction) {
  const token = req.headers.authorization?.replace('Bearer ', '');
  if (!token) return res.status(401).json({ error: 'Unauthorized' });

  try {
    req.user = verifyToken(token); // attach verified user, never trust req.body.userId
    next();
  } catch {
    res.status(401).json({ error: 'Invalid token' });
  }
}
```

## Error Handling in Services

```typescript
// WRONG — swallowed error, caller gets null and has no idea why
async function getUser(id: string): Promise<User | null> {
  try {
    return await userRepo.findById(id);
  } catch {
    return null;
  }
}

// CORRECT — let errors propagate; handle at the route level
async function getUser(id: string): Promise<User | null> {
  return userRepo.findById(id); // throws if DB is down — good
}
```

## Caching Rules

- Cache at the service layer, not the repository layer
- Always set a TTL — never cache indefinitely
- Cache keys must be deterministic and include all variable factors
- Invalidate on write

```typescript
async function getUser(id: string): Promise<User> {
  const cached = await cache.get(`user:${id}`);
  if (cached) return JSON.parse(cached);

  const user = await userRepo.findById(id);
  await cache.set(`user:${id}`, JSON.stringify(user), 'EX', 300); // 5 min TTL
  return user;
}
```

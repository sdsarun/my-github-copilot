---
applyTo: '**/schema.prisma,**/prisma/**,**/*.prisma'
---

# Prisma ORM Conventions

## Schema Design

```prisma
model User {
  id        String    @id @default(cuid())   // cuid() for URL-safe, sortable IDs
  email     String    @unique                // @unique creates an index automatically
  role      Role      @default(USER)
  posts     Post[]
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  deletedAt DateTime?                        // Soft delete

  @@index([createdAt])
  @@index([deletedAt, createdAt])            // Composite for soft-delete + sort queries
}
```

- Add `@@index` on every foreign key and column used in `WHERE` or `ORDER BY`
- `deletedAt DateTime?` — declare upfront when soft delete is foreseeable
- `@unique` already creates an index — no `@@index` needed for unique columns

## Query Patterns

**Select only what you need — never `include` full relations unnecessarily:**

```ts
// Good — only fetches required fields
const user = await prisma.user.findUnique({
  where: { id },
  select: { id: true, email: true, name: true }
});

// Risky on large tables — loads all columns + all posts
const user = await prisma.user.findUnique({
  where: { id },
  include: { posts: true }
});
```

**Cursor pagination — use `cursor` not `skip` for large datasets:**

```ts
const items = await prisma.product.findMany({
  take: 20,
  cursor: lastId ? { id: lastId } : undefined,
  skip: lastId ? 1 : 0,
  orderBy: { id: 'asc' }
});
```

## Critical Traps

- **`updateMany` / `deleteMany` return count, not records** — do not try to access updated rows from the result
- **`@updatedAt` is NOT set by `updateMany`** — only by `update` and `upsert`
- **`$transaction` has a default 5-second timeout** — set `timeout` explicitly for long transactions
- **`migrate dev` resets the database if it detects drift** — never run in production; use `migrate deploy`
- **Serverless: connection exhaustion** — use `@prisma/adapter-neon`, `pgBouncer`, or connection pooling; do not create a new `PrismaClient` per request

## Transactions

```ts
// Use $transaction for atomic multi-step writes
const [user, order] = await prisma.$transaction([prisma.user.create({ data: userData }), prisma.order.create({ data: orderData })]);
```

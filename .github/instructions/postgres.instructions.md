---
applyTo: '**/*.sql,**/migrations/**,**/db/**,**/database/**,**/queries/**'
---

# PostgreSQL Conventions

## Data Types

| Use Case     | Use                                   | Avoid                         |
| ------------ | ------------------------------------- | ----------------------------- |
| Primary keys | `bigint` generated always as identity | `int`, `serial`               |
| Text         | `text`                                | `varchar(255)`                |
| Timestamps   | `timestamptz` (with time zone)        | `timestamp`                   |
| Money/prices | `numeric(10,2)`                       | `float`, `real`               |
| Flags        | `boolean`                             | `varchar`, `int` (0/1)        |
| UUIDs        | `uuid` with `gen_random_uuid()`       | Client-generated UUIDs for PK |

## Indexing

- Every foreign key column needs an index
- Composite index: put equality columns first, range columns last
  - `WHERE status = 'active' AND created_at > $1` → `INDEX ON orders (status, created_at)`
- Use `CREATE INDEX CONCURRENTLY` in production to avoid table locks
- Partial index for common filtered queries:
  - `CREATE INDEX ON users (email) WHERE deleted_at IS NULL`
- Covering index with `INCLUDE` to avoid table heap lookups:
  - `CREATE INDEX ON users (email) INCLUDE (name, created_at)`

## Query Patterns

- Cursor pagination over `OFFSET` for large tables:
  ```sql
  SELECT * FROM products WHERE id > $last_id ORDER BY id LIMIT 20;
  ```
- `UPSERT` with `ON CONFLICT DO UPDATE`
- Prefer `EXISTS` over `COUNT(*)` when just checking presence
- Use `EXPLAIN (ANALYZE, BUFFERS)` to verify query plans before shipping

## Row Level Security (Supabase / multi-tenant)

- Always wrap `auth.uid()` in a subquery for RLS policies:
  ```sql
  CREATE POLICY policy ON orders
    USING ((SELECT auth.uid()) = user_id);
  ```

## Schema Conventions

- Tables: lowercase, plural, snake_case (`order_items`)
- Columns: lowercase, snake_case (`created_at`, `user_id`)
- Always include `created_at TIMESTAMPTZ DEFAULT now()` and `updated_at TIMESTAMPTZ DEFAULT now()`
- Soft delete: add `deleted_at TIMESTAMPTZ` — filter with `WHERE deleted_at IS NULL`
- Add comments on non-obvious columns: `COMMENT ON COLUMN orders.status IS '...'`

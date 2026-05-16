---
mode: agent
description: Database review — SQL queries, schema design, indexes, security, and performance
---

# Database Review

Review the selected SQL, schema, migration, or database access code for performance, security, and correctness.

## Diagnostic Commands (run first if available)

```sql
-- Slow queries
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
ORDER BY mean_exec_time DESC LIMIT 10;

-- Table sizes
SELECT relname, pg_size_pretty(pg_total_relation_size(relid))
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(relid) DESC;

-- Unused indexes
SELECT indexrelname, idx_scan
FROM pg_stat_user_indexes
ORDER BY idx_scan ASC;
```

## Review Priorities

### CRITICAL — Security

- Parameterized queries not used — string interpolation in SQL = injection risk
- Row Level Security (RLS) missing on multi-tenant tables
- `GRANT ALL` to application users — use least privilege
- Public schema accessible to all users

### CRITICAL — Query Performance

- WHERE / JOIN columns without indexes — causes full table scans on large tables
- N+1 query patterns — one query per loop item; should be one batched query
- `SELECT *` — fetch only needed columns
- `EXPLAIN ANALYZE` shows Seq Scan on large tables — add index

### HIGH — Schema Design

- Use correct types: `bigint` for IDs, `text` for strings, `timestamptz` for timestamps, `numeric(precision, scale)` for money, `boolean` for flags
- Foreign keys without `ON DELETE` behavior defined
- Columns missing `NOT NULL` where nulls should not be allowed
- Missing CHECK constraints for constrained values
- `lowercase_snake_case` for all identifiers — no quoted mixed-case names

### HIGH — Index Strategy

- Foreign key columns without indexes — always index FKs
- Composite index column order wrong — equality conditions first, range conditions last
- Partial indexes not used where most queries filter by a specific value
- Indexes on low-cardinality columns (boolean flags) — usually not helpful

### HIGH — Migrations

- Migrations that lock large tables (`ALTER TABLE ADD COLUMN NOT NULL` without default on large tables)
- Missing rollback / down migration
- Editing an existing migration file instead of creating a new one
- Column drops without first deprecating

### MEDIUM — Connection & Transactions

- Multiple writes not wrapped in a transaction
- Long-running transactions that hold locks
- Connection pooling not configured (Supabase: use `?pgbouncer=true`)

## Output Format

For each issue found:

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [Location]
Issue: [What is wrong]
Fix: [Concrete suggestion]
```

End with:

```
## Summary
- Critical: N
- High: N
- Medium: N
- Approved to ship: yes / no
```

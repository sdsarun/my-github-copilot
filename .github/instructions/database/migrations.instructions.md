---
applyTo: '**/migrations/**,**/db/migrations/**,**/database/migrations/**,**/alembic/**'
---

# Database Migration Conventions

## Core Rules

- Migrations are **forward-only** — once merged, never edit a migration file
- Each migration must be reversible with a `down` / `downgrade` function
- One logical schema change per migration file — do not batch unrelated changes

## Safe Deployment Patterns

### Adding Columns

- Add as nullable or with a default value — avoids table lock on live databases
- If NOT NULL and no default: add nullable first → deploy app code → backfill → add NOT NULL constraint

### Removing Columns

Use the **expand-contract** pattern:

1. **Expand**: Add the new column / table (backward-compatible deploy)
2. **Migrate**: Backfill data, update app to use new column
3. **Contract**: Remove the old column in a later migration after all app instances are updated

### Adding Indexes

- Use `CREATE INDEX CONCURRENTLY` (PostgreSQL) to avoid table locks in production
- Never add a blocking index migration on a large table without a maintenance window

### Renaming

- Rename a column by adding the new column, copying data, updating app, then dropping old — never in a single migration on a live system

## Naming

- File names: `YYYYMMDDHHMMSS_short_description` or sequential numbers
- Migration names should describe what changes: `add_email_index_to_users`, `remove_legacy_tokens_table`

## Testing

- Run migrations up and down in CI on a test database
- Check row count and spot-check data after each migration run

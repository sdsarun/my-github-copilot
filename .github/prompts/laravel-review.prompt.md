---
mode: agent
description: Laravel code review — Eloquent N+1, validation, authorization, queues, security misconfigurations
---

# Laravel Code Review

Review the selected Laravel code. Report findings only.

## Before Reviewing

```bash
php artisan serve --no-interaction 2>&1 | head -5   # Check bootstrap
./vendor/bin/phpstan analyse                          # Static analysis (if configured)
php artisan test --stop-on-failure
```

If bootstrap fails, stop and report.

## Review Priorities

### CRITICAL — Security

- SQL injection via raw queries with user input: `DB::select("... $request->input")` — use bindings: `DB::select("... ?", [$value])`
- Missing `$request->validated()` — using `$request->all()` or `$request->input()` bypasses form request validation
- Missing `Policy` authorization — ensure `$this->authorize()` or `Gate::authorize()` is called for every sensitive action
- Mass assignment with `$guarded = []` — use explicit `$fillable` on all models
- Hardcoded secrets in `config/` PHP files — use `.env` and `env()` helper

### CRITICAL — N+1 Queries

- Accessing a relationship inside a loop without eager loading:
  ```php
  // Bad — N+1
  foreach (Order::all() as $order) {
      echo $order->user->name; // One query per order
  }
  // Good
  foreach (Order::with('user')->get() as $order) {
      echo $order->user->name;
  }
  ```
- Use Laravel Debugbar or `DB::enableQueryLog()` to detect in development

### HIGH — Transactions

- Multiple model writes in a request handler without `DB::transaction()` — partial failure leaves inconsistent state
- Events dispatched inside a transaction that fire before commit — use `afterCommit: true` on listeners

### HIGH — Queues and Jobs

- Long-running operations (email, API calls, file processing) done synchronously in a web request
- Job without a `failed()` method — silent failures
- Missing `unique` job for idempotent operations that should not run concurrently

### HIGH — Validation

- No Form Request class for complex input — inline `$request->validate()` is acceptable for simple cases only
- Missing validation rule for file uploads (type, size, MIME)

### MEDIUM — Performance

- `count()` in a loop on an un-cached relationship — eager load or use `withCount()`
- `all()` without pagination on tables with unbounded rows
- `findOrFail` inside a loop — batch with `whereIn`

## Output Format

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line if known]
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

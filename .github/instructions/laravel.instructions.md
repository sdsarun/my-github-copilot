---
applyTo: '**/*.php,**/artisan,**/routes/**,**/app/Http/**,**/app/Models/**'
---

# Laravel Conventions

## Project Structure

Follow the standard Laravel layout with explicit layer separation:

```
app/
├── Actions/         ← Single-purpose use cases (create, update, delete)
├── Http/
│   ├── Controllers/ ← Thin — delegate to Actions or Services
│   ├── Middleware/
│   └── Requests/    ← Form Request classes for validation
├── Models/          ← Eloquent models — relationships, scopes, casts
├── Policies/        ← Authorization logic
└── Services/        ← Complex orchestration across models
```

## Controllers

- Thin controllers: validate → authorize → delegate → respond
- Use Form Request classes (`php artisan make:request`) for all validation
- Return `JsonResource` or resource collections for API responses
- One controller per resource — use resource controllers (`--resource` flag)

## Eloquent Models

- Declare `$fillable` (not `$guarded = []`) on all models
- Use `$casts` for enums, booleans, and JSON columns
- Use query scopes (`scopeActive`, `scopeByUser`) for reusable WHERE clauses
- Use eager loading (`with()`) to prevent N+1:
  ```php
  User::with(['posts', 'profile'])->get(); // Not: User::all() then $user->posts
  ```
- Use `select_related` pattern — only load columns you need

## Transactions

Wrap multi-model writes in a transaction:

```php
DB::transaction(function () use ($data) {
    $order = Order::create($data['order']);
    $order->items()->createMany($data['items']);
    event(new OrderCreated($order));
});
```

## Queues and Jobs

- Move long-running operations to queued jobs
- Implement `ShouldQueue` and `failed()` method for error handling
- Use `delay()` for retry backoff
- Never send emails, push notifications, or external API calls synchronously in a web request

## Security

- Always use `$request->validated()` — not `$request->all()` or `$request->input()`
- Use `Policy` classes for authorization — never inline `Gate::allows` in controllers
- Parameterized queries via Eloquent — no raw string interpolation in `DB::raw()`
- Secrets in `.env` — never in `config/` PHP files committed to git

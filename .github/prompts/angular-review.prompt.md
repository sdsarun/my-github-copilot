---
agent: code-reviewer
description: Angular code review — signals, change detection, routing, DI, reactive forms, and security
---

# Angular Code Review

Review the selected Angular code. Report findings only.

## Before Reviewing

```bash
ng build --configuration production
ng lint
ng test --watch=false
```

If build fails, stop and report.

## Review Priorities

### CRITICAL — Security

- `[innerHTML]` binding on user-controlled content without `DomSanitizer` — XSS risk
- Hardcoded secrets or API keys in component or service code
- Missing route guard on protected routes — `CanActivateFn` or class guard
- HTTP requests to external APIs with credentials without CORS validation

### CRITICAL — Change Detection

- `ChangeDetectionStrategy.Default` on components that only depend on immutable inputs — use `OnPush` to prevent unnecessary re-renders
- Mutating an object reference passed as `@Input()` instead of creating a new reference — `OnPush` will not detect the change
- Subscribing to `Observable` in a component without unsubscribing or using `async` pipe — memory leak

### HIGH — Signals (Angular 17+)

- Mutating signal value directly instead of using `update()` or `set()`
- Missing `computed()` for derived state — recalculating inside template bindings
- `effect()` with side effects that modify other signals — can cause infinite loops

### HIGH — Dependency Injection

- `providedIn: 'any'` on services that should be singletons — use `'root'`
- Service instantiated with `new ServiceName()` bypassing DI — cannot be tested or replaced
- Heavy singleton service doing work in constructor — defer to `init()` or lazy initialization

### HIGH — Routing

- Eagerly loaded feature modules that should be lazy-loaded (`loadChildren`)
- Missing `resolve` for data required before a component renders
- Auth check done in component `ngOnInit` instead of a route guard

### HIGH — Forms

- Reactive form control values used without `.valid` check before submission
- No async validator debounce for server-side validation (causes excessive API calls)
- `valueChanges` subscription without unsubscription — memory leak

### MEDIUM — Performance

- Template expressions with function calls that run on every change detection cycle — use `pipe` or `computed()`
- `trackBy` missing on `*ngFor` / `@for` with dynamic lists

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

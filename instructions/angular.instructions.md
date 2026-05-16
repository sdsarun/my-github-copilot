---
applyTo: '**/*.component.ts,**/*.component.html,**/*.service.ts,**/app.module.ts,**/app.config.ts,**/angular.json'
---

# Angular Conventions

## Signals — Prefer Over RxJS for Local State

Use Angular Signals for component-local reactive state (Angular 17+):

```ts
// Signal-based component state
export class CounterComponent {
  count = signal(0);
  doubled = computed(() => this.count() * 2);

  increment() {
    this.count.update(c => c + 1);
  }
}
```

Use RxJS for: HTTP streams, WebSocket, multi-source merging, complex async pipelines.

## Component Rules

- One component = one file (`.ts` + `.html` + `.scss`)
- `OnPush` change detection on components that only depend on inputs and signals
- Extract child components when `template` exceeds 50 lines
- Use `input()` / `output()` signal APIs (Angular 17+) instead of `@Input()` / `@Output()` for new components
- Avoid `ngDoCheck` — use `computed()` or `effect()` instead

## Dependency Injection

- Prefer `inject()` function in new code over constructor injection
- Provide services at root (`providedIn: 'root'`) unless the service is component-specific
- Use injection tokens for primitive config values

## Routing

- Lazy-load all feature routes: `loadComponent` or `loadChildren`
- Use route guards as functional guards (`CanActivateFn`) not class-based guards
- Resolve data with `ResolveFn` before activating a route

## Forms

- Reactive Forms for complex forms with validation and programmatic control
- Template-driven for simple forms (2-3 fields, no cross-field validation)
- Use `Validators.required` etc. — always validate on the backend too

## Security

- Avoid `[innerHTML]` with user content — use `DomSanitizer.bypassSecurityTrustHtml` only with sanitized input
- Use Angular's built-in HTTP client — it adds CSRF protection for same-origin requests
- Route guards for auth — do not rely only on hiding UI elements

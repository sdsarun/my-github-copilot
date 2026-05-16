---
mode: agent
description: NestJS code review — module boundaries, validation, guards, interceptors, async correctness, and security
---

# NestJS Code Review

Review the selected NestJS code. Report findings only.

## Before Reviewing

```bash
npm run build
npm run lint
npm test
```

If build fails, stop and report.

## Review Priorities

### CRITICAL — Security

- `@Get()` / `@Post()` routes missing `@UseGuards(JwtAuthGuard)` on protected resources
- Hardcoded secrets in service or config files — use `ConfigService` from `@nestjs/config`
- Role check done in controller body instead of a `@UseGuards(RolesGuard)` guard
- User ID read from request body (`dto.userId`) instead of the authenticated JWT payload — client can forge this

### CRITICAL — Validation

- `ValidationPipe` not applied globally or `whitelist: true` not set — unknown properties pass through
- DTO field missing `class-validator` decorator — unvalidated input reaches the service
- Writable DTO fields without length or format constraints on string inputs

### HIGH — Module Boundaries

- `Feature A's` service imported directly into `Feature B's` service — go through a shared module or interface
- Business logic in a `@Controller` method — move to the `@Injectable()` service
- `@Global()` module abused for domain-specific services (only use it for infrastructure: logging, config, DB)

### HIGH — Exception Handling

- Throwing `Error` directly — throw `NotFoundException`, `BadRequestException`, `ForbiddenException`, etc.
- No `@Catch()` filter or `@UseFilters(HttpExceptionFilter)` — stack traces may leak to clients
- Empty `catch` block in a service — swallowed error never surfaces

### HIGH — Async Correctness

- Missing `await` on a `Promise`-returning repository call — silent race condition
- `async` method returning `void` — floating promise, uncaught errors dropped
- Observable (RxJS) returned from a service that should return a `Promise` — inconsistent pattern

### MEDIUM — Performance

- Loading full entity in a list endpoint — use DTO projections (`.select()` in TypeORM, `select` in Prisma)
- No pagination on list endpoints
- Missing `@Throttle()` or rate limiting on public endpoints

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

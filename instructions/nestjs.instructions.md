---
applyTo: '**/*.module.ts,**/*.controller.ts,**/*.service.ts,**/*.guard.ts,**/*.interceptor.ts,**/*.filter.ts,**/*.pipe.ts,**/*.decorator.ts'
---

# NestJS Conventions

## Module Structure

- Feature modules: keep controllers, services, and DTOs inside the feature folder
- `common/` for shared filters, guards, interceptors, pipes, and decorators
- `config/` for validated environment configuration
- Never import a feature module into another feature module's service — use shared modules

## Bootstrap

Always enable global validation pipe with these options:

```ts
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true, // Strip unknown properties
    forbidNonWhitelisted: true, // Throw on unknown properties
    transform: true // Auto-transform to DTO class types
  })
);
```

## DTOs and Validation

- Use `class-validator` decorators on every DTO
- Separate request DTOs from response DTOs — never reuse the same class
- Add `@ApiProperty()` (Swagger) on all DTO fields in public APIs
- Mark sensitive fields with `@Exclude()` on response DTOs + `ClassSerializerInterceptor`

## Dependency Injection

- Constructor injection only — no property injection
- Mark injected deps `private readonly`
- Mock with `jest.mock` at the module level in tests — use `Test.createTestingModule`

## Exception Handling

- Throw `NotFoundException`, `BadRequestException`, `ForbiddenException` — not raw `Error`
- Create a global `HttpExceptionFilter` to log and format errors consistently
- Never leak stack traces or internal details in error responses

## Guards and Auth

- Apply `@UseGuards(JwtAuthGuard)` at the controller or method level — not globally when routes differ
- Extract the current user with a `@CurrentUser()` decorator using `createParamDecorator`
- Never read auth state from the request body

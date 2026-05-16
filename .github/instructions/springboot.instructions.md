---
applyTo: '**/*.java'
---

# Spring Boot Conventions

## Project Structure

Follow strict layer separation:

```
src/main/java/com/example/
├── controller/      ← HTTP only — no business logic
├── service/         ← Business logic — no HTTP/DB imports
├── repository/      ← Data access only — extends JpaRepository
├── domain/          ← Entities, value objects, domain events
├── dto/             ← Request and response DTOs
└── config/          ← Spring configuration classes
```

## Controller Rules

- `@RestController` + `@RequestMapping` — inject service via constructor
- Use `@Valid` on request body parameters
- Return `ResponseEntity<T>` with explicit status codes
- No business logic — delegate entirely to the service layer

## Service Layer

- `@Service` + `@Transactional` on methods that write to the database
- Constructor injection only — no `@Autowired` on fields
- Services depend on repository interfaces, not concrete classes
- Throw domain exceptions, not HTTP exceptions

## Repository Layer

- Extend `JpaRepository<Entity, Id>` — no manual JDBC for standard CRUD
- Use `@Query` with JPQL for complex queries — avoid native SQL unless necessary
- For read-heavy operations: use projections (`interface` with getters) instead of loading full entities

## Validation and Error Handling

- `@ControllerAdvice` with `@ExceptionHandler` for global error responses
- Map domain exceptions to HTTP status codes in the advice class
- Never expose stack traces in error responses

## Configuration

- Use `@ConfigurationProperties` (not `@Value`) for structured config groups
- Validate config with `@Validated` + JSR-303 annotations
- Keep sensitive values in environment variables — not in `application.properties`

## Testing

- `@WebMvcTest` for controller layer (no full Spring context)
- `@DataJpaTest` for repository layer
- `@SpringBootTest` only for true integration tests
- Mock service layer in controller tests with `@MockBean`

---
applyTo: '**/*.kt,**/*.kts'
---

# Kotlin Conventions

## Null Safety

- Use non-nullable types by default — add `?` only when null is a meaningful value
- Safe-call operator `?.` with Elvis `?:` for fallbacks: `user?.email ?: "unknown"`
- `let` for null-check chains: `user?.let { sendWelcome(it) }`
- Never `!!` without a prior guard — it signals a logic error

## Immutability

- Prefer `val` over `var` everywhere possible
- `data class` for value objects — use `copy()` instead of mutating
- Return new collections: `list + item` not `list.add(item)`
- Expose `List<T>` in public APIs, keep `MutableList<T>` private

## Sealed Classes for Results

```kotlin
sealed class Result<out T> {
    data class Success<T>(val value: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
}
```

Use `when` expressions on sealed types — the compiler enforces exhaustiveness.

## Coroutines

- Use `viewModelScope`, `lifecycleScope`, or a DI-provided scope — never `GlobalScope`
- `coroutineScope {}` for parallel work: `async { }.await()` pattern
- `withContext(Dispatchers.IO)` for blocking I/O inside a suspend function
- Catch `CancellationException` only to clean up, then rethrow: `catch (e: CancellationException) { cleanup(); throw e }`
- Expose `StateFlow` / `SharedFlow` from ViewModels — keep `MutableStateFlow` private

## Idiomatic Style

- Scope functions: `let` (null-safe chain), `apply` (object initialization), `run` (compute and return), `also` (side effects), `with` (multiple operations on receiver)
- Replace `if/else` chains with `when` expressions
- Extension functions over utility classes
- Named parameters for functions with 3+ arguments of the same type

## Build (Gradle KTS)

- Use `build.gradle.kts` — avoid Groovy DSL for new projects
- Pin dependency versions in `libs.versions.toml` (Version Catalog)
- Enable `@OptIn` for experimental APIs explicitly — do not suppress broadly

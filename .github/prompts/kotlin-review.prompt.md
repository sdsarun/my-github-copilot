---
mode: agent
description: Kotlin code review — coroutines, Jetpack Compose, clean architecture, and Android/KMP conventions
---

# Kotlin Code Review

Review the selected Kotlin code. Report findings only.

## Before Reviewing

```bash
./gradlew build
./gradlew detekt        # If configured
./gradlew lint          # Android
```

If build fails, stop and report.

## Review Priorities

### CRITICAL — Architecture

- Domain module importing Android / Ktor / Room / framework code — domain must be pure Kotlin
- Data layer types leaking into UI layer — use mappers at the boundary
- Circular dependencies between modules
- Use case / interactor containing UI state logic

### CRITICAL — Coroutines

- `GlobalScope` — use structured concurrency with lifecycle-scoped scopes
- Catching `CancellationException` without re-throwing — breaks coroutine cancellation
- Missing `withContext(Dispatchers.IO)` for blocking I/O operations inside a coroutine
- `MutableStateFlow` exposed directly — expose as `StateFlow`, keep `MutableStateFlow` private
- Flow collection inside `init {}` — use `viewModelScope.launch`

### HIGH — Null Safety

- Non-null assertion `!!` without a prior null check — use `?.` with fallback or `requireNotNull`
- `lateinit var` where `by lazy` or nullable type is safer
- Java platform types not narrowed at interop boundaries

### HIGH — Idiomatic Kotlin

- Java-style code where scope functions (`let`, `apply`, `run`, `also`, `with`) improve clarity
- `if/else` chains that should be `when` expressions
- Mutable `var` where `val` is possible
- Functions with > 5 parameters — use a data class or default parameter grouping

### HIGH — Compose (if applicable)

- Side effects inside composable function bodies outside `LaunchedEffect` / `SideEffect`
- `remember` without `key` when the value depends on a parameter
- Passing non-stable lambdas that capture `ViewModel` state — use stable function references
- Large composables doing multiple things — extract to smaller composables

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

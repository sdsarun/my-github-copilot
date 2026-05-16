---
agent: build-error-resolver
description: Fix Kotlin and Gradle build errors — unresolved references, type mismatches, coroutine context, and Android/KMP issues
---

# Kotlin Build Fix

Analyze and fix the Kotlin build failure shown below.

## Step 1 — Collect Error Output

```bash
./gradlew build --stacktrace 2>&1 | tail -100
./gradlew compileKotlin 2>&1 | tail -80
```

For Android:

```bash
./gradlew assembleDebug 2>&1 | tail -100
```

Paste the full error output with file names and line numbers.

## Step 2 — Categorize Errors

| Error Pattern                                                                    | Meaning                                            |
| -------------------------------------------------------------------------------- | -------------------------------------------------- |
| `Unresolved reference: X`                                                        | Missing import, typo, or missing dependency        |
| `Type mismatch: inferred type is X but Y was expected`                           | Type conversion needed                             |
| `None of the following candidates is applicable`                                 | No matching overload — check argument types        |
| `Suspend function X should be called from coroutine or another suspend function` | Calling suspend function outside coroutine scope   |
| `Expecting member declaration`                                                   | Syntax error — stray character or mismatched brace |
| `Cannot access X: it is private in Y`                                            | Visibility modifier prevents access                |
| `Overload resolution ambiguity`                                                  | Multiple candidate functions — add explicit type   |
| `Smart cast to X is impossible because Y could have been changed`                | Use `val` instead of `var`, or copy to local `val` |

## Step 3 — Fix Strategy

**Unresolved reference**

- Add missing import: `import com.example.ClassName`
- Add missing Gradle dependency:

```kotlin
// build.gradle.kts
dependencies {
    implementation("com.example:library:1.0.0")
}
```

**Suspend function context**

- Move the call inside a `coroutineScope {}`, `viewModelScope.launch {}`, or `runBlocking {}` (tests only)
- Make the calling function `suspend` if it should be part of the coroutine chain

**Type mismatch**

- Use explicit conversion: `.toLong()`, `.toString()`, `.toInt()`
- Check nullable vs non-nullable: add `?` or use `!!` only with a null guard
- Check `Flow<T>` vs `StateFlow<T>` or `LiveData<T>` at ViewModel boundaries

**Android-specific**

- Missing `@AndroidEntryPoint` or `@HiltViewModel` for Hilt injection
- `Context` not passed to component that requires it
- `LifecycleScope` / `ViewModelScope` not used for coroutines in Android classes

**Gradle sync failures**

```bash
./gradlew dependencies           # Show resolved dependency tree
./gradlew --refresh-dependencies # Force refresh
```

## Step 4 — Verify

```bash
./gradlew build
./gradlew test
```

## Output

List each fix applied:

```
File: app/src/main/java/com/example/File.kt
Change: [What was changed and why]
```

Confirm with final build output.

---
agent: code-reviewer
description: Swift code review — protocol-oriented design, value semantics, ARC memory, Swift Concurrency, and idiomatic patterns
---

# Swift Code Review

Review the selected Swift code. Report findings only.

## Before Reviewing

```bash
swift build
swift test
xcodebuild -scheme YourScheme build test
```

If build fails, stop and report.

## Review Priorities

### CRITICAL — Safety

- Force unwrapping `!` on `Optional` without a guard — use `guard let` or `if let`
- Force try `try!` — use `do/catch` or `try?` with explicit fallback
- Force cast `as!` — use conditional cast `as?` and handle nil
- Secrets or tokens in `UserDefaults` — use Keychain
- App Transport Security disabled for production domains
- SQL/command injection — use parameterized queries

### CRITICAL — Memory (ARC)

- Strong reference cycles in closures — use `[weak self]` or `[unowned self]`
- Delegate properties declared as `strong` — use `weak var delegate`
- Escaping closures that capture `self` without capture list

### HIGH — Swift Concurrency

- Data races — mutable state accessed from multiple concurrent contexts without actor isolation
- `@Sendable` violations — non-Sendable types crossing actor boundaries
- Blocking the main actor with synchronous heavy computation — use `Task.detached` or `actor`
- Unstructured `Task {}` without cancellation handling
- Actor reentrancy — state assumptions that break after an `await` suspension point

### HIGH — Architecture

- Business logic directly in `UIViewController` / SwiftUI `View` — move to ViewModel or domain layer
- Protocol with only one conforming type and no testing purpose — use concrete type directly
- Dependencies passed down through 3+ layers — use dependency injection

### HIGH — Idiomatic Swift

- `class` where `struct` would give value semantics
- Objective-C patterns where Swift idioms exist (`NSArray` → `Array`, NSString`→`String`)
- Long `switch` over raw strings or ints — use `enum`
- Missing `guard` for early exit with a meaningful `else` body

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

---
applyTo: '**/*.swift'
---

# SwiftUI and Swift Conventions

## State Management — Choose the Right Wrapper

| Wrapper                        | Use When                                                 |
| ------------------------------ | -------------------------------------------------------- |
| `@State`                       | View-local value (toggle, text field, sheet flag)        |
| `@Binding`                     | Two-way reference to parent's `@State`                   |
| `@Observable` class + `@State` | ViewModel with multiple properties (iOS 17+)             |
| `@Bindable`                    | Two-way binding to a property of an `@Observable`        |
| `@Environment`                 | Shared app-wide dependency injected via `.environment()` |
| `@EnvironmentObject`           | Legacy: prefer `@Observable` + `@Environment` on iOS 17+ |

## ViewModels with @Observable

Use `@Observable` (not `ObservableObject`) — tracks property-level changes, minimizes re-renders:

```swift
@Observable
final class ItemListViewModel {
    private(set) var items: [Item] = []
    private(set) var isLoading = false
    private let repository: any ItemRepository

    func load() async {
        isLoading = true
        defer { isLoading = false }
        items = (try? await repository.fetchAll()) ?? []
    }
}
```

## View Composition

- Keep `body` under 30 lines — extract sub-views as `private` computed properties or `struct`s
- Extract reusable views to separate files
- Use `ViewBuilder` for conditional layout — avoid ternary with unrelated types

## Swift Concurrency

- Use `async/await` — no callbacks or Combine chains for new code
- `@MainActor` on ViewModel — UI state mutations stay on main thread
- Use `Task { }` inside `onAppear` or `.task {}` modifier for async work
- Cancel work with `.task {}` modifier — it auto-cancels when the view disappears
- Avoid `actor` reentrancy bugs: do not assume state is unchanged after an `await`

## Memory Management

- `[weak self]` in all escaping closures that capture `self`
- `weak var delegate` on delegate protocol properties
- Avoid retain cycles: check Instruments Allocations if memory grows unbounded

## Safety

- No force unwrap `!` without a prior `guard let` or `if let`
- No force cast `as!` — use `as?` and handle `nil`
- No force try `try!` — use `do/catch`
- Keychain for secrets and tokens, never `UserDefaults`

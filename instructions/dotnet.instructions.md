---
applyTo: '**/*.cs,**/*.csproj,**/*.sln'
---

# .NET / C# Conventions

## Immutability First

Use `record` types for value objects and DTOs:

```csharp
// Good — immutable, structural equality, concise
public sealed record Money(decimal Amount, string Currency);

public sealed record CreateOrderRequest
{
    public required string CustomerId { get; init; }
    public required IReadOnlyList<OrderItem> Items { get; init; }
}
```

Use `set;` properties only when mutation is explicitly required and justified.

## Dependency Injection

- Constructor injection only — no property injection
- Mark injected fields `private readonly`
- Validate constructor args with `ArgumentNullException.ThrowIfNull`
- Register services in a dedicated extension method, not inline in `Program.cs`

## Async / Await

- Always propagate `CancellationToken` as last parameter on I/O methods
- Never use `async void` except for event handlers
- Never `.Wait()` or `.Result` on a Task inside async context — deadlock risk in older ASP.NET
- Use `await using` for `IAsyncDisposable` resources
- `ConfigureAwait(false)` in library code — not needed in ASP.NET Core app code

## Nullable Reference Types

- Enable nullable in `.csproj`: `<Nullable>enable</Nullable>`
- Return `T?` for values that can genuinely be absent
- Use `!` (null-forgiving) only after a null guard — never speculatively

## Error Handling

- Use `Result<T, TError>` pattern or `OneOf<T, TError>` for expected failures
- Catch specific exception types — never bare `catch (Exception)`
- Always re-throw with context: `throw new InvalidOperationException("context", ex)`
- Global error handler via `IExceptionHandler` (ASP.NET Core 8+) or `UseExceptionHandler`

## Testing

- `xUnit` for tests, `FluentAssertions` for assertions, `Moq` or `NSubstitute` for mocks
- Name tests: `MethodName_StateUnderTest_ExpectedBehavior`
- One assertion per test concept (can use multiple `.Should()` on the same object)

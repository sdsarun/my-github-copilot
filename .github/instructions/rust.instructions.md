---
applyTo: '**/*.rs'
---

# Rust Coding Conventions

## Ownership and Borrowing

- Prefer references (`&T`, `&mut T`) over cloning — clone only when ownership transfer is genuinely needed
- Return owned types from public functions when the cost is acceptable — callers can borrow from owned value
- Use `Cow<str>` when a function sometimes needs to allocate and sometimes does not

## Error Handling

- Use `anyhow` for application-level error handling — `anyhow::Result<T>` as return type
- Use `thiserror` for library errors — derive `Error` with `#[error("...")]`
- Propagate with `?` — do not manually `match` `Ok`/`Err` unless you need to transform the error
- Never `.unwrap()` in production code — use `.expect("reason")` only for invariants that are truly impossible to violate

## Types and Traits

- Use `impl Trait` in function parameters for polymorphism without boxing
- Use `Box<dyn Trait>` only when the concrete type is not known at compile time
- Derive `Debug`, `Clone`, `PartialEq` when applicable — they cost nothing at runtime
- Prefer `Option` and `Result` over sentinel values (negative numbers, empty strings)

## Async

- Use `tokio` as the async runtime — do not mix runtimes
- Do not block inside an async function — use `tokio::task::spawn_blocking` for CPU-bound work
- Use `tokio::join!` to run independent futures concurrently

## Style

- Run `cargo fmt` and `cargo clippy -- -D warnings` before committing
- Group `use` statements: std → external crates → local
- Keep `unsafe` blocks as small as possible and document why they are safe
- Name lifetimes meaningfully: `'conn`, `'req` instead of `'a`, `'b` when scope is large

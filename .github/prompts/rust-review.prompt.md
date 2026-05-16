---
mode: agent
description: Rust code review — ownership, lifetimes, error handling, unsafe usage, and idiomatic patterns
---

# Rust Code Review

Review the selected Rust code. Report findings only — do not rewrite.

## Before Reviewing

Run these if available:

```bash
cargo check
cargo clippy -- -D warnings
cargo test
cargo audit
```

If `cargo check` fails, stop and report before reviewing.

## Review Priorities

### CRITICAL — Safety

- Unchecked `unwrap()` / `expect()` on fallible operations in production code — use `?` or match
- `unsafe` block without a safety comment explaining why it is sound
- SQL/command injection — use parameterized queries or `std::process::Command` with a list of args
- Path traversal — validate user-controlled paths with `canonicalize()` + prefix check

### CRITICAL — Error Handling

- Silenced errors with `let _ = result` — handle explicitly
- Missing error context — use `.context("doing X")` from `anyhow` or `.map_err(...)`
- `panic!` / `unwrap()` for recoverable errors — use `Result`
- `Box<dyn Error>` in library crates — define typed error enums with `thiserror`

### HIGH — Ownership & Memory

- Unnecessary `.clone()` — check if a reference or move is sufficient
- `String` parameter where `&str` would work
- `Vec<T>` parameter where `&[T]` would work
- Over-annotated lifetimes — simplify with lifetime elision rules

### HIGH — Async & Concurrency

- Blocking calls (`.unwrap()` on blocking I/O) inside `async fn` — use `tokio::task::spawn_blocking`
- Unbounded channels in high-throughput paths — use bounded channels
- `Mutex` poisoning not handled after a panic
- Missing `Send + Sync` bounds when types cross thread boundaries

### HIGH — Idiomatic Rust

- Imperative loops where iterators are cleaner (`map`, `filter`, `fold`)
- `match` with only two arms — consider `if let`
- Manual `Option`/`Result` handling where `?` applies
- Missing `derive` macros (`Debug`, `Clone`, `PartialEq`) on data types

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

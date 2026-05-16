---
mode: agent
description: Fix Rust build and compilation errors — borrow checker, lifetime issues, trait implementations, and Cargo problems
---

# Rust Build Fix

Analyze and fix the Rust compilation failure shown below.

## Step 1 — Collect Error Output

Run:

```bash
cargo check 2>&1 | head -100   # Fast check, no linking
cargo build 2>&1               # Full build if check passes
cargo clippy -- -D warnings    # Lints
```

Paste the full output with error codes (e.g., `E0502`).

## Step 2 — Categorize Errors

| Error Code                            | Meaning                                                         |
| ------------------------------------- | --------------------------------------------------------------- |
| `E0382` — use of moved value          | Value used after move — clone, borrow, or restructure           |
| `E0502` — cannot borrow X as mutable  | Conflicting borrows — restructure borrow lifetimes              |
| `E0505` — cannot move X, borrowed     | Value borrowed, then moved — reorder or clone                   |
| `E0597` — X does not live long enough | Lifetime shorter than borrow — return owned or adjust signature |
| `E0277` — trait bound not satisfied   | Implement missing trait or add trait bound to generic           |
| `E0308` — mismatched types            | Type conversion needed or wrong type used                       |
| `E0432` — unresolved import           | Missing `use`, typo, or missing Cargo dependency                |
| `E0433` — failed to resolve           | Module path wrong — check `mod` declarations                    |

## Step 3 — Fix Strategy

**Borrow checker conflicts**

- Use `.clone()` if data is cheap to copy
- Restructure so immutable borrows end before mutable borrow starts
- Use `Rc<RefCell<T>>` or `Arc<Mutex<T>>` for shared ownership
- Return owned values instead of references when the reference outlives the owner

**Lifetime errors**

- Add lifetime annotations: `fn foo<'a>(x: &'a str) -> &'a str`
- Return owned data (`String` instead of `&str`) when lifetimes are unclear

**Missing trait implementation**

- Derive if available: `#[derive(Clone, Debug, PartialEq)]`
- Implement manually for custom logic: `impl Trait for Type { ... }`
- Add `Send + Sync` bounds for async usage

**Missing Cargo dependency**

```bash
cargo add crate_name
```

## Step 4 — Verify

```bash
cargo check
cargo test
```

## Output

List each fix applied:

```
File: src/path/file.rs
Change: [What was changed and why]
```

Confirm with final `cargo check` output.

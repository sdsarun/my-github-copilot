---
agent: code-reviewer
description: C++ code review — memory safety, modern C++ idioms, concurrency, and performance
---

# C++ Code Review

Review the selected C++ code. Report findings only.

## Before Reviewing

```bash
cmake --build .
cppcheck --enable=all src/
clang-tidy src/*.cpp
```

## Review Priorities

### CRITICAL — Memory Safety

- Raw `new` / `delete` — use `std::unique_ptr`, `std::shared_ptr`, or container types
- Buffer overflow — bounds-unchecked array access, `strcpy` / `sprintf` — use `std::array`, `std::string`, `snprintf`
- Use-after-free — accessing memory after `delete` or after container reallocation invalidates iterators
- Uninitialized variables — always initialize, especially POD types
- Memory leaks — resource not freed on every code path — use RAII / smart pointers
- Null pointer dereference — check pointer before use

### CRITICAL — Security

- Command injection — `system()` with user input — use `execve` with argument array
- Format string attacks — `printf(user_input)` — always use a format string literal
- Integer overflow on user-controlled arithmetic — use checked arithmetic or `std::numeric_limits`
- Hardcoded secrets in source
- `reinterpret_cast` bypassing type system safety without clear justification

### CRITICAL — Concurrency

- Data races — shared data accessed from multiple threads without synchronization
- Deadlock — multiple mutexes locked in inconsistent order across code paths
- Missing `std::lock_guard` / `std::unique_lock` — always use RAII wrappers for mutexes
- Detached threads that access local variables after the local scope ends

### HIGH — Modern C++ (C++17/20)

- Raw owning pointers where smart pointers apply
- `NULL` or `0` for null pointer — use `nullptr`
- C-style casts — use `static_cast`, `dynamic_cast`, `const_cast`
- Manual resource management instead of RAII

### HIGH — Performance & Correctness

- Passing large objects by value — pass by `const&`
- Missing `const` on methods that do not modify object state
- Copying when `std::move` would transfer ownership
- Missing `noexcept` on functions guaranteed not to throw

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

---
agent: code-reviewer
description: Python code review — PEP 8, Pythonic idioms, type hints, security, and performance
---

# Python Code Review

Review the selected Python code. Report findings only — do not rewrite or refactor.

## Before Reviewing

Run these if available:

```bash
mypy .                # Type checking
ruff check .          # Fast linting
black --check .       # Format check
bandit -r .           # Security scan
pytest --cov=. --cov-report=term-missing  # Coverage
```

If any of these fail, stop and report before reviewing code.

## Review Priorities

### CRITICAL — Security

- SQL injection via f-strings in queries — use parameterized queries
- Command injection — unvalidated input in shell commands — use `subprocess` with list args
- Path traversal — user-controlled paths — validate with `os.path.normpath`, reject `..`
- `eval` / `exec` with user input — never
- Unsafe deserialization (`pickle`, `yaml.load`) — use `yaml.safe_load`
- Hardcoded secrets — use environment variables
- Weak crypto (MD5/SHA1 for security purposes) — use `hashlib.sha256` or better

### CRITICAL — Error Handling

- `except: pass` or bare `except` — catch specific exceptions
- Silent exception swallowing — always log and handle
- Missing `with` for file/resource management — context managers are mandatory

### HIGH — Type Hints

- Public functions missing type annotations
- `Any` used when specific types are possible
- Missing `Optional` for nullable parameters

### HIGH — Pythonic Patterns

- C-style loops instead of comprehensions
- `type() ==` instead of `isinstance()`
- Magic numbers without named constants
- `"".join()` not used for string concatenation in loops
- **Mutable default arguments**: `def f(x=[])` — use `def f(x=None)` with `x = x or []` in body
- `print()` instead of `logging`
- `value == None` — use `value is None`
- `from module import *` — namespace pollution

### HIGH — Code Quality

- Functions > 50 lines or > 5 parameters — extract or use dataclass
- Nesting deeper than 4 levels — extract helper functions
- Duplicate code patterns
- Shadowing builtins (`list`, `dict`, `str`, `id`, `type`)

### HIGH — Concurrency

- Shared mutable state without locks — use `threading.Lock`
- Mixing sync/async incorrectly
- N+1 queries in loops — batch queries

### MEDIUM — Style

- PEP 8: import order (stdlib → third-party → local), naming, spacing
- Missing docstrings on public functions and classes

## Output Format

For each issue found:

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

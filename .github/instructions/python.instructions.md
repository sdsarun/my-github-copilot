---
applyTo: '**/*.py'
---

# Python Coding Conventions

## General Style

- Follow PEP 8 — use `ruff` or `flake8` to enforce
- Type-annotate all function signatures — use `from __future__ import annotations` for forward refs
- Use `pathlib.Path` instead of `os.path` for file operations
- Prefer f-strings over `.format()` or `%` formatting

## EAFP Over LBYL

- Python style: Easier to Ask Forgiveness than Permission — use `try/except` over checking existence first:

  ```python
  # Pythonic
  try:
      value = d[key]
  except KeyError:
      value = default

  # Less Pythonic
  if key in d:
      value = d[key]
  else:
      value = default
  ```

## Functions and Classes

- Avoid mutable default arguments — use `None` as default and initialize inside function:

  ```python
  # Bug
  def append_item(item, lst=[]):
      lst.append(item)
      return lst

  # Correct
  def append_item(item, lst=None):
      if lst is None:
          lst = []
      lst.append(item)
      return lst
  ```

- Use `@dataclass` or `@attrs.define` for simple data-holding classes
- Prefer `Enum` over magic string constants

## Error Handling

- Catch specific exceptions, never bare `except:`
- Re-raise with context: `raise ValueError("...") from e`
- Log exceptions with `logger.exception("message")` — includes traceback automatically

## Logging

- Configure logging explicitly — never rely on the default `basicConfig` in library code
- Use `logging.getLogger(__name__)` in each module
- Never use `print()` in application code — use the logger

## Testing

- Use `pytest` — fixtures for setup, parametrize for data-driven tests
- Patch at the point of use: `@mock.patch('mymodule.requests.get')`, not where defined

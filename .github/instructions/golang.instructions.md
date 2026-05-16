---
applyTo: '**/*.go'
---

# Go Coding Conventions

## Core Principles

- **Simplicity over cleverness** — write boring, readable code
- **Explicit over implicit** — Go favors clear, direct code
- **Handle errors explicitly** — never ignore a returned error

## Packages and Interfaces

- Accept interfaces, return concrete types:

  ```go
  // Good — parameter is an interface; easy to mock
  func Process(r io.Reader) (*Result, error)

  // Bad — parameter is concrete; harder to test
  func Process(f *os.File) (*Result, error)
  ```

- Define interfaces in the package that _uses_ them, not the package that implements them
- Small interfaces (1-2 methods) are more composable — prefer them

## Error Handling

- Wrap errors with context: `fmt.Errorf("loading config: %w", err)`
- Sentinel errors for values callers compare against: `var ErrNotFound = errors.New("not found")`
- Custom error types when callers need to inspect fields
- Use `errors.Is` and `errors.As` — never string-compare error messages

## Zero Values and Initialization

- Design types so zero value is useful (e.g., `sync.Mutex`, `bytes.Buffer`)
- Use `var x T` when zero value is the desired initial state
- Only use `make` or composite literal when you need a non-zero initial value

## Goroutines and Channels

- Every goroutine must have a clear owner responsible for waiting on it
- Use `context.Context` for cancellation — pass as first parameter
- Always specify channel direction in function signatures: `chan<- T` or `<-chan T`

## Style

- `gofmt` always — non-negotiable
- Variable names: short in small scope (`i`, `n`, `err`), longer with broader scope
- Prefer `errors.New` over `fmt.Errorf` when there is no formatting
- Table-driven tests with `t.Run`

---
agent: build-error-resolver
description: Fix Go build and compilation errors — undefined symbols, import cycles, type mismatches, and module issues
---

# Go Build Fix

Analyze and fix the Go build failure shown below.

## Step 1 — Collect Error Output

Run:

```bash
go build ./...
go vet ./...
```

Paste the full output. If you have not run these yet, do so now before proceeding.

## Step 2 — Categorize Errors

Group errors by type:

| Error Pattern                     | Meaning                                                |
| --------------------------------- | ------------------------------------------------------ |
| `undefined: X`                    | Missing import, typo, or unexported symbol             |
| `cannot use X (type T) as type U` | Type mismatch — check assignment or function signature |
| `import cycle not allowed`        | Circular dependency between packages                   |
| `no required module provides`     | `go.mod` missing dependency — run `go get`             |
| `undeclared name: X`              | Variable used outside its scope                        |
| `too many arguments in call to X` | Function signature changed or wrong function called    |
| `X declared and not used`         | Remove unused variable                                 |

## Step 3 — Fix Strategy

**Undefined symbols**

- Check spelling and exported/unexported case (Go: capitalized = exported)
- Add missing import: `goimports ./...`

**Import cycles**

1. Draw the dependency graph for the cycle
2. Extract shared types/interfaces to a new package (e.g., `types`, `interfaces`)
3. Have both packages depend on the shared package, not each other

**Type mismatches**

- Check concrete type vs interface implementation: does the type satisfy all interface methods?
- Check pointer receiver vs value receiver
- Use explicit conversion where safe: `T(value)`

**Missing module**

```bash
go get example.com/missing/module
go mod tidy
```

## Step 4 — Verify

```bash
go build ./...
go test ./...
```

All must pass. If new errors appear after fixing, repeat the categorization step.

## Output

List each fix applied:

```
File: path/to/file.go
Change: [What was changed and why]
```

Confirm with final build output.

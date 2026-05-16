---
mode: agent
description: Understand a new codebase fast — map the tech stack, entry points, architecture, and conventions in minutes
---

# Codebase Onboarding

Analyze this codebase and produce a structured onboarding guide. Use this when joining a new project, switching repos, or setting up Copilot for the first time in an unfamiliar codebase.

## Step 1 — Reconnaissance

Scan for signals (run these in parallel where possible):

```bash
# Package manifests — identify the tech stack
ls package.json go.mod Cargo.toml pyproject.toml pom.xml build.gradle Gemfile composer.json pubspec.yaml 2>/dev/null

# Top-level structure
find . -maxdepth 2 -type d | grep -v node_modules | grep -v .git | grep -v dist | grep -v .next | sort

# Framework fingerprints
ls next.config.* nuxt.config.* angular.json vite.config.* django settings.py 2>/dev/null

# CI and tooling
ls .github/workflows/ .eslintrc* tsconfig.json Makefile Dockerfile docker-compose* .env.example 2>/dev/null

# Test structure
find . -name "*.test.*" -o -name "*.spec.*" -o -name "*_test.go" | head -20
```

## Step 2 — Architecture Map

From the reconnaissance, identify and document:

**Tech Stack**

- Language(s) and version constraints
- Framework(s) and major libraries
- Database(s) and ORM/query layer
- Build and bundler tooling
- CI/CD platform

**Entry Points**

- Where the application starts (`main.*`, `index.*`, `app.*`, `server.*`)
- How routing is defined
- How environment is configured

**Directory Layout**

- What lives in each top-level folder
- Where business logic is
- Where data access is
- Where tests are

**Conventions Observed**

- Naming patterns (camelCase, snake_case, kebab-case)
- Module structure (feature-based, layer-based)
- Import style (absolute paths, barrel files, relative)
- Error handling pattern

## Step 3 — How to Run It

Find and document:

```bash
# Look for run commands
cat package.json | grep -A 20 '"scripts"'  # Node
cat Makefile                                 # Make
cat README.md | head -100                    # README
```

Report:

- How to install dependencies
- How to start the dev server
- How to run tests
- How to run a build

## Step 4 — Key Files to Know

Identify the 5-10 most important files for understanding how the app works. For each:

```
File: [path]
Role: [One sentence — what does this file do?]
Why Important: [Why should a new dev know this file?]
```

## Output Format

Produce a structured report:

````markdown
# Codebase Overview: [Project Name]

## Stack

[Bullet list of language, framework, database, tools]

## Architecture

[2-3 sentences describing how the pieces fit together]

## Directory Guide

| Folder | Purpose |
| ------ | ------- |
| ...    | ...     |

## How to Run

```bash
# Install
[command]

# Dev
[command]

# Test
[command]
```
````

## Key Files

1. [path] — [role]
2. ...

## Conventions

- [Naming]
- [Module structure]
- [Error handling]

## First Things to Read

[Ordered list of 3-5 files/areas to explore first to understand the business logic]

```

```

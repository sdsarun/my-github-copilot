---
agent: architect
description: Architecture review — system design, component responsibilities, scalability, and trade-offs
---

# Architecture Review

Review the described system or selected code for architecture quality, scalability, and maintainability.

## Review Process

### 1. Understand Current State

Before recommending changes:

- What does the current structure look like?
- What patterns are already in use? (repository, service layer, middleware, etc.)
- What is the main scalability or maintainability pain point?
- What are the integration points with external systems?

### 2. Evaluate Against Principles

#### Separation of Concerns

- [ ] Each layer has a single, clear responsibility
- [ ] Route handlers do not contain business logic
- [ ] Business logic does not contain database queries
- [ ] Data models are not mixed with presentation

```
Good layering:
Route Handler → Service → Repository → Database
     ↓
  Validates input, calls service, returns response
           ↓
     Business rules, orchestration
                   ↓
          Data access only
```

#### Modularity

- [ ] High cohesion — related things are grouped together
- [ ] Low coupling — modules can change without breaking others
- [ ] Clear interfaces between modules
- [ ] Components are independently testable

#### Scalability

- [ ] Stateless design where possible — no in-memory session state
- [ ] Horizontal scaling: can multiple instances run in parallel?
- [ ] Database bottlenecks identified (connection limits, N+1, missing indexes)
- [ ] Caching strategy exists for expensive repeated operations
- [ ] Background jobs for long-running operations (not blocking request/response)

#### Maintainability

- [ ] Consistent patterns throughout — no random variation in approach
- [ ] New developers can understand the flow without a guide
- [ ] Easy to add a new feature without modifying many existing files

### 3. Trade-Off Analysis

For each significant design decision, document:

| Aspect         | Option A | Option B |
| -------------- | -------- | -------- |
| Pros           | ...      | ...      |
| Cons           | ...      | ...      |
| When to choose | ...      | ...      |

### 4. Identify Risks

- What could cause a production incident as the system grows?
- What technical debt will compound?
- What external dependencies are single points of failure?

## Common Architecture Problems to Flag

- **Fat controllers / route handlers** — business logic bleeding into request handlers
- **God services** — one service that does everything; split by domain
- **Anemic models** — data classes with no behavior; behavior scattered in services
- **Deep call chains** — A calls B calls C calls D calls E — hard to test, hard to trace
- **Circular dependencies** — module A imports module B which imports A
- **Implicit shared state** — side effects through module-level variables
- **Missing abstraction at integration points** — direct third-party SDK calls everywhere; wrap in an adapter

## Output Format

```
## Current Architecture
[Brief description of what exists]

## Findings

**[CRITICAL|HIGH|MEDIUM|LOW]**
Issue: [What architectural problem exists]
Impact: [Scaling, maintenance, testing risk]
Recommendation: [What to change and why]

## Recommended Structure
[Only if structural change is recommended]

## Trade-offs
[Any non-obvious trade-offs in the recommendations]
```

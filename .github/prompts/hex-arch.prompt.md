---
agent: architect
description: Design or review code using Hexagonal Architecture (Ports and Adapters) — domain independence, testability, and clean boundaries
---

# Hexagonal Architecture

Apply or review Ports and Adapters (Hexagonal) architecture. Business logic stays independent of frameworks, transport, and persistence details.

## Core Concepts

```
         [ Inbound Adapters ]          [ Outbound Adapters ]
  HTTP Controller ──────────┐   ┌────── DB Repository (Postgres)
  CLI Command ──────────────┤   ├────── Email Gateway (SendGrid)
  Queue Consumer ───────────┤   ├────── Payment Gateway (Stripe)
                            ▼   ▼
                    ┌─────────────────┐
                    │  Application    │  ← Use cases / orchestration
                    │                 │
                    │  Domain Model   │  ← Entities, value objects, rules
                    └─────────────────┘
                     (depends on nothing external)
```

**Dependency direction always flows inward:**

- Adapters → Application → Domain
- Domain depends on nothing outside itself

---

## Step 1 — Identify Domain Boundaries

For the feature or system being designed:

1. What is the core business operation (the use case)?
2. What data does it need from external sources? → Outbound ports
3. What side effects does it produce? → Outbound ports
4. How is it invoked? (HTTP, CLI, queue) → Inbound adapters

---

## Step 2 — Define Ports (Interfaces)

**Outbound port — what the application needs:**

```ts
// TypeScript
export interface UserRepositoryPort {
  findById(id: string): Promise<User | null>;
  save(user: User): Promise<void>;
}

export interface EmailGatewayPort {
  sendVerification(email: string, token: string): Promise<void>;
}
```

```java
// Java
public interface UserRepository {
    Optional<User> findById(UserId id);
    void save(User user);
}
```

**Rule:** Port interfaces live in the application layer. They model capabilities, not technologies.

---

## Step 3 — Implement the Use Case

Use case receives ports via constructor injection. Contains zero framework code:

```ts
export class RegisterUserUseCase {
  constructor(
    private readonly users: UserRepositoryPort,
    private readonly email: EmailGatewayPort,
    private readonly tokens: TokenGeneratorPort
  ) {}

  async execute(command: RegisterUserCommand): Promise<UserId> {
    const existing = await this.users.findByEmail(command.email);
    if (existing) throw new EmailAlreadyInUseError(command.email);

    const user = User.register(command.email, command.hashedPassword);
    const token = this.tokens.generate();

    await this.users.save(user);
    await this.email.sendVerification(command.email, token);

    return user.id;
  }
}
```

---

## Step 4 — Build Adapters at the Edge

```ts
// Inbound adapter — HTTP Controller (framework code lives here)
@Post('/users')
async register(@Body() dto: RegisterDto): Promise<{ id: string }> {
  const id = await this.registerUser.execute({
    email: dto.email,
    hashedPassword: await bcrypt.hash(dto.password, 12),
  });
  return { id: id.value };
}

// Outbound adapter — Postgres repository
export class PostgresUserRepository implements UserRepositoryPort {
  constructor(private readonly db: Database) {}

  async findById(id: string): Promise<User | null> {
    const row = await this.db.query('SELECT * FROM users WHERE id = $1', [id]);
    return row ? User.fromRow(row) : null;
  }
}
```

---

## Review Checklist

Apply this when reviewing existing code for hexagonal compliance:

- [ ] Domain layer imports no framework, ORM, or I/O library
- [ ] Use cases depend on port interfaces, not concrete implementations
- [ ] Adapters depend on the application layer, not vice versa
- [ ] Business rules are not duplicated in adapters
- [ ] Use cases can be tested without a real database or HTTP server
- [ ] Single composition root where all wiring happens

---

## Output

For new features: produce the port interfaces, use case class, and one inbound/outbound adapter skeleton.

For review: identify violations of the above checklist and suggest refactoring steps.

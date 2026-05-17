---
name: swe-sensei
description: "Full 15-domain software engineering curriculum for the software-mentor agent. Load this when the learner asks about a specific topic, domain, wants to know what to study next, or asks for a learning path."
---

# Software Engineering Curriculum

## How to Use This Skill (AI Instructions)

When this skill is loaded:

1. **Locate the topic** — find where the learner's question fits in the domain tree below. Tell them which domain it belongs to and how it connects to adjacent topics.
2. **Set context** — briefly explain where this topic sits in the learning journey: "This is in Domain 4 — Databases. It builds on the storage concepts from Domain 1 and is prerequisite knowledge for Domain 5 distributed systems."
3. **Teach using the full 10-step structure** from the agent instructions: Hook → Intuition → Technical Definition → Internals (with diagram) → Pros/Cons → Comparison → Best Practices → Pitfalls to Avoid → References → Check Understanding.
4. **Use the curriculum as a path, not a cage** — you can teach any software engineering topic. If it's not listed, apply the same teaching structure.
5. **Recommend the next topic** — after finishing a concept, tell the learner what to study next within the same domain, and when ready, what domain to progress to.

> Teaching structure reference: (Hook → Intuition → Technical → Pros/Cons → Comparison → Best Practices → Pitfalls → References → Questions) — the full definitions are in the agent instructions file.

---

You can teach **any topic** in software engineering. If a learner asks about something not listed below, use the same teaching structure from the agent instructions. The curriculum below is the recommended learning path, not a limitation.

When a learner asks about a topic, first locate it in this tree, explain how it fits into the bigger picture, and then teach it using the full structure (Hook → Intuition → Technical → Pros/Cons → Comparison → Best Practices → Pitfalls → References → Questions).

---

## Domain 1 — How Computers Actually Work

> The foundation everything else is built on. Never skip or assume.

**Computer Architecture:**

- Binary, bits, bytes, hexadecimal — why computers speak in 1s and 0s
- CPU anatomy: ALU, control unit, registers, program counter, instruction cycle
- CPU cache hierarchy: L1/L2/L3 — why cache exists and why it's tiny
- Instruction pipelining and branch prediction — how CPUs stay fast
- SIMD and vectorization — parallel math without threads
- CPU clock speed vs core count vs IPC — what actually makes a CPU fast

**Memory:**

- RAM: how DRAM works, row/column addressing, latency vs bandwidth
- Virtual memory: pages, page tables, TLB, page faults
- Stack vs heap: layout, allocation, why stack is faster
- Cache lines and false sharing — the hidden performance killer
- Memory-mapped files: mmap internals
- NUMA (Non-Uniform Memory Access) — why location of memory matters at scale

**Storage:**

- HDD vs SSD vs NVMe — physical differences and their performance implications
- How SSDs work: NAND flash, wear leveling, write amplification, P/E cycles
- File systems: ext4, APFS, NTFS — how they organize data, journaling, fsync
- I/O modes: buffered, direct, async I/O (io_uring on Linux)
- RAID levels: 0, 1, 5, 6, 10 — trade-offs

**Operating Systems:**

- Kernel space vs user space — why the boundary exists
- System calls: what they are, how the CPU switches modes (interrupt mechanism)
- Process management: fork, exec, copy-on-write, zombie processes
- Threads: kernel threads vs user threads, M:N threading models
- Scheduling algorithms: FIFO, Round Robin, CFS (Linux Completely Fair Scheduler)
- Concurrency primitives: mutexes, semaphores, condition variables, spinlocks, RW locks
- Deadlock: conditions (Coffman), detection, prevention, avoidance (Banker's algorithm)
- Memory management: malloc internals, buddy allocator, slab allocator
- Inter-process communication: pipes, named pipes, Unix sockets, shared memory, message queues
- Signals and interrupts
- File descriptors, epoll/kqueue — how async I/O multiplexing works

**Networking (Layer by Layer):**

- Physical and Data Link layer: Ethernet, MAC addresses, ARP
- Network layer: IP addresses, subnets, CIDR, routing tables, BGP basics
- Transport layer: TCP (three-way handshake, flow control, congestion control, TIME_WAIT), UDP, QUIC
- Application layer: HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC), WebSockets, gRPC, DNS
- TLS: certificate chains, handshake (RSA vs ECDHE), forward secrecy, OCSP stapling
- Load balancing at L4 vs L7: NAT vs proxy, session persistence
- What happens when you type a URL — full 15-step walkthrough

---

## Domain 2 — Data Structures & Algorithms

> The language of problem-solving. Teaches how to think about efficiency.

**Complexity Analysis:**

- Big O notation: time and space — formal definition, not just memorization
- Amortized analysis — why dynamic arrays are O(1) append on average
- Best/average/worst case — why average matters more than worst in practice
- Common complexity classes: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ)

**Linear Data Structures:**

- Arrays vs dynamic arrays (ArrayList/Vector) — memory layout, growth factor
- Linked lists: singly, doubly, circular — when the pointer overhead is worth it
- Stacks: applications (call stack, undo, expression parsing)
- Queues: circular buffer implementation, deque
- Skip lists — probabilistic alternative to balanced BST

**Hash-Based Structures:**

- Hash functions: properties of a good hash, djb2, MurmurHash, xxHash
- Collision resolution: separate chaining vs open addressing (linear/quadratic probing, double hashing)
- Load factor and rehashing — when and how it happens
- Consistent hashing — for distributed systems (covered also in Domain 5)
- Bloom filters — probabilistic set membership, false positive rate
- Count-Min Sketch — approximate frequency counting at scale
- HyperLogLog — approximate cardinality estimation (how Redis counts uniques)

**Tree Structures:**

- Binary trees: traversal orders (pre/in/post/level), recursive vs iterative
- Binary Search Trees: insert, delete, successor, predecessor
- Balanced BSTs: AVL rotations, Red-Black tree properties (why Linux uses RB-trees)
- B-trees and B+trees — why databases use them (disk block alignment, sequential reads)
- Tries (prefix trees): autocomplete, IP routing, spell checkers
- Segment trees and Fenwick (BIT) trees — range queries and point updates
- Heap / Priority Queue: binary heap, heapify, heap sort, use in Dijkstra

**Graph Structures:**

- Representations: adjacency matrix vs adjacency list — when each is better
- BFS: shortest path in unweighted graphs, level-order traversal
- DFS: cycle detection, topological sort, connected components, SCC (Kosaraju, Tarjan)
- Shortest path: Dijkstra (weighted), Bellman-Ford (negative weights), Floyd-Warshall (all pairs)
- Minimum Spanning Tree: Prim's, Kruskal's, union-find (DSU)
- Network flow: Max-flow/min-cut (Ford-Fulkerson, Edmonds-Karp)

**Algorithm Paradigms:**

- Divide and conquer: merge sort, quick sort, binary search — master theorem for recurrences
- Dynamic programming: overlapping subproblems, optimal substructure, memoization vs tabulation
  - Classic problems: knapsack, LCS, LIS, edit distance, coin change, matrix chain
- Greedy algorithms: when greedy is provably optimal (matroids), when it fails
- Backtracking: constraint satisfaction, N-queens, Sudoku solver
- Two pointers, sliding window, prefix sums — common interview patterns
- Bit manipulation: masks, shifts, XOR tricks

**Sorting:**

- Comparison sorts: bubble, insertion, selection, merge, quick, heap — stability, in-place
- Non-comparison sorts: counting, radix, bucket — when O(n) is possible
- Tim sort — why Python and Java use it (hybrid merge + insertion)
- External sorting — when data doesn't fit in RAM

---

## Domain 3 — Programming Languages & Paradigms

> Understanding WHY languages are designed the way they are.

**Type Systems:**

- Static vs dynamic typing — type checking at compile vs runtime
- Strong vs weak typing — implicit coercion
- Structural vs nominal typing — TypeScript vs Java
- Type inference — how Hindley-Milner works
- Generics / parametric polymorphism — why `List<T>` is better than `List<Object>`
- Variance: covariance, contravariance, invariance — why `List<Dog>` is not a `List<Animal>`

**Memory Management:**

- Manual memory management (C/C++): malloc/free, common bugs (use-after-free, double-free, buffer overflow)
- Garbage collection: mark-and-sweep, generational GC, reference counting (Python), GC pauses
- JVM GC algorithms: Serial, G1, ZGC, Shenandoah — latency vs throughput trade-offs
- Rust ownership model: ownership, borrowing, lifetimes — compile-time memory safety without GC
- RAII pattern — deterministic cleanup

**Concurrency Models:**

- Threads + shared memory: race conditions, critical sections, happens-before
- Locks: mutex, RW lock, optimistic locking, lock-free algorithms (CAS, ABA problem)
- Async/await and event loops: Node.js event loop (call stack, task queue, microtask queue), Python asyncio
- Goroutines and channels (Go): M:N scheduling, communicating sequential processes (CSP)
- Actor model (Erlang/Akka): message passing, no shared state, supervision trees
- Software Transactional Memory (Clojure STM)
- Reactive programming: Observables, backpressure (RxJS, Reactor, RxJava)

**Programming Paradigms:**

- Object-Oriented: encapsulation, inheritance, polymorphism, abstraction — and where OOP fails
- Functional: pure functions, immutability, higher-order functions, monads, functor, applicative
- Procedural vs declarative — SQL as an example of declarative
- Metaprogramming: macros, reflection, annotations/decorators, code generation
- Logic programming: Prolog — unification and backtracking

**Language-Specific Deep Dives:**

- **JavaScript/TypeScript**: event loop, prototype chain, closures, this binding, module systems (CJS/ESM), V8 hidden classes and JIT deoptimization, TypeScript type system internals
- **Python**: GIL and why it exists, asyncio internals, generators/coroutines, descriptors, metaclasses, CPython bytecode
- **Go**: goroutine scheduler (GMP model), channel internals, garbage collector (tri-color mark-and-sweep), interface satisfaction at compile time, escape analysis
- **Rust**: ownership and borrow checker in depth, lifetime elision rules, trait objects vs generics, async Rust (Futures, Pin, async runtime tokio vs async-std), unsafe Rust
- **Java/JVM**: class loading, JIT compilation (C1 vs C2 compilers), JVM memory model (JMM), volatile and happens-before, Java concurrency (synchronized, volatile, java.util.concurrent)
- **C/C++**: pointer arithmetic, undefined behavior, memory model, RAII, move semantics, template metaprogramming, undefined behavior sanitizer

**Compiler & Runtime Internals:**

- Compilation pipeline: lexing → parsing → AST → semantic analysis → IR → optimization → code gen
- Interpreter vs JIT vs AOT compilation — trade-offs (startup time vs peak performance)
- SSA form and common compiler optimizations (constant folding, dead code elimination, inlining)
- How V8 optimizes JavaScript: hidden classes, inline caches, Turbofan
- How the JVM JITs Java: tiered compilation, OSR (on-stack replacement), escape analysis

---

## Domain 4 — Databases

> The persistence layer — understanding it deeply separates good from great engineers.

**Relational Databases:**

- Relational model: relations, tuples, attributes, keys (candidate, primary, foreign, composite)
- SQL: DDL, DML, DCL — joins (inner, outer, cross, self), window functions, CTEs, subqueries
- Normalization: 1NF → 2NF → 3NF → BCNF — what each eliminates and what it costs
- ACID: atomicity (WAL + undo log), consistency (constraints), isolation (lock-based, MVCC), durability (WAL + fsync)
- Transaction isolation levels: read uncommitted, read committed, repeatable read, serializable — and their anomalies (dirty read, non-repeatable read, phantom read, write skew)
- MVCC (Multi-Version Concurrency Control): how PostgreSQL and MySQL InnoDB implement it
- Write-Ahead Logging (WAL): checkpointing, crash recovery, log sequence numbers
- B+tree index internals: pages, fill factor, index bloat, covering indexes, index-only scans
- Other index types: Hash (PostgreSQL), GIN (full-text, arrays, JSONB), GiST (spatial, range types), BRIN (time-series)
- Query planning: cost-based optimizer, statistics (pg_stats), explain/analyze, join algorithms (nested loop, hash join, merge join)
- Connection pooling: PgBouncer — why you can't have 10,000 direct DB connections
- PostgreSQL-specific: MVCC visibility, autovacuum, TOAST, partitioning, logical replication

**Distributed Databases:**

- Replication: statement-based vs row-based vs logical, leader-follower, multi-leader, leaderless (Dynamo-style)
- Replication lag: read-your-writes, monotonic reads, consistent prefix reads
- Sharding: range-based, hash-based, directory-based — resharding challenges
- Distributed transactions: 2PC (two-phase commit), 3PC, Paxos/Raft-based commit
- Google Spanner: TrueTime API, external consistency, globally distributed ACID
- CockroachDB, YugabyteDB — NewSQL approaches

**NoSQL:**

- Key-value: Redis (data structures, persistence RDB/AOF, Lua scripting, cluster mode), DynamoDB (consistent hashing, eventually consistent reads, conditional writes)
- Document: MongoDB (BSON, aggregation pipeline, atlas search, change streams, transactions), Firestore
- Wide-column: Cassandra (ring topology, consistent hashing, tunable consistency, compaction strategies, tombstones), HBase (HDFS-backed, strong consistency, coprocessors)
- Graph: Neo4j (property graph model, Cypher, traversal algorithms), Amazon Neptune
- Time-series: InfluxDB, TimescaleDB, Prometheus storage engine
- Search: Elasticsearch / OpenSearch (inverted index, TF-IDF, BM25, analyzer pipeline, shards and replicas, near-real-time indexing)
- Vector databases: pgvector, Pinecone, Weaviate, Chroma — ANN algorithms (HNSW, IVF), use in semantic search and RAG

---

## Domain 5 — System Design & Distributed Systems

> How large-scale systems are built to handle millions of users.

**Scalability:**

- Vertical vs horizontal scaling
- Stateless vs stateful services — why stateless enables horizontal scaling
- Load balancers: L4 vs L7, algorithms (round robin, least connections, consistent hash), health checks, sticky sessions
- Auto-scaling: reactive vs predictive, scale-in safety
- CAP Theorem: formal definition, partition tolerance is not optional, PACELC extension
- BASE vs ACID — eventual consistency in practice
- Consistency models: linearizability, sequential consistency, causal consistency, eventual consistency

**Caching:**

- Cache placement: client-side, CDN, reverse proxy, application-level, database cache
- Cache strategies: cache-aside (lazy loading), read-through, write-through, write-behind (write-back)
- Cache invalidation: TTL, event-driven invalidation, cache tags — why invalidation is hard
- Cache stampede / thundering herd: probabilistic early expiration, locking patterns
- Redis deep dive: data structures (string, list, set, sorted set, hash, stream, HyperLogLog, geospatial), pub/sub, Lua scripting, persistence (RDB snapshots vs AOF), clustering (hash slots), Sentinel for HA
- Memcached vs Redis — when the simpler tool wins
- CDN: edge caching, cache-control headers, origin shield, cache purge strategies, Cloudflare vs CloudFront

**Messaging & Event Streaming:**

- Synchronous vs asynchronous communication — coupling, latency, resilience trade-offs
- Message queues: RabbitMQ (exchanges, bindings, dead-letter queues, quorum queues), ActiveMQ, Amazon SQS
- Event streaming: Kafka (log structure, partitions, consumer groups, offset management, compaction, ISR, exactly-once semantics with transactions), Amazon Kinesis
- Kafka vs RabbitMQ — retention, replay, ordering guarantees
- Delivery semantics: at-most-once, at-least-once, exactly-once — how each is achieved
- Backpressure: what it is, how Kafka, reactive streams, and TCP handle it
- Event-driven architecture patterns: event sourcing, CQRS, choreography vs orchestration sagas

**Distributed Systems Theory:**

- Fallacies of distributed computing (Peter Deutsch's 8 fallacies)
- Consistent hashing: virtual nodes, why it minimizes data movement
- Clock synchronization: NTP limitations, logical clocks (Lamport), vector clocks, hybrid logical clocks
- Leader election: bully algorithm, Raft election, ZooKeeper ZAB
- Consensus: Paxos (single-decree, multi-Paxos), Raft (log replication, leader election, membership changes) — understand the problem, not just the names
- Distributed transactions: 2PC (coordinator failure problem), Saga pattern (compensating transactions), TCC (try-confirm-cancel)
- CRDTs: G-Counter, PN-Counter, OR-Set — conflict-free merging for eventual consistency
- Idempotency: idempotency keys, natural idempotency (PUT vs POST), deduplication at the consumer

**Large-Scale System Design Practice:**

- URL shortener (100M URLs, 1B reads/day)
- Distributed rate limiter (token bucket in Redis, sliding window log, sliding window counter)
- Distributed cache (consistent hashing, replication, eviction policies)
- Notification system (fan-out: push vs pull, multi-channel, preference management)
- Twitter/X feed (fanout-on-write vs fanout-on-read, celebrity problem hybrid approach)
- YouTube (video upload pipeline, transcoding, CDN distribution, view counter with HyperLogLog)
- Uber/Lyft (geospatial indexing with geohash/S2, matching algorithm, surge pricing)
- Google Search (crawling, inverted index, PageRank, serving tier)
- WhatsApp (connection management, message delivery receipts, end-to-end encryption, group messaging)
- Distributed job scheduler (cron-like, leader election for claiming jobs, idempotency, retry with backoff)
- Search autocomplete (trie vs indexed prefix, personalization, real-time updates)
- Distributed counter (accuracy vs performance: approximate with HyperLogLog, exact with Cassandra counters)

---

## Domain 6 — Software Architecture & Design Patterns

> How to structure code so it remains maintainable as it grows.

**SOLID Principles (with bad → good code examples for each):**

- Single Responsibility Principle: one reason to change
- Open/Closed Principle: open for extension, closed for modification
- Liskov Substitution Principle: subtypes must be substitutable — why violating it causes runtime bugs
- Interface Segregation Principle: many focused interfaces over one fat interface
- Dependency Inversion Principle: depend on abstractions, inject implementations

**Gang of Four Design Patterns:**

- Creational: Factory Method, Abstract Factory, Builder, Singleton (and why to avoid it), Prototype
- Structural: Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy
- Behavioral: Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor

**Architectural Patterns:**

- Layered / N-tier: presentation → business logic → data — and where it breaks down
- Clean Architecture (Robert Martin): entities → use cases → interface adapters → frameworks
- Hexagonal Architecture (Ports & Adapters): driving vs driven adapters, testability benefits
- CQRS: separate read model from write model — when it's worth the complexity
- Event Sourcing: log of events as source of truth, rebuilding state, temporal queries, challenges (schema evolution, snapshots)
- Microservices vs Monolith vs Modular Monolith — honest comparison with team size and maturity
- Service Mesh: sidecar proxy, Istio, Envoy — observability, traffic management, mTLS

**Domain-Driven Design (DDD):**

- Strategic: bounded contexts, context map, ubiquitous language, anti-corruption layer
- Tactical: entities, value objects, aggregates (consistency boundary), domain events, repositories, domain services, factories
- Aggregate design rules: small aggregates, reference by ID across boundaries, eventual consistency between aggregates

**API Design:**

- RESTful principles (and the 6 most common violations)
- REST vs GraphQL vs gRPC vs tRPC — honest trade-offs, not hype
- API versioning strategies: URI path, header, query param — pros and cons
- Pagination: offset vs cursor-based — why cursor is required for real-time feeds
- Error response design: RFC 7807 Problem Details
- Idempotency keys for mutations
- Rate limiting and quotas in API design

---

## Domain 7 — Frontend Engineering

> Building UIs that are fast, accessible, and maintainable.

**Browser Internals:**

- Browser architecture: renderer process, browser process, GPU process, network process
- Critical rendering path: HTML parsing → DOM → CSSOM → Render tree → Layout → Paint → Composite
- JavaScript engine: V8 hidden classes (shapes), inline caches, Ignition interpreter, Turbofan JIT, deoptimization triggers
- Event loop in the browser: call stack, Web APIs, task queue (macrotasks), microtask queue (Promises), requestAnimationFrame
- Reflows vs repaints vs compositing — what triggers them and why compositing is cheapest

**Web Performance:**

- Core Web Vitals: LCP, CLS, INP — what affects each and how to fix
- Resource loading: preload, prefetch, preconnect, modulepreload
- Code splitting and lazy loading: dynamic import(), route-based splitting
- Tree shaking: how bundlers eliminate dead code (ES module static analysis)
- Images: WebP/AVIF, responsive images (srcset, sizes), lazy loading, image CDN
- Web Workers: offloading CPU work from the main thread
- Service Workers: cache API, offline support, background sync, push notifications
- Lighthouse and Chrome DevTools Performance panel — how to profile

**JavaScript/TypeScript Deep Dives:**

- Prototypal inheritance: prototype chain, Object.create, class syntax desugaring
- Closures: lexical scoping, use in encapsulation, common interview pitfall (loop closures)
- this binding: default, implicit, explicit (call/apply/bind), new, arrow function
- Modules: CommonJS (require/module.exports), ESM (import/export), circular dependencies
- Promises and async/await: Promise states, microtask queue, Promise.all vs Promise.allSettled vs Promise.race vs Promise.any
- TypeScript: structural typing, declaration merging, conditional types, mapped types, template literal types, discriminated unions, infer keyword

**CSS Internals:**

- Normal flow, box model (content/padding/border/margin)
- Stacking contexts and z-index — why z-index doesn't always work
- Flexbox: main axis, cross axis, flex-grow/shrink/basis, alignment
- CSS Grid: explicit vs implicit grid, grid areas, auto-placement algorithm
- CSS custom properties (variables) and inheritance
- BEM, CSS Modules, CSS-in-JS, Tailwind — honest comparison

**Frameworks:**

- React: reconciliation (fiber architecture, work loop, priority scheduling), hooks (useState, useEffect, useMemo, useCallback, useRef — closures and dependency arrays), concurrent features (transitions, Suspense), server components
- Vue: reactivity system (Proxy-based in Vue 3 vs defineProperty in Vue 2), composition API vs options API
- Angular: zone.js change detection, signals (Angular 17+), dependency injection system
- State management: Redux (action → reducer → store → view), Zustand, Jotai, Recoil, TanStack Query (server state vs client state distinction), Signals pattern

**Web Security (Frontend):**

- XSS: reflected, stored, DOM-based — prevention (CSP, output encoding)
- CSRF: SameSite cookies, CSRF tokens
- CORS: preflight, allowed origins, credentials
- Content Security Policy: directives, nonce-based, hash-based
- Secure cookie attributes: HttpOnly, Secure, SameSite

---

## Domain 8 — Backend Engineering

> Building APIs and services that are correct, secure, and fast.

**Authentication & Authorization:**

- Session-based auth: cookie, server-side session store, CSRF exposure
- JWT: header.payload.signature, signing algorithms (HS256 vs RS256 vs ES256), stateless vs stateful (token revocation problem), access + refresh token pattern
- OAuth 2.0: authorization code flow, PKCE, client credentials, implicit flow (deprecated) — when to use each grant
- OpenID Connect (OIDC): ID token, userinfo endpoint, discovery document
- API keys vs JWTs vs mutual TLS — when each is appropriate
- RBAC vs ABAC — role-based vs attribute-based access control
- Multi-factor authentication: TOTP (RFC 6238), WebAuthn/FIDO2, SMS (and why it's weak)
- Password storage: bcrypt, scrypt, Argon2 — why MD5/SHA-1 is wrong for passwords

**Backend Patterns:**

- Middleware pattern: request/response pipeline, cross-cutting concerns
- Repository pattern + Unit of Work — decoupling domain from persistence
- Service layer: orchestrating use cases
- Background jobs: task queues (BullMQ, Celery, Sidekiq), scheduling, idempotency
- Webhook patterns: delivery guarantees, signature verification, retry logic
- File upload: multipart upload, presigned URLs (S3), virus scanning, chunked upload
- Pagination: keyset/cursor pagination for large datasets
- Full-text search: integrating PostgreSQL FTS vs Elasticsearch
- Caching strategies at the application layer

**Web Frameworks (internals):**

- Express.js: middleware chain, router, how it wraps Node.js http
- NestJS: decorator-based DI, module system, guards/interceptors/pipes/filters
- FastAPI: Pydantic validation, async SQLAlchemy, Depends injection
- Spring Boot: IoC container, AOP, auto-configuration, Spring Security filter chain

**Email & Notifications:**

- SMTP: MX records, SPF, DKIM, DMARC — why email deliverability is hard
- Transactional email services: SES, SendGrid, Postmark — trade-offs
- Push notifications: APNs, FCM — device token management, silent push

---

## Domain 9 — Security Engineering

> Every engineer must understand security. Not optional.

**Cryptography:**

- Symmetric encryption: AES (block modes — ECB vs CBC vs GCM), ChaCha20-Poly1305
- Asymmetric encryption: RSA (why key size matters), ECC (ECDSA, ECDH)
- Hashing: SHA-2, SHA-3, BLAKE3 — collision resistance, preimage resistance
- MACs and HMACs — message authentication
- Digital signatures: non-repudiation, certificate authorities, chain of trust
- Key exchange: Diffie-Hellman, ECDHE — perfect forward secrecy
- Password hashing: Argon2id, bcrypt, scrypt — work factors, why you don't use SHA-256
- Common mistakes: using ECB mode, rolling your own crypto, nonce reuse in AES-GCM

**OWASP Top 10 (in depth):**

- A01 Broken Access Control: IDOR, privilege escalation, missing function-level auth
- A02 Cryptographic Failures: weak ciphers, plaintext secrets, sensitive data in logs
- A03 Injection: SQL injection (parameterized queries, ORM pitfalls), command injection, LDAP injection, NoSQL injection
- A04 Insecure Design: threat modeling, security requirements, abuse case analysis
- A05 Security Misconfiguration: default credentials, verbose errors, CORS wildcard
- A06 Vulnerable Components: dependency scanning (Snyk, Dependabot), SBOMs
- A07 Authentication Failures: credential stuffing, brute force, weak password policies
- A08 Software and Data Integrity: CI/CD pipeline security, supply chain attacks (SolarWinds, Log4Shell)
- A09 Logging Failures: what to log, what NOT to log (PII, secrets), tamper-proof logs
- A10 SSRF: impact, prevention, metadata endpoint abuse in cloud

**Infrastructure Security:**

- Principle of least privilege — everywhere
- Secrets management: environment variables vs Vault vs AWS Secrets Manager, secret rotation
- Zero trust networking: never trust, always verify, micro-segmentation
- Container security: image scanning, rootless containers, read-only filesystem, seccomp profiles
- Supply chain security: SLSA framework, SBOM, sigstore/cosign

---

## Domain 10 — DevOps & Infrastructure

> How software actually runs in production.

**Linux & Systems:**

- Linux file system hierarchy: /proc, /sys, /dev — what they expose
- Process management: ps, top, htop, strace, lsof, /proc/<pid>
- Networking tools: ss, netstat, tcpdump, curl, dig, traceroute
- systemd: unit files, journald, socket activation
- cgroups and namespaces — the building blocks of containers
- Performance tools: perf, vmstat, iostat, sar, flamegraph generation

**Containers:**

- Docker internals: union filesystems (overlay2), namespaces (PID, network, mount, UTS, IPC, user), cgroups for resource limits
- Image layers: how Dockerfile instructions create layers, layer caching, multi-stage builds
- Networking: bridge network, host network, overlay network (Swarm/K8s), CNI plugins
- Container security: running as non-root, capabilities, seccomp, AppArmor

**Kubernetes:**

- Architecture: control plane (API server, etcd, scheduler, controller manager) vs data plane (kubelet, kube-proxy, container runtime)
- Core objects: Pod, Deployment, ReplicaSet, Service, ConfigMap, Secret, Ingress, PVC
- Scheduling: resource requests/limits, node affinity, pod affinity/anti-affinity, taints/tolerations
- Networking: kube-proxy (iptables/ipvs), CNI (Calico, Cilium), Service discovery via DNS
- Storage: PersistentVolumes, StorageClasses, CSI drivers
- Health checks: liveness, readiness, startup probes — why they matter and common mistakes
- Autoscaling: HPA (CPU/custom metrics), VPA, KEDA (event-driven scaling)
- Security: RBAC, ServiceAccounts, NetworkPolicies, PodSecurity Standards, OPA Gatekeeper

**Infrastructure as Code:**

- Terraform: provider model, state file (remote state, locking), plan/apply cycle, modules, workspaces
- Pulumi: IaC in general-purpose languages, state management
- Helm: charts, values, templating, hooks, chart repositories

**CI/CD:**

- GitHub Actions: workflow syntax, matrix builds, caching, secrets, OIDC for cloud auth
- GitLab CI: pipeline stages, artifacts, environments, review apps
- Deployment strategies: rolling, blue/green, canary — how to implement in Kubernetes
- GitOps: ArgoCD, Flux — declarative desired state in Git
- Release engineering: semantic versioning, changelog automation, feature flags for dark launches

**Cloud Platforms:**

- AWS core services: EC2, ECS/EKS, Lambda, S3, RDS, ElastiCache, SQS/SNS, CloudFront, Route53, IAM, VPC, ALB/NLB
- GCP core services: GKE, Cloud Run, Cloud Functions, BigQuery, Pub/Sub, Cloud Spanner, Firebase
- Azure core services: AKS, Azure Functions, Blob Storage, Cosmos DB, Service Bus
- Cloud-native patterns: managed services vs self-hosted, egress costs, multi-cloud vs single-cloud

**Serverless:**

- Cold start problem: causes, mitigation (provisioned concurrency, warm-up pings, language choice)
- Execution model: event-driven, stateless, ephemeral filesystem
- When NOT to use serverless: long-running tasks, stateful workloads, cost at scale

---

## Domain 11 — Testing & Quality

> Confidence to change code without fear.

**Testing Strategy:**

- Test pyramid: unit (many, fast) → integration (some) → E2E (few, slow) — why the pyramid shape
- Ice cream cone anti-pattern — and why teams fall into it
- Test doubles: dummy, stub, fake, spy, mock — precise definitions, not just "mock"
- Test isolation: why tests should not share state, flakiness causes
- Shift-left testing: testing earlier in the development cycle

**Unit Testing:**

- Arrange-Act-Assert (AAA) pattern
- What to test: behavior, not implementation — avoid testing private methods directly
- Test naming: should/when/given-when-then conventions
- Coverage metrics: line, branch, mutation — why 100% line coverage is a false goal
- Testing frameworks: Jest, Vitest, pytest, Go testing package, JUnit

**Integration Testing:**

- Testing with real databases (Testcontainers)
- Testing HTTP APIs: supertest, httpx, rest-assured
- Contract testing with Pact — consumer-driven contracts
- Database testing: transactional rollback vs truncate strategies

**E2E Testing:**

- Playwright vs Cypress vs Selenium — honest comparison
- Page Object Model — maintainable selectors
- Visual regression testing: Percy, Chromatic
- Flakiness: causes (timing, test pollution, environment) and fixes

**Advanced Testing:**

- Property-based testing: QuickCheck, fast-check, Hypothesis — testing invariants
- Mutation testing: Stryker, PITest — measuring test quality
- Fuzzing: AFL, libFuzzer, go-fuzz — finding security and stability bugs
- Load testing: k6, Locust, Gatling — designing realistic load scenarios
- Chaos engineering: fault injection, Chaos Monkey, Gremlin, Chaos Mesh

---

## Domain 12 — Performance Engineering

> Making software fast — with data, not guesses.

**Profiling:**

- CPU profiling: sampling vs instrumentation, flamegraphs (Brendan Gregg)
- Memory profiling: heap dumps, allocation tracking, GC analysis
- I/O profiling: disk and network I/O, blocking vs non-blocking
- Database profiling: slow query logs, EXPLAIN ANALYZE, pg_stat_statements

**Application Performance:**

- N+1 query problem: detection and solutions (eager loading, DataLoader)
- Memory leaks: event listener leaks, closure-held references, WeakMap/WeakRef
- Lock contention: profiling, reducing critical section size, lock-free algorithms
- Cache tuning: hit rate optimization, eviction policy selection, key design
- Connection pooling: why direct connections don't scale, pool sizing formula (Little's Law)

**Network Performance:**

- Latency vs throughput — they are not the same
- TCP tuning: keep-alive, Nagle's algorithm, TCP_NODELAY
- HTTP keep-alive vs connection pooling
- Compression: gzip vs Brotli, when compression hurts (small responses, already-compressed content)
- Protocol choice: HTTP/1.1 vs HTTP/2 multiplexing vs HTTP/3 QUIC

**Frontend Performance:**

- JavaScript bundle size: bundle analysis (webpack-bundle-analyzer, source-map-explorer)
- Runtime performance: React profiler, avoiding unnecessary renders, virtualization for large lists
- Web Vitals optimization: LCP (preload hero image, CDN), CLS (size attributes on images), INP (long tasks, scheduler API)

---

## Domain 13 — Reliability & Operations

> Keeping systems alive when things go wrong.

**Observability (The Three Pillars):**

- Metrics: counters, gauges, histograms, summaries — Prometheus data model, PromQL, RED method (Rate, Errors, Duration), USE method (Utilization, Saturation, Errors)
- Logs: structured logging (JSON), log levels, correlation IDs for request tracing, what NOT to log
- Traces: distributed tracing, spans, context propagation (W3C Trace Context), OpenTelemetry
- Dashboards: Grafana, alerting rules, SLO-based alerts vs symptom-based vs cause-based

**Reliability Engineering:**

- SLIs, SLOs, SLAs: definitions, how to choose the right SLI for each service type
- Error budgets: the concept, burn rate alerts, how to use them to make release decisions
- Availability math: 99.9% = 8.7h downtime/year, 99.99% = 52m, 99.999% = 5m — what each requires
- Toil: definition, quantification, why reducing toil is an engineering investment

**Incident Management:**

- Incident severity levels and escalation
- On-call: runbooks, alerts that are actionable, avoiding alert fatigue
- Blameless postmortems: 5 Whys, contributing factors, action items
- Game days and chaos drills

**Deployment & Release:**

- Blue/green deployment: traffic switching, rollback, database migrations challenge
- Canary deployment: percentage rollout, automated rollback on error rate increase
- Feature flags: targeting, gradual rollout, kill switch, flag lifecycle management
- Database migration safety: expand-contract pattern, backward-compatible migrations

---

## Domain 14 — AI/ML Engineering

> Building and operating AI systems in production.

**Machine Learning Fundamentals (for engineers):**

- Supervised vs unsupervised vs reinforcement learning — conceptual understanding
- Model training pipeline: data → features → training → evaluation → serving
- Overfitting and underfitting — bias-variance trade-off
- Evaluation metrics: accuracy, precision, recall, F1, AUC-ROC — when each matters

**LLM & Generative AI Engineering:**

- Transformer architecture: attention mechanism, tokens, context window — intuition not math
- Prompt engineering: zero-shot, few-shot, chain-of-thought, system prompts
- RAG (Retrieval-Augmented Generation): chunking strategies, embedding models, vector search, reranking
- Fine-tuning vs RAG vs prompt engineering — when to use each
- Hallucination: why it happens, mitigation strategies (grounding, citations, temperature tuning)
- LLM evaluation: benchmarks, human eval, LLM-as-judge pattern

**ML Systems Engineering:**

- Feature stores: offline vs online features, point-in-time correctness, training-serving skew
- Model serving: REST vs gRPC, batching, latency SLOs, A/B testing models
- MLOps: experiment tracking (MLflow), model registry, deployment pipelines, model monitoring (data drift, concept drift)
- Vector databases: HNSW vs IVF indexes, approximate nearest neighbor trade-offs

---

## Domain 15 — Engineering Leadership & Practices

> The skills that make teams effective, not just individuals.

**Engineering Practices:**

- Code review: what to look for, how to give constructive feedback, PR size discipline
- Technical writing: Architecture Decision Records (ADRs), RFCs, runbooks, postmortems
- Technical debt: types (deliberate vs accidental, reckless vs prudent), when to pay it down
- Refactoring safely: strangler fig pattern, branch by abstraction, feature flags as safety nets
- Pair programming and mob programming — when they accelerate vs slow down

**Engineering Leadership:**

- Staff/Principal engineer skills: technical strategy, influence without authority, making proposals
- Technical roadmaps: horizon 1/2/3 planning, balancing feature work vs infrastructure
- Estimation: cone of uncertainty, t-shirt sizing, story points vs time estimates
- Engineering metrics: DORA (deployment frequency, lead time, MTTR, change failure rate)
- Mentoring: giving feedback, 1:1s, career development conversations

**Emerging Technologies:**

- WebAssembly: use cases (compute-heavy browser code, plugin systems), toolchains (Emscripten, wasm-pack)
- eBPF: observability without instrumentation, security enforcement, networking (Cilium)
- Edge computing: Cloudflare Workers, Deno Deploy — constraints and use cases
- Quantum computing: qubits, superposition, entanglement, Shor's/Grover's algorithms — conceptual awareness for engineers

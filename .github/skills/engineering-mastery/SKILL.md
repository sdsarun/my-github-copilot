---
name: engineering-mastery
description: "Use this skill when helping someone learn software engineering from scratch. Provides structured curriculum, teaching templates, practice project bank, and assessment rubrics. Activate for any learning, teaching, mentoring, or knowledge-assessment session."
---

# Engineering Mastery Curriculum

A structured, depth-first curriculum for going from developer to master software engineer. This skill provides the teaching framework, topic maps, practice project bank, and assessment rubrics.

---

## Learning Principles

### The Feynman Test

A learner truly understands a concept when they can:

1. Explain it in plain language (no jargon)
2. Give a concrete real-world example
3. Identify where the analogy breaks down
4. Apply it to a new problem they haven't seen before

Do not advance until all four are demonstrated.

### The Verification Habit

After every concept, ask: "How would you verify this is true?"
Train the learner to find primary sources: official documentation, RFCs, research papers, benchmarks.
Never accept "the AI said so" or "I read it somewhere" as verification.

---

## Topic Map by Phase

### Phase 0 — Prerequisites

| Topic                         | Key Questions to Answer                                                      | Verification Source                                               |
| ----------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Memory model (CPU, RAM, Disk) | What makes disk slow? What makes RAM fast? Why is CPU cache tiny but fast?   | "What Every Programmer Should Know About Memory" (Ulrich Drepper) |
| Processes vs Threads          | What do they share? What don't they? When does one thread crashing kill all? | OS textbook, `man 2 fork`                                         |
| Full HTTP lifecycle           | 13 steps from URL to rendered page                                           | MDN Web Docs, Wireshark experiment                                |
| TCP vs UDP                    | Why does TCP guarantee order? What does UDP sacrifice for speed?             | RFC 793, RFC 768                                                  |
| TLS handshake                 | What's the point of the certificate? What's a symmetric key?                 | TLS 1.3 RFC                                                       |

### Phase 1 — Data Structures & Algorithms

| Topic       | Core Insight                                                 | Common Misconception                                             |
| ----------- | ------------------------------------------------------------ | ---------------------------------------------------------------- |
| Arrays      | O(1) random access because memory is contiguous              | "Arrays are always fast" — cache misses kill large arrays        |
| Hash Tables | O(1) average because of the hash function                    | "O(1) always" — worst case is O(n) with bad hash/many collisions |
| Trees       | Hierarchy = fast search on ordered data                      | Confusing tree traversal orders (inorder ≠ BFS)                  |
| Graphs      | Generalization of trees — cycles allowed                     | Forgetting to handle cycles in DFS (infinite loop)               |
| Heaps       | Partial order — not fully sorted, but root is always min/max | Assuming heaps are sorted arrays                                 |

**Complexity Reference:**

| Operation | Array | Linked List | Hash Table | BST (balanced) |
| --------- | ----- | ----------- | ---------- | -------------- |
| Access    | O(1)  | O(n)        | O(1) avg   | O(log n)       |
| Search    | O(n)  | O(n)        | O(1) avg   | O(log n)       |
| Insert    | O(n)  | O(1)        | O(1) avg   | O(log n)       |
| Delete    | O(n)  | O(1)        | O(1) avg   | O(log n)       |

### Phase 2 — System Design

| Topic                        | The Core Trade-off                                       | Real-World Example                                                                                 |
| ---------------------------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| CAP Theorem                  | You can only guarantee 2 of 3 during a network partition | Cassandra (AP), HBase (CP), traditional RDBMS (CA — but partitions aren't optional)                |
| Horizontal vs Vertical       | Horizontal = more machines. Vertical = bigger machine.   | YouTube adds more video servers (horizontal). Early-stage startup upgrades RDS instance (vertical) |
| Consistent Hashing           | Minimize data movement when nodes are added/removed      | Amazon DynamoDB, Cassandra, Memcached                                                              |
| SQL vs NoSQL                 | ACID vs scale/flexibility                                | Instagram uses PostgreSQL for core data, Cassandra for activity feeds                              |
| Cache Aside vs Write-Through | Who is responsible for populating cache?                 | Redis with cache-aside is most common; write-through used in CDNs                                  |
| Kafka vs RabbitMQ            | Retention + replay vs routing + acknowledgement          | Kafka at LinkedIn for activity tracking; RabbitMQ at many fintech for task queues                  |
| Sharding                     | Distribute data across nodes                             | MongoDB sharding, Vitess for MySQL sharding at YouTube                                             |

### Phase 3 — Architecture & Design Patterns

| Pattern            | Problem It Solves                        | When NOT to use it                                                    |
| ------------------ | ---------------------------------------- | --------------------------------------------------------------------- |
| Repository Pattern | Decouples domain from persistence        | Overkill for tiny scripts; adds boilerplate                           |
| Factory Pattern    | Decouple creation from usage             | When there's only ever one type — YAGNI                               |
| Observer/Event     | Decouple emitters from listeners         | Hard to trace flows; debugging becomes complex                        |
| Strategy Pattern   | Replace if/else with polymorphism        | When behavior never changes                                           |
| Decorator Pattern  | Add behavior without subclassing         | Deep chains become confusing                                          |
| CQRS               | Separate read model from write model     | Complexity not worth it unless read/write demands differ dramatically |
| Saga Pattern       | Distributed transactions across services | Complex to implement; only needed in microservices                    |

---

## Practice Project Bank

### Tier 1 — Fundamentals (Phase 1)

Implement from scratch, no built-in equivalents.

| Project            | Skills Tested                                        | Minimum Requirements                                                       |
| ------------------ | ---------------------------------------------------- | -------------------------------------------------------------------------- |
| Hash Table         | Hash functions, collision handling, dynamic resizing | Separate chaining + open addressing implementations, load factor threshold |
| LRU Cache          | Doubly linked list + hash map composition            | O(1) get and put, proper eviction                                          |
| Binary Search Tree | Tree traversal, recursion, balancing awareness       | Insert, delete, search, inorder traversal, find successor                  |
| Graph BFS/DFS      | Graph representation, traversal, cycle detection     | Both traversal types, detect cycles, find shortest path (unweighted)       |
| Min-Heap           | Heap property, heapify up/down                       | Insert, extract-min, heapify from array                                    |

### Tier 2 — System Design (Phase 2)

Write a complete design document (not code). Grade on: requirements clarity, data model, API design, scale handling, failure handling.

| Project               | Constraints to handle                              | Gotchas to test                                                          |
| --------------------- | -------------------------------------------------- | ------------------------------------------------------------------------ |
| URL Shortener         | 100M URLs, 1B reads/day                            | Hash collision handling, custom aliases, expiration, analytics           |
| Rate Limiter          | 10k RPS per service                                | Distributed rate limiting (Redis), sliding window vs token bucket choice |
| Distributed Cache     | 1TB data, 99.99% uptime                            | Consistent hashing when nodes join/leave, cache stampede prevention      |
| Notification System   | 10M users, push/email/SMS                          | Fan-out strategies, idempotency, retry logic, preference management      |
| Twitter Feed          | 100M users, celebrities with 10M followers         | Fanout-on-write vs fanout-on-read, hybrid approach for celebrities       |
| Video Upload Pipeline | Like YouTube, 500 hours of video/minute            | Chunked upload, transcoding workers, CDN distribution, progress tracking |
| Job Scheduler         | Cron-like, distributed, no single point of failure | Idempotency, at-least-once delivery, missed-job recovery                 |
| Search Autocomplete   | Google-like, 10ms response, 1B queries/day         | Trie vs inverted index, personalization, prefix caching                  |

### Tier 3 — Coding + Architecture (Phase 3)

Build real code with proper architecture.

| Project                      | Architecture Goal                                       | Acceptance Criteria                                                             |
| ---------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Refactor spaghetti REST API  | Apply clean architecture + repository pattern           | Zero persistence code in controllers, domain logic isolated, all tests pass     |
| E-commerce domain model      | DDD: entities, value objects, aggregates, domain events | Order invariants enforced (can't ship unconfirmed order), money as value object |
| Build a mini event bus       | Observer pattern + async dispatch                       | Subscribe, publish, unsubscribe, error isolation between handlers               |
| Implement retry with backoff | Resilience patterns                                     | Exponential backoff, jitter, max retries, circuit breaker integration           |

---

## Assessment Rubrics

### Coding Exercise — Scoring Guide

| Score | Criteria                                                                      |
| ----- | ----------------------------------------------------------------------------- |
| 5/5   | Correct, optimal complexity, handles edge cases, can explain why it's optimal |
| 4/5   | Correct, near-optimal, misses one edge case or can't fully explain complexity |
| 3/5   | Mostly correct, O(n²) where O(n log n) was possible                           |
| 2/5   | Partially correct, fundamental misunderstanding of a concept                  |
| 1/5   | Wrong approach, but shows understanding of the problem                        |
| 0/5   | Couldn't start without significant hints                                      |

### System Design — Scoring Guide

| Dimension       | 5/5                                                      | 3/5                                 | 1/5                                 |
| --------------- | -------------------------------------------------------- | ----------------------------------- | ----------------------------------- |
| Requirements    | Asks clarifying questions, defines scale, defines SLOs   | States basic requirements           | Starts designing without clarifying |
| Data Model      | Correct schema, handles relationships, considers indexes | Mostly correct, missing some fields | Incorrect or missing                |
| API Design      | RESTful, proper status codes, pagination, idempotency    | Mostly correct                      | Incorrect or vague                  |
| Scalability     | Identifies bottleneck, proposes horizontal scaling       | Mentions scaling vaguely            | Assumes single server               |
| Fault Tolerance | Handles single node failure, data replication            | Mentions replication                | No failure handling                 |
| Trade-offs      | Explicitly names 3+ trade-offs in their design           | Names 1-2                           | Names none                          |

---

## Question Bank (by Phase)

### Phase 2 — System Design Questions

**Easy:**

- What is the difference between horizontal and vertical scaling?
- Explain what a CDN does. When would you NOT use one?
- What problem does consistent hashing solve?

**Medium:**

- A database query is taking 2 seconds. Walk me through how you'd diagnose and fix it.
- Your cache hit rate drops from 95% to 40% after a deployment. What could cause this?
- You need to implement rate limiting across 100 API servers. Why can't you just use an in-memory counter on each server?

**Hard:**

- You're designing a distributed counter (like YouTube view count). How do you make it accurate AND fast? What trade-offs do you make?
- Your team wants to migrate a PostgreSQL table from one schema to another with zero downtime. Walk through how you'd do it.
- How would you design a system that guarantees exactly-once delivery of a payment event across two independent services?

---

## Red Flags in Learner Answers

Watch for these and address them immediately:

- **Silver bullet thinking**: "I'll just use Kafka for everything" — always ask "why specifically Kafka and not X?"
- **Cargo culting**: "Netflix uses microservices so we should too" — context matters enormously
- **Ignoring failure modes**: A design with no mention of what happens when a node fails
- **Premature optimization**: Adding Cassandra and Kafka to a system with 100 users
- **Under-specification**: "I'll use a database" — which one? why? what schema?
- **No trade-off awareness**: Choosing an approach without naming what it sacrifices

---

## Complete Resource Library

Every resource below is a verified primary or canonical source. Only cite these — do not invent other URLs.

### Domain 1 — How Computers Work

| Resource                                              | Type      | URL / Where to find               |
| ----------------------------------------------------- | --------- | --------------------------------- |
| What Every Programmer Should Know About Memory        | Paper     | akkadia.org/drepper/cpumemory.pdf |
| Computer Systems: A Programmer's Perspective (CS:APP) | Book      | csapp.cs.cmu.edu                  |
| The Missing Semester of Your CS Education             | Course    | missing.csail.mit.edu             |
| Operating Systems: Three Easy Pieces                  | Free book | ostep.org                         |
| Linux man pages                                       | Reference | man7.org/linux/man-pages          |
| Brendan Gregg's Systems Performance blog              | Blog      | brendangregg.com                  |

### Domain 2 — Data Structures & Algorithms

| Resource                          | Type                | URL / Where to find           |
| --------------------------------- | ------------------- | ----------------------------- |
| Introduction to Algorithms (CLRS) | Book                | MIT Press (standard textbook) |
| Algorithms by Sedgewick & Wayne   | Book + Course       | algs4.cs.princeton.edu        |
| LeetCode                          | Practice            | leetcode.com                  |
| Neetcode.io roadmap               | Structured practice | neetcode.io                   |
| Visualgo                          | Visual learning     | visualgo.net                  |
| Big-O Cheat Sheet                 | Reference           | bigocheatsheet.com            |

### Domain 3 — Programming Languages

| Resource                                    | Type             | URL / Where to find                    |
| ------------------------------------------- | ---------------- | -------------------------------------- |
| JavaScript: The Definitive Guide (Flanagan) | Book             | O'Reilly                               |
| TypeScript Handbook                         | Official docs    | typescriptlang.org/docs/handbook       |
| You Don't Know JS (YDKJS)                   | Free book series | github.com/getify/You-Dont-Know-JS     |
| The Go Programming Language                 | Book             | gopl.io                                |
| Go specification                            | Official spec    | go.dev/ref/spec                        |
| The Rust Programming Language ("the Book")  | Free book        | doc.rust-lang.org/book                 |
| Rustonomicon (unsafe Rust)                  | Free book        | doc.rust-lang.org/nomicon              |
| Python language reference                   | Official docs    | docs.python.org/3/reference            |
| Fluent Python by Ramalho                    | Book             | O'Reilly                               |
| JVM Internals                               | Article          | blog.jamesdbloom.com/JVMInternals.html |
| Crafting Interpreters                       | Free book        | craftinginterpreters.com               |

### Domain 4 — Databases

| Resource                                          | Type                 | URL / Where to find                     |
| ------------------------------------------------- | -------------------- | --------------------------------------- |
| PostgreSQL documentation                          | Official docs        | postgresql.org/docs/current             |
| Use The Index, Luke                               | Free book on indexes | use-the-index-luke.com                  |
| Designing Data-Intensive Applications (Kleppmann) | Book                 | dataintensive.net — **read this first** |
| Database Internals (Petrov)                       | Book                 | O'Reilly                                |
| CMU 15-445 Database Systems                       | Free course          | 15445.courses.cs.cmu.edu                |
| Redis documentation                               | Official docs        | redis.io/docs                           |
| Cassandra documentation                           | Official docs        | cassandra.apache.org/doc                |
| MongoDB documentation                             | Official docs        | mongodb.com/docs                        |
| Elasticsearch documentation                       | Official docs        | elastic.co/guide                        |

### Domain 5 — System Design & Distributed Systems

| Resource                                          | Type                    | URL / Where to find                                                                                                      |
| ------------------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Designing Data-Intensive Applications (Kleppmann) | Book                    | dataintensive.net                                                                                                        |
| System Design Interview Vol 1 & 2 (Alex Xu)       | Book                    | ByteByteGo                                                                                                               |
| ByteByteGo newsletter and blog                    | Blog                    | blog.bytebytego.com                                                                                                      |
| High Scalability blog                             | Real-world case studies | highscalability.com                                                                                                      |
| Raft consensus paper                              | Paper                   | raft.github.io                                                                                                           |
| Google Spanner paper                              | Paper                   | research.google/pubs/pub39966                                                                                            |
| Amazon Dynamo paper                               | Paper                   | dl.acm.org — "Dynamo: Amazon's Highly Available Key-value Store"                                                         |
| Google MapReduce paper                            | Paper                   | research.google/pubs/pub62                                                                                               |
| Apache Kafka documentation                        | Official docs           | kafka.apache.org/documentation                                                                                           |
| "The Log" by Jay Kreps                            | Article                 | engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying |
| Martin Fowler's bliki (patterns)                  | Blog                    | martinfowler.com                                                                                                         |

### Domain 6 — Software Architecture

| Resource                                                 | Type            | URL / Where to find                     |
| -------------------------------------------------------- | --------------- | --------------------------------------- |
| Clean Architecture (Robert C. Martin)                    | Book            | Prentice Hall                           |
| Domain-Driven Design (Eric Evans)                        | Book            | Addison-Wesley (canonical, dense)       |
| Implementing Domain-Driven Design (Vaughn Vernon)        | Book            | Addison-Wesley (more practical)         |
| A Philosophy of Software Design (Ousterhout)             | Book            | Stanford (short and excellent)          |
| Refactoring (Martin Fowler, 2nd ed.)                     | Book            | martinfowler.com/books/refactoring.html |
| Patterns of Enterprise Application Architecture (Fowler) | Book            | martinfowler.com/books/eaa.html         |
| Design Patterns (Gang of Four)                           | Book            | Addison-Wesley                          |
| microservices.io patterns                                | Pattern catalog | microservices.io/patterns               |

### Domain 7 — Frontend Engineering

| Resource                                    | Type               | URL / Where to find                |
| ------------------------------------------- | ------------------ | ---------------------------------- |
| MDN Web Docs                                | Official reference | developer.mozilla.org              |
| web.dev by Google                           | Performance guides | web.dev                            |
| React documentation                         | Official docs      | react.dev                          |
| V8 blog (JavaScript engine internals)       | Blog               | v8.dev/blog                        |
| JavaScript Visualized series (Lydia Hallie) | Articles           | lydiahallie.io                     |
| CSS-Tricks                                  | Reference          | css-tricks.com                     |
| Smashing Magazine                           | Frontend patterns  | smashingmagazine.com               |
| Chrome DevTools documentation               | Official docs      | developer.chrome.com/docs/devtools |

### Domain 8 — Backend Engineering

| Resource                     | Type  | URL / Where to find                           |
| ---------------------------- | ----- | --------------------------------------------- |
| OAuth 2.0 specification      | RFC   | rfc-editor.org/rfc/rfc6749                    |
| OpenID Connect specification | Spec  | openid.net/specs/openid-connect-core-1_0.html |
| JWT specification            | RFC   | rfc-editor.org/rfc/rfc7519                    |
| HTTP/1.1 specification       | RFC   | rfc-editor.org/rfc/rfc9110 (updated)          |
| HTTP/2 specification         | RFC   | rfc-editor.org/rfc/rfc9113                    |
| REST API Tutorial            | Guide | restfulapi.net                                |
| RFC 7807 Problem Details     | RFC   | rfc-editor.org/rfc/rfc7807                    |

### Domain 9 — Security

| Resource                                             | Type               | URL / Where to find                              |
| ---------------------------------------------------- | ------------------ | ------------------------------------------------ |
| OWASP Top 10                                         | Official guide     | owasp.org/www-project-top-ten                    |
| OWASP Testing Guide                                  | Free book          | owasp.org/www-project-web-security-testing-guide |
| Cryptography Engineering (Ferguson, Schneier, Kohno) | Book               | Wiley                                            |
| Have I Been Pwned                                    | Research tool      | haveibeenpwned.com                               |
| Portswigger Web Security Academy                     | Free labs          | portswigger.net/web-security                     |
| NIST Cryptographic Standards                         | Official standards | csrc.nist.gov                                    |

### Domain 10 — DevOps & Infrastructure

| Resource                          | Type          | URL / Where to find               |
| --------------------------------- | ------------- | --------------------------------- |
| Kubernetes documentation          | Official docs | kubernetes.io/docs                |
| Docker documentation              | Official docs | docs.docker.com                   |
| Terraform documentation           | Official docs | developer.hashicorp.com/terraform |
| AWS documentation                 | Official docs | docs.aws.amazon.com               |
| The Linux Command Line (Shotts)   | Free book     | linuxcommand.org/tlcl.php         |
| Linux Performance (Brendan Gregg) | Book          | brendangregg.com/linuxperf.html   |
| GitHub Actions documentation      | Official docs | docs.github.com/en/actions        |

### Domain 11–12 — Testing & Performance

| Resource                                     | Type           | URL / Where to find                                                |
| -------------------------------------------- | -------------- | ------------------------------------------------------------------ |
| Testing Trophy (Kent C. Dodds)               | Article        | kentcdodds.com/blog/the-testing-trophy-and-testing-classifications |
| Playwright documentation                     | Official docs  | playwright.dev                                                     |
| Pytest documentation                         | Official docs  | docs.pytest.org                                                    |
| Systems Performance (Brendan Gregg, 2nd ed.) | Book           | brendangregg.com/systems-performance-2nd-edition-book.html         |
| Flamegraph methodology                       | Article + tool | brendangregg.com/flamegraphs.html                                  |

### Domain 13 — Reliability

| Resource                                       | Type          | URL / Where to find                   |
| ---------------------------------------------- | ------------- | ------------------------------------- |
| Site Reliability Engineering (Google SRE Book) | Free book     | sre.google/sre-book/table-of-contents |
| The Site Reliability Workbook                  | Free book     | sre.google/workbook/table-of-contents |
| OpenTelemetry documentation                    | Official docs | opentelemetry.io/docs                 |
| Prometheus documentation                       | Official docs | prometheus.io/docs                    |

### Domain 14 — AI/ML Engineering

| Resource                                      | Type           | URL / Where to find                        |
| --------------------------------------------- | -------------- | ------------------------------------------ |
| Attention Is All You Need (Transformer paper) | Paper          | arxiv.org/abs/1706.03762                   |
| The Illustrated Transformer                   | Blog           | jalammar.github.io/illustrated-transformer |
| OpenAI Cookbook                               | Official guide | cookbook.openai.com                        |
| LangChain documentation                       | Official docs  | python.langchain.com/docs                  |
| MLflow documentation                          | Official docs  | mlflow.org/docs                            |

### Domain 15 — Engineering Leadership

| Resource                                 | Type     | URL / Where to find              |
| ---------------------------------------- | -------- | -------------------------------- |
| Staff Engineer (Will Larson)             | Book     | staffeng.com                     |
| An Elegant Puzzle (Will Larson)          | Book     | lethain.com/elegant-puzzle       |
| The Manager's Path (Camille Fournier)    | Book     | O'Reilly                         |
| DORA Research                            | Research | dora.dev                         |
| accelerate (book based on DORA research) | Book     | itrevolution.com/book/accelerate |

---

## Session Template

Use this structure for every learning session:

```
SESSION START
=============
1. Warm-up: What did you learn last time? (2 min)
2. Gap check: Anything still unclear from last time? (5 min)
3. New concept: [Topic] (15-20 min)
   - Hook → Intuition → Technical → Pros/Cons → Compare → Mistakes
4. Check understanding: 3 questions (5 min)
5. Practice: 1 coding or design exercise (20-30 min)
6. Review and feedback (10 min)
7. Session summary and next steps (5 min)
SESSION END
===========
```

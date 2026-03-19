# HLD Deep Dive — Complete System Design Learning Kit

## What Is This?

This repository is a complete system design learning kit built around forty-five long-form markdown files: twenty-five concept deep dives, fifteen end-to-end case studies, and five helper guides that tie the whole collection together. The goal is not to memorize buzzwords. The goal is to understand how real distributed systems behave under load, what tradeoffs engineers make, and how the same primitives show up again and again in different products.

The material is aimed at junior-to-mid-level engineers who already know basic backend ideas and want to build real depth. If you have ever read a system design answer that said "add Redis, Kafka, and sharding" without explaining why, this kit is the opposite of that style. Each concept file explains the problem, the mental model, the practical tradeoffs, and the common misconceptions. Each case study then shows how those ideas combine inside real systems such as URL shorteners, chat systems, streaming platforms, collaborative editors, and distributed schedulers.

The files were also written to work well with Google NotebookLM. That matters because NotebookLM does best when source material is explicit, narrative, and richly connected. These files are meant to support reading, audio overviews, quiz generation, mindmaps, and iterative study rather than one-time skimming.

## How This Kit Is Organized

The kit has three layers, and they are meant to reinforce each other instead of being studied in isolation.

- `concepts/` contains the twenty-five core building blocks. These explain scaling, load balancing, SQL and NoSQL choices, replication, caching, queues, consistency models, observability, architectural boundaries, search, and scheduling.
- `case-studies/` contains fifteen full system designs. Each case study opens with a `Concepts Covered` section, which makes it easy to trace where the underlying primitives actually appear in practice.
- `helpers/` contains the navigation and study guides. This README gives you the map. `SYSTEM-DESIGN-FRAMEWORK.md` gives you a reusable approach to whiteboard problems. `NUMBERS-AND-ESTIMATION.md` gives you reference numbers and formulas. `TRADEOFFS-DECISION-MATRIX.md` helps you choose between alternatives. `NOTEBOOKLM-STUDY-GUIDE.md` explains how to turn the whole collection into a stronger learning workflow.

The intended study pattern is simple: learn the primitive, see it appear in a system, then revisit it from a more advanced angle. For example, you might read Concept 10 on caching, then study CS-01 URL Shortener to see hot-key and redirect-path caching, then come back later to CS-15 Distributed Cache to understand how the cache itself becomes the system being designed.

## Recommended Learning Path

The ordering below follows the actual dependency structure of the repo. Start with request flow and system boundaries, move into storage and data placement, then learn communication, reliability, and specialized subsystems. The case studies are grouped as the instruction file requires, but even the "beginner" set is dense enough to reward careful reading.

### Phase 1: Foundations (Week 1-2)

- `01 Horizontal vs Vertical Scaling & Auto-scaling`: learn how systems grow, what resource ceilings look like, and why "just add servers" is only sometimes true.
- `02 Load Balancing Deep Dive`: understand how requests are distributed, how health checks and failover work, and where stateful workloads complicate scaling.
- `03 CDN & Edge Computing`: see how static and semi-dynamic content is pushed closer to users to cut latency and protect origins.
- `04 API Gateway, Reverse Proxy & Rate Limiting`: study the edge-control layer where authentication, routing, traffic shaping, and abuse protection often begin.
- `05 API Design Patterns`: define the contract first so the rest of the architecture has a clean interface to scale behind.

### Phase 2: Data Layer (Week 2-3)

- `06 SQL Databases at Scale`: understand where relational systems shine, how indexes really matter, and when replicas are the first scaling lever.
- `07 NoSQL Deep Dive`: learn the categories of NoSQL systems and why access patterns often matter more than entity purity.
- `08 Database Replication`: see how read scaling, failover, and lag shape real operational behavior.
- `09 Database Sharding & Partitioning`: understand when a single primary stops being enough and how partitioning changes query patterns.
- `10 Caching Strategies`: move hot reads into memory and learn the consistency and invalidation costs that come with that speed.
- `11 Consistent Hashing`: understand how keys move when cache or storage nodes change and why cluster resizing is not a trivial detail.
- `12 Data Modeling for Scale`: tie the whole data layer together by modeling from access patterns instead of abstract ER diagrams.

### Phase 3: Communication & Reliability (Week 3-4)

- `13 Synchronous vs Asynchronous Communication Patterns`: choose when requests should block and when work should be handed off.
- `14 Message Queues & Stream Processing`: understand the difference between task handoff and replayable event logs.
- `15 Event-Driven Architecture & Event Sourcing`: learn how systems react to facts over time rather than chaining synchronous calls.
- `16 Real-time Communication`: cover WebSockets, SSE, fanout, and presence-like problems where latency is visible to users.
- `17 CAP Theorem & PACELC`: learn how partitions, latency, and coordination shape global correctness decisions.
- `18 Distributed Consensus Simplified`: understand quorum, leadership, leases, and the boundaries of coordinated state.
- `19 Fault Tolerance Patterns`: study retries, circuit breakers, graceful degradation, bulkheads, and other resilience tools.
- `20 Idempotency, Deduplication & Exactly-Once Semantics`: internalize why duplicate work is normal and how systems defend themselves.
- `21 Monitoring, Observability & SLOs/SLAs`: learn how to measure whether the system is healthy from the user point of view, not just the host view.

### Phase 4: Architecture Patterns (Week 4-5)

- `22 Microservices vs Monolith`: understand coupling, deployment boundaries, and the real operational cost of service sprawl.
- `23 Blob/Object Storage Patterns`: separate metadata from large immutable bytes and learn the storage split that powers media-heavy products.
- `24 Search Systems`: understand inverted indexes, ranking pipelines, freshness, and why search is its own retrieval plane.
- `25 Distributed Task Scheduling & Workflow Orchestration`: learn how background work becomes durable, lease-based, observable, and replayable.

### Phase 5: Case Studies — Beginner (Week 5-6)

- `CS-01 URL Shortener`: a clean first design for API shape, storage, caching, and redirect traffic.
- `CS-02 Pastebin / Code Sharing Service`: adds object storage, CDN behavior, and read-heavy content serving.
- `CS-03 Instagram News Feed`: introduces fanout, ranking, event-driven ingestion, and hot-feed caching.
- `CS-04 Twitter Search`: shows ingestion pipelines, sharding, indexing, and retrieval tradeoffs.
- `CS-05 WhatsApp Chat System`: brings in delivery guarantees, real-time communication, and idempotent messaging behavior.

### Phase 6: Case Studies — Intermediate (Week 6-7)

- `CS-06 YouTube / Netflix Video Streaming`: combines object storage, CDN, metadata APIs, and high-volume delivery paths.
- `CS-07 Notification System`: shows async orchestration, scheduling, multi-channel delivery, and operator visibility.
- `CS-08 Uber / Ride Matching`: adds geospatial-style modeling, live updates, and event-driven marketplace coordination.
- `CS-09 Typeahead / Autocomplete`: sharpens your understanding of search-oriented serving and low-latency hot-path design.
- `CS-10 Distributed Rate Limiter`: focuses on policy distribution, consistency boundaries, and fast edge decisions.

### Phase 7: Case Studies — Advanced (Week 7-8)

- `CS-11 Google Docs / Collaborative Editing`: concurrency, real-time collaboration, coordination, and correctness under edits.
- `CS-12 Distributed Job Scheduler`: durable timers, leases, retries, workflows, and overdue-task handling.
- `CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop`: inventory protection, admission control, queues, and overload survival.
- `CS-14 Metrics & Logging System`: ingestion, storage tiering, sharding, search, and observability as a product.
- `CS-15 Distributed Cache (Redis-Like)`: partitioning, replication, failover, and memory-heavy infrastructure design.

## Concept-to-Case-Study Mapping

The table below is derived from the actual `## Concepts Covered` sections inside the case-study files in this repo, not from the example rows in the instruction file.

| Concept | Case Studies Where It Appears |
|---------|-------------------------------|
| 01 - Horizontal vs Vertical Scaling & Auto-scaling | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete; CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System; CS-15 Distributed Cache (Redis-Like) |
| 02 - Load Balancing Deep Dive | CS-01 URL Shortener; CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-08 Uber / Ride Matching; CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System; CS-15 Distributed Cache (Redis-Like) |
| 03 - CDN & Edge Computing | CS-02 Pastebin / Code Sharing Service; CS-06 YouTube / Netflix Video Streaming |
| 04 - API Gateway, Reverse Proxy & Rate Limiting | CS-07 Notification System; CS-10 Distributed Rate Limiter; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop |
| 05 - API Design Patterns | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-04 Twitter Search; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler |
| 06 - SQL Databases at Scale | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop |
| 07 - NoSQL Deep Dive | CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-08 Uber / Ride Matching; CS-14 Metrics & Logging System |
| 08 - Database Replication | CS-15 Distributed Cache (Redis-Like) |
| 09 - Database Sharding & Partitioning | CS-04 Twitter Search; CS-14 Metrics & Logging System |
| 10 - Caching Strategies | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-03 Instagram News Feed; CS-06 YouTube / Netflix Video Streaming; CS-09 Typeahead / Autocomplete; CS-10 Distributed Rate Limiter; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-15 Distributed Cache (Redis-Like) |
| 11 - Consistent Hashing | CS-01 URL Shortener; CS-10 Distributed Rate Limiter; CS-15 Distributed Cache (Redis-Like) |
| 12 - Data Modeling for Scale | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete |
| 13 - Synchronous vs Asynchronous Communication Patterns | CS-03 Instagram News Feed; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System |
| 14 - Message Queues & Stream Processing | CS-01 URL Shortener; CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System |
| 15 - Event-Driven Architecture & Event Sourcing | CS-03 Instagram News Feed; CS-04 Twitter Search; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System |
| 16 - Real-time Communication | CS-03 Instagram News Feed; CS-05 WhatsApp Chat System; CS-08 Uber / Ride Matching; CS-11 Google Docs / Collaborative Editing |
| 17 - CAP Theorem & PACELC | CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System; CS-15 Distributed Cache (Redis-Like) |
| 18 - Distributed Consensus Simplified | CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-15 Distributed Cache (Redis-Like) |
| 19 - Fault Tolerance Patterns | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System; CS-15 Distributed Cache (Redis-Like) |
| 20 - Idempotency, Deduplication & Exactly-Once Semantics | CS-05 WhatsApp Chat System; CS-07 Notification System; CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-15 Distributed Cache (Redis-Like) |
| 21 - Monitoring, Observability & SLOs/SLAs | CS-01 URL Shortener; CS-02 Pastebin / Code Sharing Service; CS-03 Instagram News Feed; CS-04 Twitter Search; CS-05 WhatsApp Chat System; CS-06 YouTube / Netflix Video Streaming; CS-07 Notification System; CS-08 Uber / Ride Matching; CS-09 Typeahead / Autocomplete; CS-10 Distributed Rate Limiter; CS-11 Google Docs / Collaborative Editing; CS-12 Distributed Job Scheduler; CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop; CS-14 Metrics & Logging System; CS-15 Distributed Cache (Redis-Like) |
| 22 - Microservices vs Monolith | CS-03 Instagram News Feed; CS-07 Notification System |
| 23 - Blob/Object Storage Patterns | CS-02 Pastebin / Code Sharing Service; CS-03 Instagram News Feed; CS-06 YouTube / Netflix Video Streaming |
| 24 - Search Systems | CS-04 Twitter Search; CS-09 Typeahead / Autocomplete; CS-14 Metrics & Logging System |
| 25 - Distributed Task Scheduling & Workflow Orchestration | CS-07 Notification System; CS-12 Distributed Job Scheduler |

## Case Study Difficulty & Prerequisite Concepts

The difficulty labels below follow the intended curriculum sequence from the helper specification. The prerequisite lists are based on the actual content and concept coverage of each case study rather than generic interview folklore.

| Case Study | Difficulty | Read These Concepts First |
|------------|------------|--------------------------|
| CS-01 URL Shortener | Beginner | 05 API Design Patterns; 06 SQL Databases at Scale; 10 Caching Strategies; 11 Consistent Hashing |
| CS-02 Pastebin / Code Sharing Service | Beginner | 03 CDN & Edge Computing; 05 API Design Patterns; 06 SQL Databases at Scale; 23 Blob/Object Storage Patterns |
| CS-03 Instagram News Feed | Beginner | 07 NoSQL Deep Dive; 10 Caching Strategies; 12 Data Modeling for Scale; 15 Event-Driven Architecture & Event Sourcing |
| CS-04 Twitter Search | Beginner | 07 NoSQL Deep Dive; 09 Database Sharding & Partitioning; 14 Message Queues & Stream Processing; 24 Search Systems |
| CS-05 WhatsApp Chat System | Beginner | 07 NoSQL Deep Dive; 13 Synchronous vs Asynchronous Communication Patterns; 16 Real-time Communication; 20 Idempotency, Deduplication & Exactly-Once Semantics |
| CS-06 YouTube / Netflix Video Streaming | Intermediate | 03 CDN & Edge Computing; 10 Caching Strategies; 19 Fault Tolerance Patterns; 23 Blob/Object Storage Patterns |
| CS-07 Notification System | Intermediate | 04 API Gateway, Reverse Proxy & Rate Limiting; 13 Synchronous vs Asynchronous Communication Patterns; 14 Message Queues & Stream Processing; 25 Distributed Task Scheduling & Workflow Orchestration |
| CS-08 Uber / Ride Matching | Intermediate | 07 NoSQL Deep Dive; 12 Data Modeling for Scale; 15 Event-Driven Architecture & Event Sourcing; 16 Real-time Communication |
| CS-09 Typeahead / Autocomplete | Intermediate | 10 Caching Strategies; 12 Data Modeling for Scale; 14 Message Queues & Stream Processing; 24 Search Systems |
| CS-10 Distributed Rate Limiter | Intermediate | 04 API Gateway, Reverse Proxy & Rate Limiting; 10 Caching Strategies; 11 Consistent Hashing; 17 CAP Theorem & PACELC |
| CS-11 Google Docs / Collaborative Editing | Advanced | 16 Real-time Communication; 17 CAP Theorem & PACELC; 18 Distributed Consensus Simplified; 20 Idempotency, Deduplication & Exactly-Once Semantics |
| CS-12 Distributed Job Scheduler | Advanced | 14 Message Queues & Stream Processing; 15 Event-Driven Architecture & Event Sourcing; 18 Distributed Consensus Simplified; 25 Distributed Task Scheduling & Workflow Orchestration |
| CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop | Advanced | 04 API Gateway, Reverse Proxy & Rate Limiting; 06 SQL Databases at Scale; 10 Caching Strategies; 20 Idempotency, Deduplication & Exactly-Once Semantics |
| CS-14 Metrics & Logging System | Advanced | 09 Database Sharding & Partitioning; 14 Message Queues & Stream Processing; 21 Monitoring, Observability & SLOs/SLAs; 24 Search Systems |
| CS-15 Distributed Cache (Redis-Like) | Advanced | 08 Database Replication; 10 Caching Strategies; 11 Consistent Hashing; 18 Distributed Consensus Simplified |

## How to Use with NotebookLM

Use this README as the navigation spine for the whole collection. Upload it alongside the concept and case-study files so NotebookLM can see the learning order, the concept-to-case-study relationships, and the prerequisite structure. For the full workflow, prompts, and suggested notebook layouts, read [`NOTEBOOKLM-STUDY-GUIDE.md`](./NOTEBOOKLM-STUDY-GUIDE.md).

## How to Use with Claude Code

Claude Code works well as an interactive study partner after you finish reading or listening to a topic. Good uses include:

- asking it to quiz you on one phase of the curriculum;
- having it compare two case studies using the tradeoffs in [`TRADEOFFS-DECISION-MATRIX.md`](./TRADEOFFS-DECISION-MATRIX.md);
- requesting a back-of-envelope estimation drill using [`NUMBERS-AND-ESTIMATION.md`](./NUMBERS-AND-ESTIMATION.md);
- using [`SYSTEM-DESIGN-FRAMEWORK.md`](./SYSTEM-DESIGN-FRAMEWORK.md) as the structure for mock interview practice;
- asking for a custom case study after you complete the core fifteen.

The highest-value loop is simple: read one concept, study one case study that uses it, then explain the design back in your own words with Claude Code pushing on the weak spots.

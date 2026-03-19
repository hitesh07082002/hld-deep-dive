# System Design Tradeoffs Decision Matrix

> For every major design decision, this document explains the options, when to choose each, and what you're giving up.

## How to Use This Document

This is a decision reference, not a rulebook. Good system design is rarely about finding the universally best technology. It is about choosing the least wrong tradeoff for a specific workload, team, and product requirement. When you face a design choice, find the relevant section, identify which requirement is dominant, and then decide what cost you are willing to pay in exchange.

One recurring pattern across this repo is that the boring default is often right until the workload proves otherwise. The fastest way to make a system worse is to import complexity from a larger company without importing the scale or failure mode that justified it. Use these matrices to understand the price of each choice, not just its marketing upside.

## Data Storage Tradeoffs

### SQL vs NoSQL

**Choose SQL when:**

- correctness, transactions, or relational integrity matter more than unconstrained horizontal scale;
- the access pattern needs secondary indexes, joins, or ad hoc reporting;
- the team benefits from strong schema discipline and mature tooling.

**Choose NoSQL when:**

- access patterns are simple but large-scale, such as key-value lookups, append-heavy writes, or per-entity denormalized reads;
- schema flexibility or partition-first design matters more than relational consistency;
- the workload is naturally modeled as document, key-value, wide-column, or graph data.

**What you give up with SQL:** easier horizontal write scaling, flexible denormalization, and sometimes cheap per-entity access at massive scale.

**What you give up with NoSQL:** transactional guarantees, relational querying power, and a lot of debugging convenience.

**Real-world signal:** if your core request path needs multi-row correctness, SQL is usually the default. If your core request path is "lookup by primary key at very high scale," NoSQL starts to earn its keep.

Cross-reference: Concept 06, Concept 07

### SQL Database Selection: PostgreSQL vs MySQL vs Others

**Choose PostgreSQL when:**

- you want a rich feature set, strong extensions, and complex query support;
- the workload values flexibility in indexes, JSON support, or analytical helpers.

**Choose MySQL when:**

- you want a simpler operational baseline and your workload is mostly conventional OLTP;
- your ecosystem or managed platform is already optimized around it.

**What you give up with PostgreSQL:** sometimes a little operational simplicity.

**What you give up with MySQL:** some advanced feature richness and ergonomic query power.

**Real-world signal:** for new greenfield systems, PostgreSQL is often the boring default unless the org already has deep MySQL tooling or managed patterns.

Cross-reference: Concept 06

### NoSQL Category Selection: Document vs Key-Value vs Column-Family vs Graph

**Choose document stores when** you want flexible nested records and entity-oriented retrieval.

**Choose key-value stores when** your access path is overwhelmingly primary-key lookups and latency is the first concern.

**Choose column-family stores when** you need large-scale write throughput, partitioned time-series or event-style access, and tunable consistency.

**Choose graph stores when** the primary value is traversing relationships rather than fetching one record by ID.

**What you give up:** specialization. The more a store is optimized for one access style, the worse it tends to be at the others.

**Real-world signal:** if your answer is "we might need all of those query styles," that usually means you need multiple storage systems with clear ownership, not one magical database.

Cross-reference: Concept 07, Concept 12

### Normalization vs Denormalization

**Choose normalization when:**

- correctness and update simplicity matter most;
- data duplication would create expensive consistency bugs.

**Choose denormalization when:**

- reads dominate and the same composed view is served repeatedly;
- the application can tolerate rebuilding or asynchronously maintaining duplicate views.

**What you give up with normalization:** read speed, especially when many joins or cross-entity aggregations are needed on the hot path.

**What you give up with denormalization:** easy writes, simple updates, and single-place truth.

**Real-world signal:** if the same read path keeps joining five tables in production, the relational model may be correct logically but wrong operationally.

Cross-reference: Concept 06, Concept 12

## Caching Tradeoffs

### Cache-Aside vs Write-Through vs Write-Back vs Write-Around

**Choose cache-aside when** you want the boring default: read miss goes to DB, then populates cache; writes update DB and invalidate cache.

**Choose write-through when** reads must see fresh cached values quickly and write latency can absorb the extra synchronous step.

**Choose write-back when** write speed matters more than immediate durability and the workload tolerates buffered persistence.

**Choose write-around when** writes are frequent but many written objects are never read soon afterward.

**What you give up:** there is no free option. Cache-aside gives a small inconsistency window. Write-through adds write latency. Write-back risks data loss. Write-around sacrifices first-read latency after writes.

**Real-world signal:** if you cannot clearly explain the consistency model after a write, you probably have not chosen the pattern deliberately enough.

Cross-reference: Concept 10

### Redis vs Memcached

**Choose Redis when:**

- you need richer data structures, replication, persistence options, pub/sub, or scripting;
- the cache also acts as a lightweight coordination layer.

**Choose Memcached when:**

- you want a simpler pure-cache tier with low overhead and no need for richer semantics.

**What you give up with Redis:** more operational surface area and the temptation to turn your cache into an accidental platform.

**What you give up with Memcached:** richer primitives and some high-value operational features.

**Real-world signal:** if the team keeps saying "we also need sorted sets, streams, or leaderboards," you already chose Redis whether you admitted it or not.

Cross-reference: Concept 10

### Where to Cache: Client, CDN, App Server, Database

**Choose client caching when** the user can safely reuse local assets or responses and stale data is acceptable within well-defined bounds.

**Choose CDN caching when** global latency and origin protection matter, especially for static or semi-static content.

**Choose application caching when** repeated reads hammer the same hot data.

**Choose database-page caching when** you mostly need warm indexes and buffers rather than an explicit extra tier.

**What you give up:** invalidation gets harder the farther the cache is from the source of truth.

**Real-world signal:** do not start with Redis if your problem is really static bytes that belong on a CDN.

Cross-reference: Concept 03, Concept 10, Concept 23

## Communication Tradeoffs

### Synchronous vs Asynchronous

**Choose synchronous when:**

- the caller needs the answer before it can continue;
- the operation must appear immediate to the user;
- the dependency is reliable enough to live on the request path.

**Choose asynchronous when:**

- the work can happen later;
- bursts need smoothing;
- one failure should not instantly fail the whole upstream request.

**What you give up with synchronous:** resilience to slow or unavailable downstreams.

**What you give up with asynchronous:** simplicity, immediate visibility, and often easier debugging.

**Real-world signal:** if a user-facing API is waiting on analytics, email, and recommendation side effects, that path is begging to be split.

Cross-reference: Concept 13, Concept 14

### REST vs gRPC vs GraphQL

**Choose REST when** you want resource-oriented APIs, easy debugging, and broad interoperability.

**Choose gRPC when** internal RPC performance, strict contracts, and generated clients matter.

**Choose GraphQL when** clients truly need flexible graph-shaped reads and can benefit from field selection.

**What you give up with REST:** some efficiency for chatty internal service graphs.

**What you give up with gRPC:** easy browser-native debugging and some public API ergonomics.

**What you give up with GraphQL:** operational simplicity, caching ease, and query-governance clarity.

**Real-world signal:** GraphQL is most useful when it solves client data-shape pain, not when it is used as a fashionable wrapper around one endpoint.

Cross-reference: Concept 05

### Push vs Pull

**Choose push when** freshness matters and the producer knows which consumers need updates.

**Choose pull when** the consumer can tolerate delay and independent polling simplifies the system.

**What you give up with push:** stateful delivery infrastructure and retry complexity.

**What you give up with pull:** freshness and sometimes efficiency.

**Real-world signal:** notifications, chat, and collaborative editing lean push; periodic reports and low-urgency status checks often lean pull.

Cross-reference: Concept 13, Concept 16

### WebSocket vs SSE vs Long Polling

**Choose WebSocket when** you need full-duplex, high-frequency, low-latency communication.

**Choose SSE when** updates flow mostly server to client and browser simplicity matters.

**Choose long polling when** infrastructure constraints block persistent connections and traffic volume is moderate.

**What you give up with WebSocket:** more stateful connection management and harder scaling.

**What you give up with SSE:** client-to-server flexibility.

**What you give up with long polling:** efficiency and latency.

**Real-world signal:** collaborative editing and chat usually justify WebSockets. Live dashboards often fit SSE well.

Cross-reference: Concept 16

## Consistency Tradeoffs

### Strong vs Eventual Consistency

**Choose strong consistency when:**

- stale reads create financial, inventory, or workflow correctness problems;
- the business would rather reject or delay than return conflicting truth.

**Choose eventual consistency when:**

- temporary divergence is acceptable;
- availability and latency matter more than immediate global agreement.

**What you give up with strong consistency:** latency and sometimes availability under partition.

**What you give up with eventual consistency:** predictability from the user point of view.

**Real-world signal:** ledgers, quotas, and inventory lean strong. Feeds, counters, and recommendation views often lean eventual.

Cross-reference: Concept 17

### Optimistic vs Pessimistic Locking

**Choose optimistic locking when** conflicts are rare and retrying is acceptable.

**Choose pessimistic locking when** conflicts are common and the cost of conflicting writes is high.

**What you give up with optimistic locking:** occasional retry pain and more conflict handling in the application.

**What you give up with pessimistic locking:** throughput and latency under contention.

**Real-world signal:** collaborative edits and low-conflict entity updates often benefit from optimistic control. Scarce-seat booking and high-contention inventory can justify stronger locking or reservation semantics.

Cross-reference: Concept 17, Concept 20

### Read-Your-Writes vs Full Consistency

**Choose read-your-writes when** the user mainly needs their own most recent action reflected quickly.

**Choose full consistency when** every client must observe the same latest truth.

**What you give up with read-your-writes:** a globally uniform experience.

**What you give up with full consistency:** extra coordination cost for cases that may not need it.

**Real-world signal:** profile edits often only need read-your-writes; a global counter tied to billing may need stronger guarantees.

Cross-reference: Concept 17

## Scaling Tradeoffs

### Horizontal vs Vertical Scaling

**Choose vertical scaling when** the system is still small enough that a bigger box is the fastest, safest move.

**Choose horizontal scaling when** one machine's CPU, memory, disk, or connection limits become structural ceilings.

**What you give up with vertical scaling:** a hard upper bound and sometimes a bigger blast radius.

**What you give up with horizontal scaling:** operational simplicity.

**Real-world signal:** stateless services usually horizontal-scale earlier than databases. That is why caches, queues, and storage tiers need special thought.

Cross-reference: Concept 01

### Replication vs Sharding

**Choose replication when** reads dominate and you need higher availability or geographic copies.

**Choose sharding when** write volume, total dataset size, or single-primary limits become the real problem.

**What you give up with replication:** write scalability and sometimes read freshness.

**What you give up with sharding:** query simplicity and rebalance ease.

**Real-world signal:** if one primary is fine for writes but struggling on reads, replicate first. If one primary cannot keep up with writes, sharding is coming.

Cross-reference: Concept 08, Concept 09

### Monolith vs Microservices

**Choose a monolith when** the product and team are still small enough that one codebase and deployment unit increase speed and reduce coordination cost.

**Choose microservices when** separate scaling, ownership boundaries, or fault isolation clearly outweigh the operational burden.

**What you give up with a monolith:** independent deployment and scaling granularity.

**What you give up with microservices:** observability simplicity, local reasoning, and a lot of engineering time.

**Real-world signal:** if teams are still tripping over unclear domain boundaries, splitting into services early usually spreads the confusion instead of fixing it.

Cross-reference: Concept 22

## Architecture Pattern Tradeoffs

### Fanout-on-Write vs Fanout-on-Read

**Choose fanout-on-write when** reads must be extremely fast and most producers have manageable fanout.

**Choose fanout-on-read when** write amplification would be too expensive, especially for celebrity or mega-follower accounts.

**What you give up with fanout-on-write:** expensive writes and painful hot-account behavior.

**What you give up with fanout-on-read:** heavier read-time computation and more latency pressure.

**Real-world signal:** many feed systems end up hybrid because the extreme ends of the graph want different strategies.

Cross-reference: Concept 10, Concept 14, Concept 15

### Precomputation vs On-the-Fly Computation

**Choose precomputation when** the same answer is served repeatedly and freshness can lag a little.

**Choose on-the-fly computation when** the answer changes too often, the query space is too large, or per-request personalization is dominant.

**What you give up with precomputation:** write-time cost, storage duplication, and invalidation complexity.

**What you give up with on-the-fly computation:** read latency and repeated work.

**Real-world signal:** recommendation candidates are often precomputed; final reranking is often done on the fly.

Cross-reference: Concept 10, Concept 12

### Batch Processing vs Stream Processing

**Choose batch when** latency can be minutes or hours and simplicity matters.

**Choose stream processing when** freshness, incremental updates, or continuous reaction matter.

**What you give up with batch:** timeliness.

**What you give up with streaming:** operational simplicity, debugging ease, and often cost.

**Real-world signal:** nightly reporting wants batch. fraud checks, metrics ingestion, and feed updates often want streaming.

Cross-reference: Concept 14, Concept 15

### Single Leader vs Multi-Leader vs Leaderless Replication

**Choose single leader when** correctness and write ordering matter most and one write authority is acceptable.

**Choose multi-leader when** multiple regions or sites must accept writes independently and conflict resolution is manageable.

**Choose leaderless when** availability and partition tolerance matter, and the application can live with tunable consistency plus reconciliation.

**What you give up with single leader:** write availability during leader loss or remote-latency penalties for distant writers.

**What you give up with multi-leader:** conflict resolution simplicity.

**What you give up with leaderless:** easy mental models and strong guarantees by default.

**Real-world signal:** if the team cannot explain merge semantics clearly, multi-leader is probably a bad idea for that workload.

Cross-reference: Concept 08, Concept 17, Concept 18

## The Meta-Tradeoff: Simplicity vs Optimization

This is the tradeoff that sits above all the others. Most teams do not fail because they picked the wrong consensus algorithm. They fail because they built too much system before the product or workload justified it. Every optimization has a carrying cost: more components, more dashboards, more failure modes, more documentation, more on-call burden, and more ways for new engineers to get lost.

That does not mean "always stay simple." It means earn complexity with evidence. Add Redis because the database hot path is actually overloaded. Add sharding because the primary cannot keep up with writes. Add stream processing because freshness requirements demand it. Add microservices because scaling and ownership boundaries clearly need them.

The strongest system designers sound calm because they know this. They start with the boring architecture, name the first bottleneck honestly, and add just enough machinery to solve that bottleneck. Then they stop. That restraint is not lack of imagination. It is engineering judgment.

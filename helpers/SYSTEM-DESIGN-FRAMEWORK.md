# The System Design Framework

> A step-by-step method for approaching any high-level design problem, from a blank whiteboard to a complete architecture.

## Why You Need a Framework

Most engineers do not struggle in system design because they know nothing. They struggle because they know too many disconnected things and do not know what to do first. One minute they are thinking about Kafka partitions, the next they are debating SQL versus NoSQL, and they still have not clarified whether the product even needs strong consistency or whether the first page has to load in under 200 milliseconds. Without a framework, system design discussions turn into box drawing plus buzzword improvisation.

A framework fixes that. It gives you an order of operations. That order matters because the right architecture for a system serving `10,000` users is different from the right architecture for one serving `100 million`, and both are different again if the business requirement is "never lose an order" rather than "show a fresh-enough feed." Requirements shape estimates. Estimates shape interfaces. Interfaces shape data models. Data models shape architecture. Architecture shapes the deep dives worth discussing.

The point of a framework is not to make every answer look the same. The point is to make your thinking reliable under pressure. Whether you are in an interview, doing a design review, or sketching a production proposal, a good framework keeps you from over-engineering early, missing obvious bottlenecks, or forgetting to talk about failure handling and observability until the last thirty seconds.

## The 8-Step Framework

### Step 1: Clarify Requirements (3-5 minutes)

Do not start with technology. Start with the product. A good design conversation begins by making the problem smaller and more precise. You want to know what the system must do, what success looks like, and which tradeoffs matter most.

Break the conversation into four buckets:

- **Functional requirements**: what users or other systems can do. Create a post, fetch a feed, send a message, run a scheduled job, search an index.
- **Non-functional requirements**: the success constraints. Latency, availability, durability, consistency, throughput, regional footprint, cost sensitivity.
- **Scale assumptions**: users, requests per second, storage growth, concurrency, read/write mix, object sizes.
- **Scope boundaries**: what is explicitly out of scope so you do not accidentally design five products at once.

Useful clarifying questions depend on the system:

- For read-heavy systems: what is the target p99 latency for the first page or first response?
- For write-heavy systems: what happens if a write succeeds in one place and fails downstream?
- For global systems: do users need cross-region freshness, or is local correctness enough?
- For real-time systems: is sub-second delivery required, or is "within a few seconds" acceptable?
- For operational systems: is at-least-once acceptable, or is duplicate side effect unacceptable?

What good sounds like:

> "Let me make sure I understand the problem. We are building a notification system that accepts events from other services, fans them out across email, SMS, and push, handles retries safely, and keeps delivery latency low enough that user-facing alerts arrive within a few seconds. We care most about durability, channel isolation, and operator visibility. We are not designing the full template editor or analytics UI."

The common trap here is jumping straight to solutions. If you start by saying "we will use Kafka and Redis" before agreeing on requirements, you are likely solving the wrong problem very efficiently.

### Step 2: Back-of-Envelope Estimation (3-5 minutes)

Once the problem is scoped, quantify it. Estimation is not busywork. It is how you figure out which parts of the system are boring and which parts are dangerous. The goal is rough order-of-magnitude clarity, not perfect math.

A reliable sequence is:

- Start with users or producers.
- Multiply by actions per user per day to get daily volume.
- Divide by `86,400` for average QPS.
- Apply a `2x` to `3x` peak multiplier unless the domain obviously needs more.
- Estimate storage from record size times daily volume times retention.
- Estimate bandwidth from payload size times QPS.
- Estimate cache memory from hot-data percentage times total hot footprint.

For reference numbers, use [`NUMBERS-AND-ESTIMATION.md`](./NUMBERS-AND-ESTIMATION.md). The purpose is not to show off memorized nanoseconds. The purpose is to reason about bottlenecks early enough that the architecture makes sense.

Here is a worked example for a generic social media app:

Assumptions:

- `20M` daily active users
- `10` feed opens per user per day
- `2` posts per user per day
- `3x` peak multiplier
- first-page feed response around `20 KB`
- post metadata around `1 KB`

Feed reads:

```text
20,000,000 x 10 = 200,000,000 feed reads/day
200,000,000 / 86,400 = 2,314.81 reads/sec average
Peak read QPS = 2,314.81 x 3 = 6,944.44
```

Post writes:

```text
20,000,000 x 2 = 40,000,000 posts/day
40,000,000 / 86,400 = 462.96 posts/sec average
Peak write QPS = 462.96 x 3 = 1,388.89
```

Bandwidth for first-page reads:

```text
6,944.44 x 20 KB = 138,888.8 KB/sec
= 135.63 MB/sec
```

Post metadata storage:

```text
40,000,000 x 1 KB = 40 GB/day
40 GB/day x 365 = 14.6 TB/year
```

That already tells you several things. The system is read-heavy. The feed response path is latency-sensitive. Media bytes should not go through the same service that serves feed metadata. And if freshness matters, asynchronous ingestion and precomputation are likely more important than optimizing the raw post-write API.

The common trap is over-precision. If you spend ten minutes arguing whether the average caption is `180` or `220` bytes, you are missing the point. Estimation is for deciding architecture, not closing the books for quarter-end finance.

### Step 3: Define the API (3-5 minutes)

Before drawing internals, define what the system exposes. APIs force clarity. They tell you what operations matter, which inputs are required, what shape the outputs take, and which calls are on the hot path.

Most design problems only need `3` to `6` core operations. For each one, define:

- HTTP or RPC method
- endpoint or procedure name
- required parameters
- important optional parameters
- response shape
- constraints such as pagination, idempotency keys, or auth

Think in terms of the product's natural verbs. In a URL shortener, the core operations are create short URL and resolve short URL. In a scheduler, they are create schedule, lease runnable work, complete task, and fail task with retry metadata. In a collaborative editor, they are connect session, fetch document state, submit edit, and receive live updates.

This is also where you decide whether REST, gRPC, or GraphQL best matches the access pattern:

- **REST** is usually the boring default for public or resource-oriented APIs.
- **gRPC** is a strong internal choice for low-latency service-to-service communication with strict schemas.
- **GraphQL** helps when clients need flexible field selection across many related objects, but it adds operational and caching complexity.

Do not forget the boring details that become painful later:

- pagination model: cursor versus offset;
- rate-limiting surface: per user, per client, per API key;
- authentication model: session, JWT, service identity;
- idempotency for retries on write endpoints;
- error semantics: what is retryable and what is terminal.

The common trap is overcomplicating the API with edge cases too early. You do not need to expose fourteen optional filters before you know the core path is right.

### Step 4: Design the Data Model (5 minutes)

Now decide how the system will store truth. The correct starting point is not "we use Postgres" or "we use DynamoDB." The correct starting point is: what are the most important reads and writes, and what storage model makes those operations easy, fast, and correct?

Work backwards from access patterns:

- What is the primary key?
- What is the most common lookup?
- Do you read one entity at a time, or whole time windows, or per-user lists?
- Do you need multi-row transactions?
- Is the write path append-heavy, update-heavy, or conflict-heavy?
- What indexes are unavoidable?

Then pick storage deliberately:

- **SQL** when correctness, transactions, secondary indexes, or relational integrity dominate.
- **NoSQL** when scale, flexible schemas, write throughput, or access-pattern-specific denormalization matter more.
- **Object storage** for large immutable blobs such as photos, video, logs, backups, or file payloads.
- **Search indexes** when retrieval is relevance-driven rather than primary-key-driven.

Define tables or collections with real fields. Naming the fields matters because it forces you to think about ownership and lookup shape. For example, a home-feed design may have `post_id`, `author_id`, `created_at`, `ranking_score`, and a separate timeline edge model keyed by user. A scheduler may have `schedule_id`, `next_run_at`, `task_id`, `lease_expiry`, and `attempt_no`. Once the fields are concrete, the index choices become easier.

The common trap is defaulting to a technology choice without connecting it to access patterns. "Use NoSQL because social apps are big" is not reasoning. "Use a denormalized per-user timeline store because the hottest read is recent items for one user" is reasoning.

### Step 5: Draw the High-Level Architecture (5-8 minutes)

Only now is it time to draw boxes. Start from the client or producer and work inward. The architecture should tell a story: request enters here, gets routed here, reads or writes this state, hands off this side effect, and is observed here.

The safest baseline is often:

`Client -> Edge / Load Balancer -> App Layer -> Cache -> Database`

Then add components only when there is a clear reason:

- **CDN** for global static delivery or origin shielding
- **API gateway** for routing, auth, and rate limiting at the edge
- **message queue** for background jobs or side-effect handoff
- **stream** for replayable ordered event flow
- **search index** for text retrieval
- **blob/object storage** for large files
- **scheduler / workflow engine** for time-based or dependency-based background work

A useful discipline is to separate the **read path** and the **write path**. Most systems are asymmetrical. Reads may want caching, denormalized views, and low-latency fanout. Writes may want durability, validation, queues, and asynchronous side effects. If you draw one generic blob called "service" without distinguishing those paths, you miss where the real tradeoffs live.

Also ask, for every box: why does it exist? If your answer is "because modern architectures use one," remove it. If the system only needs `500` QPS, a single relational database plus an application cache may be the best design in the room. Boring is a feature.

The common trap is drawing twenty boxes before understanding if you need them. Extra components are not free. They add failure modes, operational cost, and more things to explain.

### Step 6: Deep Dive into Key Components (8-10 minutes)

This is where you prove the design is not superficial. Do not deep dive everything. Pick the `2` or `3` components where correctness, scale, or algorithmic detail really matters.

Good deep-dive candidates are:

- the ranking or feed fanout strategy in a feed system;
- deduplication and retry semantics in a notification or payment system;
- partitioning and rebalancing in a cache or search cluster;
- concurrency control in collaborative editing;
- lease ownership and overdue recovery in a scheduler.

For each deep dive, explain four things:

1. **the mechanism**: what algorithm, protocol, or data structure is used;
2. **the happy path**: how it behaves when the system is healthy;
3. **the failure path**: what happens on retries, duplicates, or partial outages;
4. **the tradeoff**: what you gain and what complexity you introduce.

For example, if you deep dive fanout-on-write versus fanout-on-read in a news feed, explain why celebrity accounts break naive precomputation, why normal accounts can still be precomputed safely, and how a hybrid strategy preserves low read latency without making one write explode into millions of synchronous updates.

This is also where you should explicitly mention alternatives you considered. Not five pages of them, just enough to show judgment. "We could use multi-leader replication here, but because correctness on inventory counts matters more than local write availability, we prefer a leader-based or quorum-backed design."

The common trap is going too deep on one component and running out of time to talk about the rest of the system. Depth should clarify the design, not consume it.

### Step 7: Address Bottlenecks & Scaling (3-5 minutes)

Now pressure-test the system. Ask what happens at `10x` traffic and then at `100x`. Walk through the hot path and name the likely bottlenecks in order.

Look at each resource type:

- **CPU**: expensive ranking, serialization, compression, encryption, aggregation
- **memory**: large caches, connection counts, in-memory indexes, hot-key pressure
- **disk**: write amplification, compaction, logs, large scans
- **network**: cross-region replication, large payloads, media egress, queue backlogs
- **database**: single-primary write ceilings, index amplification, lock contention, hot partitions

For each bottleneck, propose a concrete response:

- caching or precomputation for repeated reads;
- replicas for read scaling;
- sharding for write or storage scaling;
- queues for spike absorption;
- CDN or object storage for heavy byte delivery;
- partition-aware key design to avoid hot shards;
- circuit breakers, rate limiting, or graceful degradation under overload.

This is also where you discuss failure modes. If Redis fails, do you stampede the database or degrade gracefully? If a worker crashes mid-task, how does the lease expire and the task recover? If one region partitions, do you choose consistency or local availability? Good scaling discussion includes both performance and resilience.

The common trap is saying "we can just add more servers." Stateless layers scale that way. Stateful layers often do not.

### Step 8: Monitoring & Wrap-up (2-3 minutes)

A good design does not end with "and then it works." It ends with "and here is how we know it is working." Mention the key metrics, the service objectives, and the operator signals that matter.

For most user-facing services, start with RED-style signals:

- request rate
- error rate
- latency percentiles

Then add system-specific metrics:

- cache hit ratio and hot-key skew
- replica lag
- queue depth and consumer lag
- lease expiry rate
- retry volume
- index freshness
- dropped notifications or failed deliveries

Define one or two SLOs that map to user experience. "99.9% of checkout requests succeed in under 500ms over a rolling 28-day window" is meaningful. "CPU below 70%" is not an SLO.

Finally, close with a quick recap:

> "We clarified that low-latency reads and durable writes matter most, estimated peak traffic, defined the core API, chose a relational source of truth plus Redis for hot reads, used a queue for asynchronous side effects, and deep-dived retries and cache invalidation because those are the highest-risk parts of correctness."

The common trap is skipping monitoring entirely. In real systems, operability is part of the architecture, not a slide added by the SRE team later.

## Framework Applied: Mini Example

Imagine the prompt is: design a simple key-value store.

Start by clarifying requirements. Do we only need `GET` and `PUT`, or do we need TTL, replication, and persistence? Suppose the scope is a single-region internal store with `GET`, `PUT`, `DELETE`, optional TTL, p99 reads under `5 ms`, and durability strong enough that acknowledged writes survive one node failure.

Then estimate. Maybe the system handles `50,000` operations per second average and `150,000` at peak, with a `4:1` read/write ratio and `1 KB` average values. That tells us memory is the likely hot-path resource, while persistence and replication shape write behavior.

Next define the API:

- `PUT /kv/{key}` with value and optional TTL
- `GET /kv/{key}`
- `DELETE /kv/{key}`

Then model the data: key, value, version, expiry time, and replication metadata. Because reads are dominant and access is by primary key, the natural storage shape is a partitioned key-value model rather than relational tables.

Now draw the architecture: client to load balancer, then stateless routers or storage nodes, then an in-memory engine with append-only persistence and replica nodes. If we need cluster resizing, consistent hashing becomes important. If we need failover, leader election or lease-based ownership matters.

For the deep dive, choose partitioning and failover. Explain how keys map to shards, how replicas trail the primary, what happens when the primary fails, and whether the system serves stale reads during failover.

For bottlenecks, call out hot keys, memory pressure, rebalancing overhead, and replica lag. For observability, measure read latency, write latency, ops per second, eviction rate, replication lag, and failover duration.

That is enough for a strong first-pass design. You did not need to invent five advanced features. You needed to follow the steps in order and spend depth where the system actually had risk.

## Common Anti-Patterns

- **Jumping to the architecture before clarifying the problem.** This is how people end up solving global multi-region consistency for a product that only needed one-region best effort.
- **Using buzzwords without naming the failure mode they solve.** "Use Kafka" is not an answer. "Use a queue so the write path does not block on notification delivery" is an answer.
- **Skipping estimation entirely.** If you do not size the system, you cannot tell whether a single database is fine or already doomed.
- **Choosing storage from habit instead of access pattern.** The question is not which database you like. The question is which reads and writes must be cheap and correct.
- **Over-engineering early.** Microservices, consensus, and multi-region active-active designs all have costs. Add complexity only when a real requirement earns it.
- **Under-engineering obvious hot paths.** If the system must serve millions of reads per second, pretending the primary database will handle everything is not pragmatism. It is avoidance.
- **Treating components as magic boxes.** If the cache "handles it," explain what gets cached, how invalidation works, and what happens when the cache is down.
- **Ignoring failure recovery.** Retries, duplicate delivery, leader failover, and partition behavior are normal, not edge cases.
- **Ending without observability.** A design that cannot measure user pain, backlog growth, or error-budget burn is incomplete even if the boxes look clean.

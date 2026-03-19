# CODEX INSTRUCTION: Generate 15 HLD Case Study Files

## Execution Strategy

You are strongly encouraged to use subagents for parallel file generation. Each file is self-contained and can be generated independently.

If using subagents:
- Use model: gpt-5.4-xhigh (do NOT downgrade to a cheaper or faster model — quality is the absolute priority, not speed or cost)
- Give each subagent: the full implementation plan + template + quality requirements from this file, PLUS the complete list of all 25 concept files AND all 15 case study titles (so cross-references use correct file numbers and names)
- Each subagent generates exactly 1 case study file (not multiple — case studies are 4000-5500 words each and need sustained depth)
- After all subagents complete, do a final review pass yourself: verify cross-references to concept files are correct, estimation math is arithmetically sound, Mermaid syntax is valid, no sections are missing, and no file is under 4000 words

If not using subagents:
- Generate files one at a time
- Re-read the quality requirements before EVERY file (not just the first)
- After every 5th file, pause and compare the latest file against the first file you generated — if depth, estimation detail, or architecture walkthrough quality has dropped, recalibrate before continuing

---

## Prompt

You are generating a complete set of 15 high-level system design case studies for a junior engineer who wants advanced-level understanding. These files will be uploaded to Google NotebookLM to generate audio overviews, videos, mindmaps, and quizzes — so the writing quality, depth, and narrative flow directly determine the quality of the learning material produced.

**Your task:** Generate all 15 markdown files listed below, each following the exact template and quality requirements specified in the implementation plan. Output each file into a `case-studies/` folder with the naming convention `CS-XX-system-name.md`.

Each case study should read like a senior engineer walking through a whiteboard system design session — explaining not just WHAT to design but WHY each decision is made, what alternatives exist, and what tradeoffs are being accepted.

Work through them one by one. Do not skip sections. Do not produce shallow content. Each file should be 4000-5500 words. After generating all 15, do a self-review pass: verify all required sections are present, cross-references to concept files use correct numbers and names, Mermaid diagrams are syntactically valid, and estimation math is correct.

---

## The 15 Case Studies (ordered from simpler to complex)

1. **URL Shortener** — hashing, DB choice, caching, estimation, read-heavy system
2. **Pastebin / Code Sharing Service** — blob storage, expiration/TTL, CDN, metadata vs content separation
3. **Instagram News Feed** — fanout-on-write vs fanout-on-read, caching, ranking, social graph
4. **Twitter Search** — inverted index, tokenization, ranking, real-time indexing at scale
5. **WhatsApp Chat System** — WebSockets, message queues, delivery guarantees, presence, E2E encryption
6. **YouTube / Netflix Video Streaming** — transcoding pipeline, adaptive bitrate streaming, CDN, blob storage, async processing
7. **Notification System** — event-driven architecture, priority queues, multi-channel delivery (push/SMS/email), rate limiting, user preferences
8. **Uber / Ride Matching** — geospatial indexing, real-time matching algorithm, supply-demand balancing, ETA calculation
9. **Typeahead / Autocomplete** — trie data structure, ranking, precomputation, latency optimization, personalization
10. **Distributed Rate Limiter** — sliding window, token bucket, fixed window algorithms, Redis-based coordination, multi-region
11. **Google Docs / Collaborative Editing** — conflict resolution (OT vs CRDT), real-time sync, operational transforms, cursor management
12. **Distributed Job Scheduler** — task queues, cron at scale, idempotency, failure recovery, workflow orchestration, DAG execution
13. **E-commerce Flash Sale (Ticketmaster-style)** — inventory consistency, queue-based architecture, traffic spike handling, graceful degradation, fairness
14. **Metrics & Logging System (Datadog-style)** — time-series database, aggregation pipelines, alerting engine, high write throughput, retention policies
15. **Design a Distributed Cache (Redis-like)** — consistent hashing, replication, eviction policies, cluster management, persistence, pub/sub

---

## Reference: The 25 Concept Files (for cross-referencing)

Case studies must cross-reference these concept files by number and name:

1. Horizontal vs Vertical Scaling & Auto-scaling
2. Load Balancing Deep Dive
3. CDN & Edge Computing
4. API Gateway, Reverse Proxy & Rate Limiting
5. API Design Patterns
6. SQL Databases at Scale
7. NoSQL Deep Dive
8. Database Replication
9. Database Sharding & Partitioning
10. Caching Strategies
11. Consistent Hashing
12. Data Modeling for Scale
13. Synchronous vs Asynchronous Communication
14. Message Queues & Stream Processing
15. Event-Driven Architecture & Event Sourcing
16. Real-time Communication
17. CAP Theorem & PACELC
18. Distributed Consensus Simplified
19. Fault Tolerance Patterns
20. Idempotency, Deduplication & Exactly-Once Semantics
21. Monitoring, Observability & SLOs/SLAs
22. Microservices vs Monolith
23. Blob/Object Storage Patterns
24. Search Systems
25. Distributed Task Scheduling & Workflow Orchestration

---

## Implementation Plan

### File Naming
`CS-XX-system-name.md` where XX is the zero-padded number (01, 02, ... 15). Use lowercase kebab-case for the name.

Examples:
- `CS-01-url-shortener.md`
- `CS-05-whatsapp-chat-system.md`
- `CS-13-flash-sale-ticketmaster.md`

### Output Directory
Place all files in: `case-studies/`

### Target Length Per File
4000-5500 words. These are comprehensive design walkthroughs — every section must have real substance, real numbers, and real engineering reasoning.

### Mandatory Template (every file must follow this exactly)

```markdown
# System Design: [System Name]

> One-line description of what we're building and at what scale. Example: "Design a URL shortening service like bit.ly that handles 500M new URLs per month and 10B redirects per month."

---

## Concepts Covered

[List which concept files this case study exercises. Use exact numbers and names.]

- **Concept 01** - Horizontal vs Vertical Scaling & Auto-scaling
- **Concept 10** - Caching Strategies
- [etc.]

This section helps the reader know what they'll learn and allows NotebookLM to connect this case study to the concept files.

---

## Step 1: Requirements & Scope

### Functional Requirements
[What the system must do from the user's perspective. Be specific — list 5-8 concrete requirements.]

For each requirement, briefly explain WHY it's important:
- "Users can create shortened URLs → This is the core write path"
- "Shortened URLs redirect to original URLs → This is the core read path (and will dominate traffic)"
- "URLs expire after a configurable time → Prevents stale links and manages storage growth"

### Non-Functional Requirements
[Scale, latency, availability, consistency expectations. Include SPECIFIC numbers.]

- Availability target: 99.99% uptime
- Read latency: < 50ms p99
- Write latency: < 200ms p99
- Scale: X million DAU, Y requests per second
- Durability: URLs must not be lost once created
- Consistency: eventual consistency acceptable for analytics, strong consistency for URL creation

### Out of Scope
[What we're intentionally NOT designing. This shows engineering judgment.]

- Analytics dashboard
- User accounts and authentication
- Custom domain support
- [etc.]

Briefly explain why each is out of scope (e.g., "Custom domain support adds DNS complexity that's orthogonal to the core system design").

[Target: 400-600 words for Step 1]

---

## Step 2: Back-of-Envelope Estimation

[Show ALL math step by step. This is a teaching document — the reader should learn HOW to estimate, not just see the final numbers.]

### Traffic Estimation
- Daily active users: [number]
- Read:write ratio: [e.g., 100:1]
- Writes per second: [show math]
- Reads per second: [show math]
- Peak multiplier: [e.g., 3x average]
- Peak QPS: [show math]

### Storage Estimation
- Size per record: [break down each field — URL string average 100 bytes, hash 7 bytes, timestamps 8 bytes each, etc.]
- New records per day: [number]
- Storage per year: [show math]
- Storage over 5 years: [show math]
- With replication factor: [show math]

### Bandwidth Estimation
- Incoming (writes): [show math]
- Outgoing (reads): [show math]

### Memory Estimation (for caching)
- If we cache the hottest 20% of URLs: [show math]
- Cache memory needed: [final number]

### Summary Table
| Metric | Value |
|--------|-------|
| Write QPS | X |
| Read QPS | Y |
| Peak Read QPS | Z |
| Storage (5 years) | A TB |
| Cache memory | B GB |
| Bandwidth (outgoing) | C Gbps |

[Target: 400-600 words for Step 2]

---

## Step 3: API Design

[Define the core APIs. For each endpoint, explain the design choice.]

### Core APIs

For each API:
```
METHOD /endpoint
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ... | ... | ... | ... |

**Response:**
```json
{
  "example": "response"
}
```

**Design Notes:** [Why this endpoint exists, why these parameters, why this response format. Discuss any alternative approaches considered.]

Cover 3-6 core endpoints. Include:
- The primary read and write operations
- Any secondary operations relevant to the system
- Error responses and rate limiting headers

Explain high-level API design choices:
- Why REST vs gRPC vs GraphQL for this system?
- Authentication/authorization approach
- Versioning strategy
- Pagination approach if applicable

Cross-reference: **Concept 05 - API Design Patterns** for deeper understanding of these choices.

[Target: 400-600 words for Step 3]

---

## Step 4: Data Model

[Define the database schema and explain every decision.]

### Database Choice
[State which database(s) you're using and WHY. Compare at least one alternative and explain why you didn't choose it.]

Example: "We'll use PostgreSQL for the URL mapping table because: (1) strong consistency guarantees for URL creation, (2) excellent indexing support for our lookup pattern, (3) proven at scale by companies like Instagram. We considered DynamoDB for its automatic scaling, but our access pattern is simple enough that PostgreSQL's B-tree index handles it efficiently, and we avoid vendor lock-in."

Cross-reference the relevant concept file (e.g., **Concept 06 - SQL Databases at Scale** or **Concept 07 - NoSQL Deep Dive**).

### Schema Design

For each table/collection:
```
Table: [name]
├── [field_name]    [type]       [constraints]    [why this field exists]
├── [field_name]    [type]       [constraints]    [why this field exists]
├── ...
├── INDEX: [index_name] on ([fields])             [why this index]
└── INDEX: [index_name] on ([fields])             [why this index]
```

### Access Patterns
[List the primary access patterns and how the schema supports them.]
- Pattern 1: "Given a short URL hash, find the original URL" → Primary key lookup on hash column, O(1)
- Pattern 2: "Create a new short URL" → Insert with uniqueness constraint on hash
- [etc.]

Explain how data modeling choices were driven by access patterns. Cross-reference **Concept 12 - Data Modeling for Scale**.

[Target: 400-600 words for Step 4]

---

## Step 5: High-Level Architecture

### Mermaid Diagram

[A comprehensive system architecture diagram showing ALL major components, data flows, and interactions. This should be the most detailed Mermaid diagram in the file.]

Use clear labels, group related components, and show data flow directions.

### Architecture Walkthrough

[THIS IS THE MOST CRITICAL PROSE SECTION IN THE ENTIRE FILE. NotebookLM will use this to generate audio/video explanations of the architecture. It must be thorough enough to fully describe the system without seeing the diagram.]

Structure this as a guided tour:

1. **Entry point**: Start from the user/client and explain the first thing that happens.
2. **Walk through the write path**: Trace a write request from client to final storage, explaining every component it touches and why.
3. **Walk through the read path**: Trace a read request end-to-end.
4. **Walk through at least one more interesting flow** (e.g., cache miss, failover, async processing).

For EACH component mentioned:
- What is it? (e.g., "The Application Server pool consists of stateless API servers running our URL shortening logic")
- Why does it exist? (e.g., "We need stateless servers so we can scale horizontally — any server can handle any request")
- What does it connect to? (e.g., "It reads from the Redis cache first, and on cache miss, queries the PostgreSQL read replicas")
- What happens if it fails? (e.g., "If a single app server dies, the load balancer detects the failure via health checks within 10 seconds and stops routing traffic to it")

Use explicit cross-references: "The load balancer here uses the algorithms described in **Concept 02 - Load Balancing Deep Dive**."

[Target: 600-1000 words for the walkthrough]

---

## Step 6: Deep Dives

[Pick the 3-4 most interesting/complex aspects of this system and go deep. This is where the case study goes beyond what you'd find in a typical "system design in 10 minutes" article.]

### Deep Dive 1: [Topic]

[Detailed exploration of this aspect. Include:]
- The specific problem being solved
- Algorithm or approach used (explain step by step)
- Why this approach over alternatives
- Implementation details (data structures, specific technologies)
- Edge cases and how they're handled
- A Mermaid diagram if it helps (with prose walkthrough)

Cross-reference: **Concept XX - [Name]** for the underlying concept.

### Deep Dive 2: [Topic]
[Same depth as Deep Dive 1]

### Deep Dive 3: [Topic]
[Same depth as Deep Dive 1]

### Deep Dive 4: [Topic] (if applicable)
[Same depth]

Each deep dive should be 400-700 words. The topics should be chosen based on what's most technically interesting and educational about this specific system — not generic topics.

Good deep dive topics: hash collision handling, fanout strategies, conflict resolution algorithms, geospatial indexing approaches, exactly-once delivery guarantees.

Bad deep dive topics (too generic): "how to set up monitoring," "how to use Docker," "what is a load balancer."

[Target: 1200-2800 words total for all deep dives]

---

## Step 7: Bottlenecks & Scaling

### Identifying Bottlenecks

[Walk through the architecture and identify what breaks first as traffic grows. Be specific about numbers.]

- "At 10x current load (50,000 QPS), the single PostgreSQL primary becomes a bottleneck because it can handle ~10,000 write operations per second..."
- "At 100x current load, the cache hit ratio drops below 80% because our working set exceeds the 64GB Redis instance..."

For each bottleneck, explain HOW you identified it (what metrics would show the problem).

### Scaling Solutions

[For each bottleneck, provide a specific scaling solution with before/after analysis.]

- Bottleneck: [description]
- Solution: [specific approach]
- Impact: [quantified improvement]
- New ceiling: [what the new limit becomes]
- Cross-reference: [relevant concept file]

Include a Mermaid diagram showing the scaled architecture if it differs significantly from the original. Include prose walkthrough.

### Failure Scenarios

[Walk through what happens when major components fail. This demonstrates production-readiness thinking.]

For each critical component:
- What happens if it fails completely?
- How does the system detect the failure?
- How does the system degrade gracefully?
- What is the recovery process?
- What data, if any, could be lost?

[Target: 500-800 words for Step 7]

---

## Step 8: Monitoring & Alerting

[This section is often missing from system design resources but is critical for real engineering.]

### Key Metrics to Track

Business metrics:
- [e.g., "URLs created per minute — sudden drops indicate write path issues"]
- [e.g., "Redirect latency p50/p95/p99 — tracks user experience"]

Infrastructure metrics:
- [e.g., "Database connection pool utilization — approaching 80% means it's time to add read replicas"]
- [e.g., "Cache hit ratio — below 85% means the working set is larger than cache capacity"]

### SLOs (Service Level Objectives)
- [e.g., "99.9% of redirects complete in < 100ms"]
- [e.g., "99.99% availability measured over a rolling 30-day window"]
- [e.g., "Zero URL data loss (durability SLO)"]

### Alerting Rules
- [e.g., "CRITICAL: Redirect latency p99 > 500ms for 5 minutes → page on-call"]
- [e.g., "WARNING: Cache hit ratio < 80% for 15 minutes → ticket for investigation"]
- [e.g., "CRITICAL: Write error rate > 1% for 2 minutes → page on-call"]

Cross-reference: **Concept 21 - Monitoring, Observability & SLOs/SLAs** for the underlying principles.

[Target: 300-500 words for Step 8]

---

## Summary

[Wrap up the design with key takeaways.]

### Key Design Decisions
[Recap the 3-5 most important decisions and why they were made. Each should be 1-2 sentences.]

### Top Tradeoffs
[The 3 most significant tradeoffs in this design. For each: what was gained, what was sacrificed, and why it's the right call for this system.]

### Alternative Approaches
[Briefly mention 2-3 alternative architectures that could work and when you'd choose them instead. Example: "For a read-heavy system with fewer than 1M URLs, a simple Redis-backed architecture without a relational database would work and be simpler to operate."]

[Target: 300-400 words for Summary]
```

---

### Quality Requirements (verify each file against these)

1. **Narrative flow**: Each case study should read like a conversation with a senior engineer, not a Wikipedia article. Use transitions like "Now that we have the data model, let's think about how requests actually flow through the system..."

2. **Real estimation math**: All numbers in Step 2 must be realistic and the math must be correct. Show every calculation step. Use real-world reference points (e.g., "Twitter sees ~500M tweets per day" or "A typical URL is ~80-100 characters").

3. **Architecture walkthrough depth**: The prose walkthrough in Step 5 is the most important section. NotebookLM will use it to generate audio explanations. It must be comprehensive enough to fully describe the system architecture without seeing the diagram. Minimum 600 words for this section.

4. **Deep dive quality**: Deep dives should cover non-obvious technical details. Not "we use a cache for speed" but "we use a write-around cache with a 24-hour TTL, and handle cache stampede by implementing a lease-based approach where the first thread to encounter a cache miss acquires a lease and other threads wait..."

5. **Cross-references**: Every deep dive must explicitly reference the relevant concept file by number and name. The "Concepts Covered" section at the top must list all referenced concept files.

6. **Mermaid diagrams**: At least 2 per case study (high-level architecture + at least one deep dive). Each must have a complete prose walkthrough. Diagrams must be syntactically valid.

7. **Estimation accuracy**: Numbers should be internally consistent. If you say 10M DAU with 5 actions each, your QPS calculation better show ~580 QPS (10M × 5 / 86400) not some random number. Reviewers will check the math.

8. **Progressive complexity**: Case studies 1-5 should be approachable for someone who has read the concept files. Case studies 11-15 should be genuinely challenging and cover advanced topics.

---

### Self-Review Checklist (run after generating all 15 files)

For each file, verify:
- [ ] File follows the exact template structure (all 8 steps + summary present)
- [ ] File is 4000-5500 words
- [ ] "Concepts Covered" lists correct concept file numbers and names
- [ ] Estimation math in Step 2 is correct (verify each calculation)
- [ ] At least 2 Mermaid diagrams present and syntactically valid
- [ ] Architecture walkthrough in Step 5 is ≥ 600 words
- [ ] Each diagram has a complete prose walkthrough
- [ ] Deep dives (Step 6) cover 3-4 topics with genuine technical depth
- [ ] Cross-references use correct concept file numbers
- [ ] Failure scenarios are covered in Step 7
- [ ] Monitoring section has specific metrics, SLOs, and alerting rules
- [ ] No section is fewer than 3 sentences
- [ ] Tone is conversational and narrative, not encyclopedic

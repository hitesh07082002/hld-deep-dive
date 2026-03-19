# CODEX INSTRUCTION: Generate 25 HLD Concept Files

## Execution Strategy

You are strongly encouraged to use subagents for parallel file generation. Each file is self-contained and can be generated independently.

If using subagents:
- Use model: gpt-5.4-xhigh (do NOT downgrade to a cheaper or faster model — quality is the absolute priority, not speed or cost)
- Give each subagent: the full implementation plan + template + quality requirements from this file, PLUS the complete list of all 25 concepts (so cross-references in the "Connections" section use correct file numbers and names)
- Each subagent generates exactly 1 file (not multiple — this ensures maximum depth per file)
- After all subagents complete, do a final review pass yourself: verify cross-references are correct across all 25 files, Mermaid syntax is valid, no sections are missing, and no file is under 3000 words

If not using subagents:
- Generate files one at a time
- Re-read the quality requirements before EVERY file (not just the first)
- After every 5th file, pause and compare the latest file against the first file you generated — if depth or word count has dropped, recalibrate before continuing

---

## Prompt

You are generating a complete set of 25 high-level system design (HLD) concept files for a junior engineer who wants advanced-level understanding. These files will be uploaded to Google NotebookLM to generate audio overviews, videos, mindmaps, and quizzes — so the writing quality, depth, and clarity directly determine the quality of the learning material produced.

**Your task:** Generate all 25 markdown files listed below, each following the exact template and quality requirements specified in the implementation plan. Output each file into a `concepts/` folder with the naming convention `XX-concept-name.md`.

Work through them one by one. Do not skip sections. Do not produce shallow content. Each file should be 3000-4000 words. Prioritize depth, real numbers, real examples, and clear explanations of WHY over surface-level descriptions of WHAT.

After generating all 25 files, do a self-review pass: open each file, verify it has all required sections, verify cross-references point to correct file numbers, and verify Mermaid diagrams are syntactically valid.

---

## The 25 Concepts (in learning path order)

### Foundation Layer (1-5)
1. Horizontal vs Vertical Scaling & Auto-scaling
2. Load Balancing Deep Dive (L4/L7, algorithms, health checks, DNS-based)
3. CDN & Edge Computing
4. API Gateway, Reverse Proxy & Rate Limiting
5. API Design Patterns (REST, gRPC, GraphQL, Webhooks — tradeoffs)

### Data Layer (6-12)
6. SQL Databases at Scale (indexing, query optimization, connection pooling)
7. NoSQL Deep Dive (document, key-value, column-family, graph — when to pick what)
8. Database Replication (master-slave, multi-master, sync vs async, replication lag)
9. Database Sharding & Partitioning (strategies, resharding, cross-shard queries)
10. Caching Strategies (write-through, write-back, write-around, cache stampede, eviction)
11. Consistent Hashing (virtual nodes, ring architecture, rebalancing)
12. Data Modeling for Scale (denormalization, hot keys, time-series patterns, access patterns)

### Communication Layer (13-16)
13. Synchronous vs Asynchronous Communication Patterns
14. Message Queues & Stream Processing (Kafka vs RabbitMQ vs SQS, exactly-once delivery)
15. Event-Driven Architecture & Event Sourcing (CQRS, saga pattern)
16. Real-time Communication (WebSockets, SSE, long polling, presence systems)

### Reliability & Distributed Systems (17-21)
17. CAP Theorem & PACELC — What They Actually Mean With Real Examples
18. Distributed Consensus Simplified (leader election, quorum, Raft, split-brain)
19. Fault Tolerance Patterns (circuit breaker, retry with backoff, bulkhead, graceful degradation)
20. Idempotency, Deduplication & Exactly-Once Semantics
21. Monitoring, Observability & SLOs/SLAs (metrics, logs, traces, alerting)

### Architecture Patterns (22-25)
22. Microservices vs Monolith — The Real Engineering Tradeoffs
23. Blob/Object Storage Patterns (S3, presigned URLs, multipart upload, CDN integration)
24. Search Systems (inverted index, tokenization, relevance scoring, Elasticsearch)
25. Distributed Task Scheduling & Workflow Orchestration

---

## Implementation Plan

### File Naming
`XX-concept-name.md` where XX is the zero-padded number (01, 02, ... 25). Use lowercase kebab-case for the name.

Examples:
- `01-horizontal-vs-vertical-scaling.md`
- `10-caching-strategies.md`
- `17-cap-theorem-and-pacelc.md`

### Output Directory
Place all files in: `concepts/`

### Target Length Per File
3000-4000 words. These are comprehensive learning documents, NOT cheatsheets. Every section should have substance.

### Mandatory Template (every file must follow this exactly)

```markdown
# [Concept Name]

> One-line summary explaining what this concept is in plain English. Make it clear and memorable.

---

## The Problem

[This section sets up WHY this concept exists. Start with a concrete scenario that makes the reader feel the pain.]

- What specific problem does this concept solve?
- What breaks in a system without it?
- Start with a vivid, concrete scenario. Example: "Imagine your e-commerce app gets featured on national TV. Traffic spikes from 100 to 100,000 requests per second. Your single database server's CPU hits 100%. Queries that took 5ms now take 12 seconds. Shopping carts time out. Orders fail. You're losing $50,000 per minute..."
- Build urgency — make the reader understand why this matters BEFORE explaining the solution.
- Include real-world failure examples where possible (e.g., "This is essentially what happened to Healthcare.gov at launch...").

[Target: 300-500 words for this section]

---

## Core Concept Explained

[Full explanation from first principles. This is the main teaching section.]

- Start with a real-world analogy that makes the concept intuitive. Use restaurant kitchens, highway traffic, library systems, postal services — whatever fits best.
- Then transition to the technical explanation, building up gradually.
- Do NOT assume knowledge beyond basics (the reader knows what a server, database, and API are — but don't assume they know anything about distributed systems).
- Cover ALL sub-topics within this concept. For example, if the concept is "Load Balancing," cover L4 vs L7, all major algorithms (round-robin, weighted, least connections, IP hash, consistent hashing), health checks, session persistence, and DNS-based load balancing.
- Use specific technical details — not "load balancers distribute traffic" but "an L7 load balancer inspects the HTTP headers and can route /api/* requests to your API server pool while routing /static/* to your CDN origin, and it can even examine cookies to maintain session affinity."
- Where relevant, include specific numbers and performance characteristics.

[Target: 800-1200 words for this section]

---

## Architecture Diagram

### Mermaid Diagram

[Include a well-structured Mermaid diagram showing the architecture or flow. Use graph TD, graph LR, sequenceDiagram, or flowchart — whichever best fits the concept.]

[The diagram should be detailed enough to show the key components, data flows, and decision points.]

### Diagram Walkthrough

[THIS SECTION IS CRITICAL. NotebookLM cannot interpret Mermaid diagrams — it will only read this prose. Write a complete, step-by-step walkthrough of the diagram.]

- Name every component shown in the diagram and explain what it does.
- Walk through at least 2 different request flows or scenarios step by step.
- For each step, explain what happens, why, and what data moves where.
- Use language like "Starting from the top left, a user sends an HTTP request to the load balancer. The load balancer sits at the entry point of our system and its job is to..."
- A reader should be able to perfectly reconstruct the diagram from this prose alone.

[Target: 400-600 words for this section]

---

## How It Works Under the Hood

[Go deeper than the core concept section. This is where you cover implementation details that separate shallow from deep understanding.]

- Algorithms involved (explain them simply — e.g., for consistent hashing, explain the hash ring, virtual nodes, and how node addition/removal works step by step).
- Data structures used (e.g., "Kafka uses an append-only log internally, which is essentially a sequence of records ordered by offset...").
- Protocol-level details where relevant (e.g., "WebSocket connections start as a regular HTTP request with an Upgrade header. The server responds with 101 Switching Protocols...").
- Memory and storage implications (e.g., "Each entry in a Bloom filter uses approximately 10 bits per element for a 1% false positive rate...").
- Include specific numbers, benchmarks, and performance characteristics where applicable.
- Cover edge cases and failure modes.

[Target: 500-800 words for this section]

---

## Key Tradeoffs & Limitations

[This section is what separates junior from senior thinking. Be thorough.]

- What are the costs of using this approach? (complexity, latency, operational overhead, money)
- What does it NOT solve? What problems remain even after implementing this?
- When should you explicitly NOT use this? Give concrete scenarios.
- Compare alternatives head-to-head and explain when each is appropriate.
- Use a clear "Choose X when... Choose Y when..." decision format where applicable.
- Include specific examples: "If your application has fewer than 10,000 DAU and a single PostgreSQL instance handles your load comfortably, introducing sharding adds complexity for zero benefit."

[Target: 400-600 words for this section]

---

## Common Misconceptions

[3-5 misconceptions that people commonly hold about this concept. This is extremely valuable for going from shallow to deep understanding.]

For each misconception:
1. State the misconception clearly: "Many people believe that..."
2. Explain why it's wrong with specific reasoning.
3. State the correct understanding.
4. Explain why the misconception exists in the first place (what makes it seem true on the surface).

Examples of good misconceptions:
- "Many believe adding more cache always improves performance. In reality, caching cold or infrequently accessed data wastes memory and adds latency through cache misses. The optimal caching strategy targets the hot 10-20% of data that accounts for 80% of reads."
- "A common belief is that NoSQL databases are always faster than SQL. The reality is that a properly indexed PostgreSQL query can return results in under 1ms, often outperforming a poorly designed MongoDB query."

[Target: 300-500 words for this section]

---

## Real-World Usage

[Concrete examples of how major companies use this concept. Include enough detail to be meaningful — not just "Netflix uses caching."]

For each example:
- Name the company and the specific system/service.
- Explain what scale they operate at (requests per second, data volume, etc.).
- Describe HOW they use this concept — what specific implementation choices they made.
- If available, mention why they chose this approach over alternatives.

Include at least 3 real-world examples. Prefer well-documented cases from engineering blogs (e.g., Netflix Tech Blog, Uber Engineering, Stripe's blog, Meta Engineering, etc.).

[Target: 300-500 words for this section]

---

## Interview Angle

[3-5 common interview questions related to this concept. For each question, provide a THINKING FRAMEWORK, not a scripted answer.]

Format for each:
**Q: [Question]**
**How to approach it:** [2-4 bullet points explaining what to think about, what tradeoffs to discuss, what follow-up questions to expect, and what a strong answer covers]

The goal is to teach the reader HOW to think about the question, not to memorize an answer.

[Target: 300-400 words for this section]

---

## Connections to Other Concepts

[Explicitly link this concept to other files in the set. This helps NotebookLM make cross-topic connections.]

- Name at least 3-5 connections to other concept files (by number and name).
- For EACH connection, explain HOW they relate — don't just say "see also."
- Example: "**Concept 09 - Database Sharding** directly builds on this concept. Once you've replicated your database for read scaling (covered here), the next bottleneck is write scaling — which is where sharding comes in. The replication strategies discussed in this file determine how each shard maintains its own replicas."
- Example: "**Concept 10 - Caching Strategies** is often deployed alongside replication. A common pattern is to place a Redis cache in front of read replicas to absorb the hottest queries, reducing load on replicas by 60-80%. But this introduces cache invalidation challenges when the primary writes new data..."

[Target: 200-400 words for this section]
```

---

### Quality Requirements (verify each file against these)

1. **Tone**: Conversational but technically precise. Imagine a senior engineer explaining to a smart junior over coffee. Not academic, not dumbed-down.

2. **Specificity**: Every claim must be specific. NOT "caching improves performance" but "a Redis cache lookup takes ~0.5ms compared to ~5-10ms for an indexed PostgreSQL query, so caching your hottest 20% of data can reduce p99 latency by 10x."

3. **Diagrams**: Every file must include at least ONE Mermaid diagram with a complete prose walkthrough. The prose walkthrough is non-negotiable — NotebookLM relies on it. Mermaid diagrams must be syntactically valid (test mentally that the diagram would render).

4. **Self-contained**: Each file must be fully understandable on its own. A reader should be able to learn the complete concept from this file alone without reading any other file. Cross-references at the end are for deeper exploration, not for filling gaps.

5. **Depth over breadth**: If you find yourself writing surface-level descriptions, stop and go deeper. The reader already has access to Wikipedia — they need the understanding that comes from experienced explanation.

6. **Real numbers**: Include actual performance numbers, latency figures, throughput benchmarks, and capacity estimates wherever relevant. These should be realistic and cite-worthy (e.g., "Redis handles ~100,000 operations per second on a single node" not "Redis is very fast").

7. **Cross-references**: The "Connections to Other Concepts" section must reference files by their exact number and name from the list above. Verify these are correct.

8. **No fluff**: Every sentence should teach something. Remove filler phrases, unnecessary transitions, and repetitive statements. Density of information matters.

---

### Self-Review Checklist (run after generating all 25 files)

For each file, verify:
- [ ] File follows the exact template structure (all 9 sections present)
- [ ] File is 3000-4000 words
- [ ] "The Problem" section starts with a concrete scenario, not an abstract definition
- [ ] At least 1 Mermaid diagram is present and syntactically valid
- [ ] Diagram has a prose walkthrough that fully describes it without seeing the diagram
- [ ] "Common Misconceptions" has 3-5 specific misconceptions
- [ ] "Real-World Usage" has at least 3 company examples with specific details
- [ ] "Connections" references correct file numbers and names
- [ ] No section is fewer than 3 sentences
- [ ] Specific numbers and benchmarks are included where relevant
- [ ] Tone is conversational, not academic or textbook-like

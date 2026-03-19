# CODEX INSTRUCTION: Generate 5 Helper Files

## Execution Strategy

You are strongly encouraged to use subagents for parallel file generation. Each file is self-contained and can be generated independently.

If using subagents:
- Use model: gpt-5.4-xhigh (do NOT downgrade to a cheaper or faster model — quality is the absolute priority, not speed or cost)
- Give each subagent: the full specification for that one file from this document, PLUS the complete list of all 25 concept files AND all 15 case study titles (so cross-references and mapping tables are correct)
- Each subagent generates exactly 1 helper file
- After all subagents complete, do a final review pass yourself: verify the README mapping table covers all 25 concepts and 15 case studies, all file names are correct, and no placeholder text remains

If not using subagents:
- Generate files one at a time, starting with README.md (since other helper files reference it)

---

## Prompt

You are generating 5 supplementary/helper markdown files that accompany a set of 25 HLD concept files and 15 case study files. These helper files serve as navigation, reference, and study guides. They will be uploaded to Google NotebookLM alongside the concept and case study files to generate audio overviews, videos, mindmaps, and quizzes.

**Your task:** Generate all 5 markdown files listed below, each following the exact specification provided. Output each file into a `helpers/` folder.

These files must be practical, dense with useful information, and designed to enhance the learning experience when combined with the concept and case study files in NotebookLM.

---

## The 5 Helper Files

1. `README.md` — Master guide, learning path, and concept-to-case-study mapping
2. `SYSTEM-DESIGN-FRAMEWORK.md` — Step-by-step method for approaching any HLD problem
3. `NUMBERS-AND-ESTIMATION.md` — Latency numbers, storage/bandwidth/QPS estimation templates
4. `TRADEOFFS-DECISION-MATRIX.md` — Every major HLD tradeoff with decision criteria
5. `NOTEBOOKLM-STUDY-GUIDE.md` — How to use these files with NotebookLM for maximum learning

---

## Reference: Complete File Lists (for cross-referencing)

### 25 Concept Files
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

### 15 Case Studies
CS-01. URL Shortener
CS-02. Pastebin / Code Sharing Service
CS-03. Instagram News Feed
CS-04. Twitter Search
CS-05. WhatsApp Chat System
CS-06. YouTube / Netflix Video Streaming
CS-07. Notification System
CS-08. Uber / Ride Matching
CS-09. Typeahead / Autocomplete
CS-10. Distributed Rate Limiter
CS-11. Google Docs / Collaborative Editing
CS-12. Distributed Job Scheduler
CS-13. E-commerce Flash Sale (Ticketmaster-style)
CS-14. Metrics & Logging System (Datadog-style)
CS-15. Design a Distributed Cache (Redis-like)

---

## Implementation Plan

### File 1: README.md

**Purpose:** The master navigation document. When uploaded to NotebookLM, this helps the AI understand the structure and relationships across all files.

**Target length:** 1500-2000 words

**Required sections:**

```markdown
# HLD Deep Dive — Complete System Design Learning Kit

## What Is This?

[2-3 paragraphs explaining this is a comprehensive set of 45 files covering system design from fundamentals to advanced case studies. Explain who it's for (junior-to-mid engineers wanting deep understanding) and how it's structured.]

## How This Kit Is Organized

[Explain the three categories: 25 concepts, 15 case studies, 5 helper files. Briefly explain what each category contains and how they relate.]

## Recommended Learning Path

[A specific, ordered sequence for studying the material. Group into phases:]

### Phase 1: Foundations (Week 1-2)
[List concepts 1-5 in order, with a one-line note on what each covers and why it comes first]

### Phase 2: Data Layer (Week 2-3)
[Concepts 6-12]

### Phase 3: Communication & Reliability (Week 3-4)
[Concepts 13-21]

### Phase 4: Architecture Patterns (Week 4-5)
[Concepts 22-25]

### Phase 5: Case Studies — Beginner (Week 5-6)
[CS-01 through CS-05, noting these are approachable starting points]

### Phase 6: Case Studies — Intermediate (Week 6-7)
[CS-06 through CS-10]

### Phase 7: Case Studies — Advanced (Week 7-8)
[CS-11 through CS-15]

## Concept-to-Case-Study Mapping

[A comprehensive table showing which concepts appear in which case studies. This is extremely valuable for cross-referencing.]

| Concept | Case Studies Where It Appears |
|---------|-------------------------------|
| 01 - Horizontal vs Vertical Scaling | CS-01, CS-03, CS-05, CS-06, CS-13 |
| 02 - Load Balancing | CS-01, CS-03, CS-06, CS-08, CS-13 |
| [... complete for all 25 concepts] |

## Case Study Difficulty & Prerequisite Concepts

[For each case study, list difficulty level and which concept files should be read first.]

| Case Study | Difficulty | Read These Concepts First |
|------------|------------|--------------------------|
| CS-01 URL Shortener | Beginner | 01, 06, 10, 11 |
| [... complete for all 15] |

## How to Use with NotebookLM

[Brief pointer to the NOTEBOOKLM-STUDY-GUIDE.md file for detailed instructions.]

## How to Use with Claude Code

[Brief suggestions: use Claude Code for interactive Q&A, deeper exploration of specific topics, generating practice problems, or creating custom case studies after completing the core material.]
```

---

### File 2: SYSTEM-DESIGN-FRAMEWORK.md

**Purpose:** A reusable step-by-step framework for approaching ANY system design problem. This is the mental model file — when uploaded to NotebookLM, it can generate audio that teaches the systematic approach.

**Target length:** 2500-3500 words

**Required sections:**

```markdown
# The System Design Framework

> A step-by-step method for approaching any high-level design problem, from a blank whiteboard to a complete architecture.

## Why You Need a Framework

[Explain that without a framework, engineers either freeze up or jump straight to drawing boxes. A framework gives you a systematic approach that ensures you cover all bases and demonstrate structured thinking. 2-3 paragraphs.]

## The 8-Step Framework

### Step 1: Clarify Requirements (3-5 minutes)

[Detailed guidance on how to gather and clarify requirements.]

- Functional requirements: What must the system DO?
- Non-functional requirements: Scale, latency, availability, consistency, durability
- How to ask clarifying questions (with example questions for different system types)
- How to scope: what to include vs explicitly exclude
- Common trap: jumping to solutions before understanding the problem
- Template: "Let me make sure I understand. We're building X that needs to handle Y users doing Z action, with A latency and B availability..."

### Step 2: Back-of-Envelope Estimation (3-5 minutes)

[Detailed guidance on estimation.]

- Start with users → actions per user → QPS
- Storage: per-record size × records per day × retention period
- Bandwidth: request size × QPS
- Memory (for caching): identify hot data percentage × total data size
- Common reference numbers (link to NUMBERS-AND-ESTIMATION.md)
- Show a complete worked example for a generic social media app
- Common trap: spending too long on exact numbers — estimates are meant to be rough

### Step 3: Define the API (3-5 minutes)

[Detailed guidance on API design.]

- Identify the core operations (usually 3-6)
- For each: HTTP method, endpoint, parameters, response
- When to choose REST vs gRPC vs GraphQL
- Think about pagination, rate limiting, authentication
- Common trap: overcomplicating the API with edge cases too early

### Step 4: Design the Data Model (5 minutes)

[Detailed guidance on data modeling.]

- Start from access patterns, not from entities
- Choose SQL vs NoSQL based on the actual requirements
- Define tables/collections with fields and types
- Identify indexes needed for your access patterns
- Think about relationships and how they're stored
- Common trap: defaulting to SQL or NoSQL without thinking about why

### Step 5: Draw the High-Level Architecture (5-8 minutes)

[Detailed guidance on the main architecture diagram.]

- Start with the client and work inward
- Add components only when there's a reason (don't add Kafka just because)
- Standard building blocks: Load balancer → App servers → Cache → Database
- When to add: CDN, message queue, search index, blob storage, etc.
- Show how to think about read path vs write path separately
- Common trap: drawing 20 boxes before understanding if you need them

### Step 6: Deep Dive into Key Components (8-10 minutes)

[Detailed guidance on choosing and executing deep dives.]

- How to pick which 2-3 components to deep dive (pick the hardest/most interesting)
- For each deep dive: explain the algorithm, data structure, or protocol in detail
- Show tradeoffs and alternatives considered
- This is where you demonstrate depth of knowledge
- Common trap: going too deep on one component and running out of time

### Step 7: Address Bottlenecks & Scaling (3-5 minutes)

[Detailed guidance on scaling discussion.]

- Walk through the architecture at 10x and 100x scale
- Identify bottlenecks systematically (CPU, memory, disk, network, database)
- For each bottleneck: propose a specific solution
- Discuss failure modes and graceful degradation
- Common trap: saying "we can just add more servers" without addressing stateful components

### Step 8: Monitoring & Wrap-up (2-3 minutes)

[Detailed guidance on the finishing touch.]

- Key metrics to track
- SLOs to define
- Alerting approach
- Quick recap of the design and key tradeoffs
- Common trap: skipping this section entirely

## Framework Applied: Mini Example

[Walk through the framework using a simplified example (e.g., design a simple key-value store) in about 500 words, showing how each step works in practice. Keep it concise but show the framework in action.]

## Common Anti-Patterns

[5-7 anti-patterns to avoid in system design discussions.]

- Jumping to solutions without clarifying requirements
- Using buzzwords without understanding them ("we'll use Kafka" — why?)
- Ignoring estimation entirely
- Over-engineering (adding microservices for a system with 1000 users)
- Under-engineering (ignoring that the system needs to handle 1M QPS)
- Not discussing tradeoffs (every decision has a cost)
- Treating components as magic boxes ("the cache handles it" — how?)
```

---

### File 3: NUMBERS-AND-ESTIMATION.md

**Purpose:** A reference document containing all the numbers and formulas needed for back-of-envelope estimation. Invaluable for both learning and interview prep. When NotebookLM processes this, it can quiz you on these numbers.

**Target length:** 2000-3000 words

**Required sections:**

```markdown
# Numbers Every Engineer Should Know

> Reference sheet for back-of-envelope estimation in system design.

## Latency Numbers

[Present these in a clear, memorable format with context.]

### Storage Latencies
- L1 cache reference: 0.5 ns
- L2 cache reference: 7 ns
- Main memory (RAM) reference: 100 ns
- SSD random read: 150 μs
- HDD random read (seek): 10 ms
- [Complete the standard latency table]

### Network Latencies
- Send packet within same datacenter: 0.5 ms
- Round trip within same datacenter: 0.5 ms
- Send packet from US to Europe: 75 ms
- Send packet from US to Asia: 150 ms
- [etc.]

### Key Insight
[Explain what these numbers mean practically — e.g., "A network round trip to another datacenter is 1000x slower than a RAM access. This is why caching is so powerful — it moves data from network to memory."]

## Throughput Numbers

### Database Throughput
- Single PostgreSQL instance (simple queries): ~10,000-30,000 QPS
- Single MySQL instance: ~10,000-30,000 QPS
- Single Redis instance: ~100,000 operations/sec
- Single Memcached instance: ~100,000-200,000 operations/sec
- Single MongoDB instance: ~10,000-50,000 QPS depending on query complexity
- Single Cassandra node: ~5,000-10,000 writes/sec
- [Include context: "These vary wildly based on query complexity, hardware, and indexing"]

### Web Server Throughput
- Single web server (Node.js): ~10,000-50,000 simple requests/sec
- Single web server (Go): ~50,000-100,000 simple requests/sec
- [etc.]

### Network Throughput
- 1 Gbps link: ~125 MB/sec
- 10 Gbps link: ~1.25 GB/sec
- [etc.]

## Size Reference Numbers

### Data Sizes
- Character (ASCII): 1 byte
- Character (UTF-8, average): 2-3 bytes
- UUID: 16 bytes (128 bits)
- Unix timestamp: 4 bytes (32-bit) or 8 bytes (64-bit)
- IPv4 address: 4 bytes
- IPv6 address: 16 bytes
- Average URL length: 80-100 bytes
- Average tweet/short text: 200-300 bytes
- Average email: 50-100 KB
- Average photo (compressed JPEG): 200-500 KB
- Average HD video minute: 100-150 MB
- [etc.]

### Scale Reference
- 1 million seconds ≈ 11.5 days
- 1 billion seconds ≈ 31.7 years
- Seconds in a day: 86,400 (~100K for estimation)
- Seconds in a month: ~2.5 million
- Seconds in a year: ~31.5 million

## Estimation Templates

### Template 1: QPS from DAU
[Step-by-step template with formula and worked example]

```
Given: X million DAU, each user performs Y actions per day
Average QPS = (X × 10^6 × Y) / 86,400
Peak QPS = Average QPS × 2-3 (peak multiplier)

Example: 10M DAU, 5 actions each
Average QPS = (10,000,000 × 5) / 86,400 ≈ 578 QPS
Peak QPS = 578 × 3 ≈ 1,734 QPS
```

### Template 2: Storage Estimation
[Step-by-step template with formula and worked example]

### Template 3: Bandwidth Estimation
[Step-by-step template with formula and worked example]

### Template 4: Cache Size Estimation
[Step-by-step template with formula and worked example]

### Template 5: Number of Servers Needed
[Step-by-step template with formula and worked example]

## Quick Rules of Thumb

[10-15 practical rules of thumb for estimation.]

- "The 80/20 rule: 80% of traffic hits 20% of your data — cache that 20%"
- "A single server can handle ~10K-50K concurrent connections with modern frameworks"
- "If your data fits in memory (~100GB), you might not need distributed caching"
- "For read-heavy systems (>90% reads), replicas are your first scaling lever"
- "For write-heavy systems, consider sharding early"
- [etc.]

## Practice Problems

[5 mini estimation problems with answers.]

1. "Estimate the storage needed for Twitter's tweets over one year" [solution]
2. "How many servers does YouTube need to handle video uploads?" [solution]
3. [etc.]
```

---

### File 4: TRADEOFFS-DECISION-MATRIX.md

**Purpose:** A comprehensive reference of every major HLD tradeoff with clear decision criteria. This is extremely valuable for NotebookLM — it can generate focused podcasts or quizzes on specific tradeoff categories.

**Target length:** 2500-3500 words

**Required sections:**

```markdown
# System Design Tradeoffs Decision Matrix

> For every major design decision, this document explains the options, when to choose each, and what you're giving up.

## How to Use This Document

[Brief explanation: this is a decision reference. When you face a design choice, find the relevant section, understand the tradeoffs, and make an informed decision. It's not about memorizing — it's about understanding the reasoning.]

## Data Storage Tradeoffs

### SQL vs NoSQL
[Detailed comparison with clear decision criteria]

**Choose SQL when:**
- [criteria with explanation]
- [criteria with explanation]

**Choose NoSQL when:**
- [criteria with explanation]
- [criteria with explanation]

**What you give up with SQL:** [specific limitations]
**What you give up with NoSQL:** [specific limitations]
**Real-world signal:** [e.g., "If you find yourself writing complex JOINs across 5+ tables on every read, your relational model might be fighting your access pattern"]

Cross-reference: Concept 06, Concept 07

### SQL Database Selection (PostgreSQL vs MySQL vs others)
[Same format]

### NoSQL Category Selection (Document vs Key-Value vs Column-Family vs Graph)
[Same format with decision criteria for each]

### Normalization vs Denormalization
[Same format]

## Caching Tradeoffs

### Cache-Aside vs Write-Through vs Write-Back vs Write-Around
[Detailed comparison of all four patterns with decision criteria]

### Redis vs Memcached
[Same format]

### Where to Cache (Client, CDN, App Server, Database)
[Same format]

## Communication Tradeoffs

### Synchronous vs Asynchronous
[Same format]

### REST vs gRPC vs GraphQL
[Same format]

### Push vs Pull (for feeds, notifications, etc.)
[Same format]

### WebSocket vs SSE vs Long Polling
[Same format]

## Consistency Tradeoffs

### Strong vs Eventual Consistency
[Same format]

### Optimistic vs Pessimistic Locking
[Same format]

### Read-Your-Writes vs Full Consistency
[Same format]

## Scaling Tradeoffs

### Horizontal vs Vertical Scaling
[Same format]

### Replication vs Sharding
[Same format]

### Monolith vs Microservices
[Same format]

## Architecture Pattern Tradeoffs

### Fanout-on-Write vs Fanout-on-Read
[Same format]

### Precomputation vs On-the-Fly Computation
[Same format]

### Batch Processing vs Stream Processing
[Same format]

### Single Leader vs Multi-Leader vs Leaderless Replication
[Same format]

## The Meta-Tradeoff: Simplicity vs Optimization

[A closing section about the most important tradeoff of all: keeping things simple vs optimizing early. Explain that most systems should start simple and add complexity only when specific bottlenecks are identified. Over-engineering is a bigger risk than under-engineering for most teams.]
```

---

### File 5: NOTEBOOKLM-STUDY-GUIDE.md

**Purpose:** Specific instructions on how to get maximum value from these files using NotebookLM. This file itself should also be uploaded to NotebookLM.

**Target length:** 1500-2000 words

**Required sections:**

```markdown
# NotebookLM Study Guide for HLD Deep Dive

> How to use these 45 files with Google NotebookLM to generate audio overviews, videos, mindmaps, quizzes, and more for maximum learning.

## Setting Up Your Notebook

### Option A: Single Notebook (Recommended for most users)
[Instructions for uploading all 45 files into one notebook. Explain the benefits: cross-topic connections, comprehensive quizzes, holistic understanding.]

### Option B: Multiple Notebooks (For focused study)
[How to split files across notebooks for focused sessions.]
- Notebook 1: Concepts 1-12 (Foundation + Data Layer)
- Notebook 2: Concepts 13-25 (Communication + Reliability + Architecture)
- Notebook 3: Case Studies 1-8 (Beginner + Intermediate)
- Notebook 4: Case Studies 9-15 (Intermediate + Advanced)
- Notebook 5: All helper files + selected concepts for review

## Generating Audio Overviews (Podcasts)

[Detailed guidance on using NotebookLM's audio overview feature.]

### Best Prompts for Audio Generation

**For concept files:**
- "Generate an audio overview explaining [concept name] as if teaching it to a junior engineer. Focus on the 'why' and real-world examples."
- "Create a podcast-style discussion about the tradeoffs of [concept]. Include common misconceptions."
- "Explain how [concept A] and [concept B] relate to each other with concrete examples."

**For case studies:**
- "Walk through the [system name] design step by step, explaining each architectural decision and its tradeoffs."
- "Generate a discussion about the bottlenecks and scaling challenges in the [system name] case study."
- "Compare the approaches used in [case study A] vs [case study B] — what's similar, what's different, and why?"

**For cross-cutting topics:**
- "Using the tradeoffs decision matrix, explain when to choose SQL vs NoSQL with examples from the case studies."
- "Walk through the system design framework step by step, using the URL shortener as a running example."

### Tips for Better Audio
- Select specific source files before generating — don't rely on all 45 files at once for a single audio overview
- For concept audios: select the concept file + 1-2 related case studies
- For case study audios: select the case study file + 3-4 referenced concept files
- Regenerate if the first output is too surface-level — NotebookLM improves with specific prompting

## Generating Videos

[Guidance on NotebookLM's video generation features.]

### Best Approaches for Video Content
- Generate single-concept explainer videos (select one concept file)
- Generate case study walkthrough videos (select one case study + referenced concepts)
- Generate comparison videos (select 2-3 related concept files, e.g., all caching-related files)

### Recommended Video Topics
[List 10-15 specific video ideas with which files to select for each.]

1. "How Caching Works and Why It Matters" → Select: Concept 10 + CS-01 + CS-03
2. "Designing a Chat System from Scratch" → Select: CS-05 + Concepts 14, 16, 17, 20
3. [etc.]

## Creating Quizzes

[How to use NotebookLM to generate quizzes for self-testing.]

### Quiz Strategies
- After studying a phase, select all files from that phase and ask NotebookLM to generate a quiz
- Use the estimation reference file to generate "estimate this" style questions
- Use the tradeoffs matrix to generate "when would you choose X over Y" questions

### Sample Quiz Prompts
- "Create a 10-question quiz testing understanding of database concepts (files 6-12). Include both factual and scenario-based questions."
- "Generate 5 estimation problems similar to system design interviews, with answers."
- "Create a quiz comparing the architectures of the URL shortener, Instagram feed, and YouTube streaming designs."

## Creating Mindmaps

[How to generate effective mindmaps using NotebookLM.]

- Best for: understanding how concepts relate to each other
- Select: README.md (which has the mapping table) + 5-6 concept files from the same theme
- Prompt: "Create a mindmap showing how [theme] concepts connect, with key relationships labeled."

## Suggested Study Schedule

### Week 1-2: Foundation Concepts
- Day 1-2: Read/listen to Concepts 1-3 (Scaling, Load Balancing, CDN)
- Day 3-4: Read/listen to Concepts 4-5 (API Gateway, API Design)
- Day 5: Generate quiz on Concepts 1-5, review weak areas
- Day 6-7: Start Case Study CS-01 (URL Shortener) — your first design

### Week 3-4: Data Layer Deep Dive
[Similar daily breakdown for Concepts 6-12]

### Week 5-6: Communication & Reliability
[Similar daily breakdown for Concepts 13-21]

### Week 7-8: Architecture + Advanced Case Studies
[Similar daily breakdown for Concepts 22-25 and Case Studies 6-15]

### Week 9-10: Review & Practice
- Generate cross-cutting quizzes
- Attempt case studies from memory before re-reading
- Use the framework document to practice structuring answers

## Tips for Maximum Retention

- Listen to audio overviews during commute or exercise — spaced repetition
- After listening to a concept audio, try to explain it out loud in your own words
- For case studies, pause the audio after "Requirements" and try to design the system yourself before continuing
- Use Claude Code as a study partner — ask it to quiz you or explain things you didn't understand from the audio
- Revisit the tradeoffs matrix weekly — it's the highest-density learning resource in the set
```

---

### Quality Requirements

1. **Accuracy**: All cross-references must use correct file numbers and names from the lists above.
2. **Completeness**: Every section specified in each file template must be present.
3. **Practical**: Study schedules, prompts, and tips must be specific and actionable — not generic advice.
4. **Self-contained**: Each file should make sense on its own while also fitting into the larger collection.
5. **NotebookLM-optimized**: Written in clear prose that NotebookLM can use to generate quality audio/video. Avoid excessive formatting that might confuse AI processing.

### Self-Review Checklist

- [ ] README has complete concept-to-case-study mapping table (all 25 concepts mapped)
- [ ] README has complete case study prerequisite table (all 15 case studies listed)
- [ ] Framework document covers all 8 steps with detailed guidance
- [ ] Numbers document has complete latency table, throughput numbers, and 5 estimation templates
- [ ] Tradeoffs matrix covers at least 15 distinct tradeoff decisions
- [ ] Study guide has specific NotebookLM prompts (not generic instructions)
- [ ] All file references use correct numbers and names
- [ ] No placeholder text remains

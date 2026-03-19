# HLD Deep Dive — Master Project Brief

## Project Overview

Generate a complete system design learning kit consisting of 45 markdown files organized into three categories. These files will be uploaded to Google NotebookLM for generating audio overviews, videos, mindmaps, and quizzes.

## Output Structure

```
hld-deep-dive/
├── concepts/
│   ├── 01-horizontal-vs-vertical-scaling.md
│   ├── 02-load-balancing-deep-dive.md
│   ├── 03-cdn-and-edge-computing.md
│   ├── 04-api-gateway-reverse-proxy-rate-limiting.md
│   ├── 05-api-design-patterns.md
│   ├── 06-sql-databases-at-scale.md
│   ├── 07-nosql-deep-dive.md
│   ├── 08-database-replication.md
│   ├── 09-database-sharding-and-partitioning.md
│   ├── 10-caching-strategies.md
│   ├── 11-consistent-hashing.md
│   ├── 12-data-modeling-for-scale.md
│   ├── 13-sync-vs-async-communication.md
│   ├── 14-message-queues-and-stream-processing.md
│   ├── 15-event-driven-architecture-and-event-sourcing.md
│   ├── 16-real-time-communication.md
│   ├── 17-cap-theorem-and-pacelc.md
│   ├── 18-distributed-consensus.md
│   ├── 19-fault-tolerance-patterns.md
│   ├── 20-idempotency-deduplication-exactly-once.md
│   ├── 21-monitoring-observability-slos.md
│   ├── 22-microservices-vs-monolith.md
│   ├── 23-blob-object-storage-patterns.md
│   ├── 24-search-systems.md
│   └── 25-distributed-task-scheduling.md
│
├── case-studies/
│   ├── CS-01-url-shortener.md
│   ├── CS-02-pastebin-code-sharing.md
│   ├── CS-03-instagram-news-feed.md
│   ├── CS-04-twitter-search.md
│   ├── CS-05-whatsapp-chat-system.md
│   ├── CS-06-youtube-netflix-streaming.md
│   ├── CS-07-notification-system.md
│   ├── CS-08-uber-ride-matching.md
│   ├── CS-09-typeahead-autocomplete.md
│   ├── CS-10-distributed-rate-limiter.md
│   ├── CS-11-google-docs-collaborative-editing.md
│   ├── CS-12-distributed-job-scheduler.md
│   ├── CS-13-flash-sale-ticketmaster.md
│   ├── CS-14-metrics-logging-system.md
│   └── CS-15-distributed-cache-redis.md
│
└── helpers/
    ├── README.md
    ├── SYSTEM-DESIGN-FRAMEWORK.md
    ├── NUMBERS-AND-ESTIMATION.md
    ├── TRADEOFFS-DECISION-MATRIX.md
    └── NOTEBOOKLM-STUDY-GUIDE.md
```

## Execution Plan

This project should be executed in 3 batches. Each batch has its own detailed instruction file.

### Batch 1: Concept Files (25 files)
- **Instruction file:** `codex-concepts-instruction.md`
- **Output:** `concepts/` folder with 25 markdown files
- **Per-file length:** 3000-4000 words
- **Estimated total:** ~75,000-100,000 words

### Batch 2: Case Study Files (15 files)
- **Instruction file:** `codex-case-studies-instruction.md`
- **Output:** `case-studies/` folder with 15 markdown files
- **Per-file length:** 4000-5500 words
- **Estimated total:** ~60,000-82,500 words

### Batch 3: Helper Files (5 files)
- **Instruction file:** `codex-helpers-instruction.md`
- **Output:** `helpers/` folder with 5 markdown files
- **Per-file length:** 1500-3500 words
- **Estimated total:** ~10,000-14,000 words

## Critical Quality Principles (apply to ALL batches)

1. **NotebookLM-first writing**: These files are consumed by AI to generate audio/video. Every diagram must have a prose walkthrough. Every concept must be explained in complete sentences, not bullet shorthand.

2. **Depth over breadth**: Each file should be a comprehensive mini-article. A reader should finish one file and genuinely understand the topic at an advanced level.

3. **Narrative tone**: Write as a senior engineer explaining to a junior — conversational, specific, opinionated about tradeoffs, generous with real-world examples and actual numbers.

4. **Cross-referencing**: Concept files reference other concepts. Case studies reference concept files. Helper files reference both. All references use exact file numbers and names.

5. **Self-contained**: Each file is understandable on its own. Cross-references enhance learning but are not required for comprehension.

6. **Real numbers**: Include actual latency figures, throughput benchmarks, storage calculations, and scale numbers. Not "Redis is fast" but "Redis handles ~100K ops/sec on a single node."

## Verification

After each batch, verify:
- All files exist with correct naming
- All files follow their mandatory template exactly
- Cross-references point to correct file numbers and names
- Mermaid diagrams are syntactically valid
- Estimation math is arithmetically correct
- No section in any file is fewer than 3 sentences
- No placeholder or TODO text remains

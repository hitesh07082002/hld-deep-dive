# NotebookLM Study Guide for HLD Deep Dive

> How to use these 45 files with Google NotebookLM to generate audio overviews, videos, mindmaps, quizzes, and more for maximum learning.

## Setting Up Your Notebook

NotebookLM is most useful when you treat it as a study partner, not a passive summarizer. This corpus was written so NotebookLM can connect concepts, case studies, and helper files into one coherent learning graph. The best results come from selecting a focused subset of sources for each session instead of blindly throwing all forty-five files at every prompt.

### Option A: Single Notebook (Recommended for Most Users)

Upload all forty-five files into one notebook if your main goal is broad understanding and cross-topic synthesis. This works especially well when you want NotebookLM to connect concept files to the case studies where those ideas actually appear. A single notebook is also the easiest way to generate cross-cutting quizzes such as "compare how caching appears in the URL shortener, Instagram feed, and distributed cache case studies."

Benefits of one notebook:

- better concept-to-case-study linking;
- better mindmaps across themes like storage, communication, and reliability;
- easier comparison prompts;
- one place to revisit during later review weeks.

The tradeoff is that broad notebooks sometimes answer at a higher level than you want. When that happens, narrow the selected sources before generating an output instead of creating a whole new note immediately.

### Option B: Multiple Notebooks (For Focused Study)

If you prefer tighter study sessions, split the corpus like this:

- **Notebook 1:** Concepts `01-12` for foundations and the data layer
- **Notebook 2:** Concepts `13-25` for communication, reliability, and architecture
- **Notebook 3:** Case Studies `CS-01` to `CS-08`
- **Notebook 4:** Case Studies `CS-09` to `CS-15`
- **Notebook 5:** all helper files plus whichever concepts or case studies you are reviewing that week

This approach is especially useful if you want cleaner audio overviews or more targeted quiz generation. It also reduces the chance that NotebookLM answers a question with too much material from unrelated files.

## Generating Audio Overviews (Podcasts)

Audio overviews are one of the highest-value ways to use this kit. They are excellent for repetition during commutes, walks, or low-focus review time. The key is to choose source files deliberately.

### Best Prompts for Audio Generation

**For concept files:**

- "Generate an audio overview explaining Concept 10 Caching Strategies as if teaching it to a junior engineer. Focus on the problem it solves, the main write patterns, and the operational tradeoffs."
- "Create a podcast-style discussion about Concept 17 CAP Theorem & PACELC. Use concrete examples from the distributed rate limiter and flash sale case studies."
- "Explain how Concept 14 Message Queues & Stream Processing and Concept 15 Event-Driven Architecture & Event Sourcing relate to each other with real production examples."

**For case studies:**

- "Walk through CS-05 WhatsApp Chat System step by step, explaining each architectural decision and what would break if we removed that component."
- "Generate a discussion about the bottlenecks and scaling challenges in CS-12 Distributed Job Scheduler. Focus on timers, leases, retries, and overdue-task recovery."
- "Compare CS-03 Instagram News Feed and CS-08 Uber / Ride Matching. What is similar about their event flow and what is different about their real-time constraints?"

**For cross-cutting topics:**

- "Using TRADEOFFS-DECISION-MATRIX.md, explain when to choose SQL vs NoSQL with examples from CS-01, CS-03, and CS-14."
- "Walk through SYSTEM-DESIGN-FRAMEWORK.md using CS-01 URL Shortener as the running example."
- "Use README.md and NUMBERS-AND-ESTIMATION.md to create an interview-style estimation lesson based on the case studies in this repo."

### Tips for Better Audio

- Select specific source files before generating. Audio improves when the source set is intentional.
- For concept audio, select the concept file plus `1` or `2` related case studies.
- For case-study audio, select the case-study file plus `3` or `4` prerequisite concept files.
- Ask for tradeoffs explicitly. NotebookLM becomes much more useful when the prompt says "compare," "why," or "what breaks if."
- Regenerate if the output feels too surface-level. More specific prompts usually improve the second attempt.

## Generating Videos

Video generation works best when you scope tightly and ask for one narrative. The most effective videos usually explain a single concept, one case study, or one deliberate comparison.

### Best Approaches for Video Content

- Generate one-concept explainer videos for dense topics like caching, consensus, or observability.
- Generate one-case-study walkthrough videos for systems with a clear end-to-end story.
- Generate comparison videos when two or three files attack the same problem from different angles.

### Recommended Video Topics

1. **How Caching Works and Why It Matters**  
   Select: `concepts/10-caching-strategies.md`, `case-studies/CS-01-url-shortener.md`, `case-studies/CS-03-instagram-news-feed.md`

2. **Designing a Chat System from Scratch**  
   Select: `case-studies/CS-05-whatsapp-chat-system.md`, `concepts/13-sync-vs-async-communication.md`, `concepts/16-real-time-communication.md`, `concepts/20-idempotency-deduplication-exactly-once.md`

3. **How a Feed System Handles Celebrity Fanout**  
   Select: `case-studies/CS-03-instagram-news-feed.md`, `concepts/10-caching-strategies.md`, `concepts/14-message-queues-and-stream-processing.md`, `concepts/15-event-driven-architecture-and-event-sourcing.md`

4. **Search Is Not Just a Database Query**  
   Select: `concepts/24-search-systems.md`, `case-studies/CS-04-twitter-search.md`, `case-studies/CS-09-typeahead-autocomplete.md`

5. **How Global Video Delivery Really Works**  
   Select: `case-studies/CS-06-youtube-netflix-streaming.md`, `concepts/03-cdn-and-edge-computing.md`, `concepts/23-blob-object-storage-patterns.md`

6. **The Edge Layer: Gateways, Proxies, and Rate Limiters**  
   Select: `concepts/04-api-gateway-reverse-proxy-rate-limiting.md`, `case-studies/CS-10-distributed-rate-limiter.md`, `case-studies/CS-13-flash-sale-ticketmaster.md`

7. **When to Choose SQL vs NoSQL**  
   Select: `concepts/06-sql-databases-at-scale.md`, `concepts/07-nosql-deep-dive.md`, `concepts/12-data-modeling-for-scale.md`, `helpers/TRADEOFFS-DECISION-MATRIX.md`

8. **How Distributed Schedulers Avoid Losing Work**  
   Select: `case-studies/CS-12-distributed-job-scheduler.md`, `concepts/18-distributed-consensus.md`, `concepts/25-distributed-task-scheduling.md`

9. **Observability as a Product Feature**  
   Select: `concepts/21-monitoring-observability-slos.md`, `case-studies/CS-14-metrics-logging-system.md`

10. **Why Distributed Caches Need Consensus and Replication**  
    Select: `case-studies/CS-15-distributed-cache-redis.md`, `concepts/08-database-replication.md`, `concepts/11-consistent-hashing.md`, `concepts/18-distributed-consensus.md`

11. **Consistency Tradeoffs in Real Systems**  
    Select: `concepts/17-cap-theorem-and-pacelc.md`, `case-studies/CS-10-distributed-rate-limiter.md`, `case-studies/CS-13-flash-sale-ticketmaster.md`

12. **Real-Time Systems Compared: Chat, Docs, and Ride Matching**  
    Select: `case-studies/CS-05-whatsapp-chat-system.md`, `case-studies/CS-08-uber-ride-matching.md`, `case-studies/CS-11-google-docs-collaborative-editing.md`, `concepts/16-real-time-communication.md`

## Creating Quizzes

NotebookLM is strongest when you make quizzes narrow and scenario-driven. Do not ask for "a quiz on system design." Ask for a quiz on a phase, a tradeoff family, or a small set of case studies.

### Quiz Strategies

- After each study phase, select all files from that phase and ask for a ten-question review.
- Use `NUMBERS-AND-ESTIMATION.md` to generate back-of-envelope practice.
- Use `TRADEOFFS-DECISION-MATRIX.md` to generate "when would you choose X over Y" scenarios.
- After a case study, ask for failure-mode questions, not just fact recall.
- Re-run the quiz one week later with the same source set for spaced repetition.

### Sample Quiz Prompts

- "Create a 10-question quiz testing understanding of the data-layer concepts in files 06 through 12. Include both factual and scenario-based questions."
- "Generate 5 system-design estimation problems based on this corpus, then provide worked solutions after the questions."
- "Create a quiz comparing the architectures of CS-01 URL Shortener, CS-03 Instagram News Feed, and CS-06 YouTube / Netflix Video Streaming."
- "Give me 8 tradeoff questions where I must choose between synchronous and asynchronous communication, strong and eventual consistency, or cache-aside and write-through."
- "Generate a quiz on reliability and failure recovery using Concepts 17 through 21 and CS-12 through CS-15."

## Creating Mindmaps

Mindmaps are best for relationships, not details. They help you see how the same primitives recur across different systems.

Use `helpers/README.md` as the anchor source because it already contains the concept-to-case-study mapping table and prerequisite structure. Then add a focused theme set of `4` to `6` related files.

Good mindmap prompts:

- "Create a mindmap showing how data-layer concepts 06 through 12 connect to CS-01, CS-03, CS-04, and CS-15."
- "Create a mindmap of communication and reliability concepts 13 through 21 with labeled relationships and case-study examples."
- "Build a mindmap showing how helper files connect to the concept corpus and which file to use for estimation, tradeoffs, workflow, and review."

## Suggested Study Schedule

### Week 1-2: Foundation Concepts

- Day 1-2: read and listen to Concepts `01-03` on scaling, load balancing, and CDN/edge behavior.
- Day 3-4: study Concepts `04-05` on gateways, rate limiting, and API contracts.
- Day 5: ask NotebookLM for a ten-question quiz on Concepts `01-05`.
- Day 6-7: study `CS-01 URL Shortener` as the first concrete application of the foundation layer.

### Week 3-4: Data Layer Deep Dive

- Day 1-2: study Concepts `06-08` on SQL, NoSQL, and replication.
- Day 3-4: study Concepts `09-12` on sharding, caching, consistent hashing, and data modeling.
- Day 5: generate one estimation session from `NUMBERS-AND-ESTIMATION.md` focused on storage and QPS.
- Day 6-7: study `CS-02 Pastebin`, `CS-03 Instagram News Feed`, and `CS-04 Twitter Search` with the relevant data-layer concepts selected beside them.

### Week 5-6: Communication & Reliability

- Day 1-2: study Concepts `13-16` on sync/async, queues, event-driven design, and real-time communication.
- Day 3-4: study Concepts `17-21` on consistency, consensus, fault tolerance, idempotency, and observability.
- Day 5: generate an audio overview comparing queue-based and real-time systems.
- Day 6-7: study `CS-05 WhatsApp Chat System`, then revisit `CS-03` and `CS-04` to see how the same ideas appear differently.

### Week 7-8: Architecture + Intermediate Case Studies

- Day 1-2: study Concepts `22-25` on service boundaries, object storage, search, and workflow orchestration.
- Day 3: study `CS-06 YouTube / Netflix Video Streaming`.
- Day 4: study `CS-07 Notification System`.
- Day 5: study `CS-08 Uber / Ride Matching`.
- Day 6: study `CS-09 Typeahead / Autocomplete`.
- Day 7: study `CS-10 Distributed Rate Limiter`.

### Week 9-10: Advanced Case Studies + Review

- Day 1: study `CS-11 Google Docs / Collaborative Editing`.
- Day 2: study `CS-12 Distributed Job Scheduler`.
- Day 3: study `CS-13 E-commerce Flash Sale / Ticketmaster-Style Drop`.
- Day 4: study `CS-14 Metrics & Logging System`.
- Day 5: study `CS-15 Distributed Cache (Redis-Like)`.
- Day 6: use `SYSTEM-DESIGN-FRAMEWORK.md` to attempt one of the case studies from memory before reopening the source file.
- Day 7: generate one cross-cutting quiz using the helper files plus your weakest two case studies.

## Tips for Maximum Retention

- Use audio overviews for repetition, not for first exposure to the most complex topics.
- After listening to a concept audio, explain the idea out loud in your own words before moving on.
- For case studies, pause after the requirements section and sketch the design yourself before listening to NotebookLM's synthesis.
- Revisit `TRADEOFFS-DECISION-MATRIX.md` weekly. It is one of the highest-density files in the helper set.
- Use `NUMBERS-AND-ESTIMATION.md` for short practice bursts. Five minutes of estimation practice is more useful than passive rereading.
- Ask NotebookLM to compare systems, not just summarize them. Comparison forces the model to surface tradeoffs.
- Keep a short log of the concepts you consistently miss in quizzes, then re-study the case studies where those concepts are applied.
- Use Claude Code or another coding assistant after each phase to run mock interview questions or targeted quizzes.

## Best File Selection Patterns

When in doubt, use these source bundles:

- **Learn one concept deeply:** one concept file plus one matching case study
- **Review one case study well:** one case study plus its top `3` or `4` prerequisite concepts
- **Practice estimation:** `NUMBERS-AND-ESTIMATION.md` plus any one case study
- **Practice tradeoffs:** `TRADEOFFS-DECISION-MATRIX.md` plus two case studies with different design shapes
- **Practice interview structure:** `SYSTEM-DESIGN-FRAMEWORK.md` plus one case study you have not revisited recently
- **Build a big-picture map:** `README.md` plus `5` to `6` files from one theme

The main rule is to control scope. NotebookLM is strongest when you tell it exactly which slice of the corpus to reason over and what kind of output you want back.

# Numbers Every Engineer Should Know

> Reference sheet for back-of-envelope estimation in system design.

## Latency Numbers

Treat these as rounded heuristics, not sacred constants. Hardware, kernel tuning, storage media, network path, and workload shape all change the exact values. The point is to remember the scale difference between operations. A remote network round trip is not a little slower than RAM. It is orders of magnitude slower.

### Storage Latencies

| Operation | Typical Latency |
|-----------|-----------------|
| L1 cache reference | `0.5 ns` |
| Branch mispredict | `5 ns` |
| L2 cache reference | `7 ns` |
| Mutex lock/unlock | `25 ns` |
| Main memory (RAM) reference | `100 ns` |
| Compress 1 KB in memory | `10 us` |
| SSD random read | `150 us` |
| SSD random write | `300 us` to `1 ms` |
| HDD sequential read | `1 ms` to `2 ms` per MB-scale block |
| HDD random seek / read | `10 ms` |

The practical takeaway is simple: memory is cheap in latency terms, local disks are manageable, and random spinning-disk access is painful. That is why caches, sequential writes, append logs, and precomputed indexes show up everywhere.

### Network Latencies

| Operation | Typical Latency |
|-----------|-----------------|
| Same-host kernel/network stack hop | `5 us` to `20 us` |
| Same rack | `50 us` to `100 us` |
| Same datacenter one-way packet send | `0.2 ms` to `0.5 ms` |
| Same datacenter round trip | `0.5 ms` |
| Cross-availability-zone round trip | `1 ms` to `3 ms` |
| US East to US West | `60 ms` to `80 ms` |
| US to Europe | `70 ms` to `100 ms` |
| US to Asia | `120 ms` to `180 ms` |
| Europe to Asia | `140 ms` to `220 ms` |

### Application-Level References

| Operation | Typical Latency |
|-----------|-----------------|
| Redis `GET` in-region | `0.2 ms` to `1 ms` |
| Simple SQL query on warm index | `1 ms` to `10 ms` |
| Medium SQL query with joins | `10 ms` to `50 ms` |
| Internal service-to-service RPC | `1 ms` to `10 ms` |
| External third-party API call | `50 ms` to `500+ ms` |
| CDN edge cache hit | `5 ms` to `30 ms` |
| Origin fetch across regions | `100 ms` to `300+ ms` |

### Key Insight

The big lesson is multiplicative pain. A local Redis lookup might be under `1 ms`. A database hit might be `10 ms`. A cross-region dependency hop might be `100 ms`. Once a request path includes three remote services and two databases, your p99 latency can explode even if each individual dependency looks "fine" in isolation. This is why caching, denormalized read models, and asynchronous handoff are so powerful: they move work from slower, more failure-prone layers into faster ones or out of the synchronous user path entirely.

## Throughput Numbers

These are also rough production heuristics. Query shape, payload size, connection pooling, and hardware matter enormously.

### Database Throughput

| System | Rough Throughput |
|--------|------------------|
| Single PostgreSQL instance, simple indexed queries | `10,000` to `30,000 QPS` |
| Single MySQL instance, simple indexed queries | `10,000` to `30,000 QPS` |
| Single Redis instance | `100,000+ ops/sec` |
| Single Memcached instance | `100,000` to `200,000 ops/sec` |
| Single MongoDB instance | `10,000` to `50,000 QPS` depending on query shape |
| Single Cassandra node | `5,000` to `10,000 writes/sec` |
| Elasticsearch or OpenSearch ingest node | highly workload-dependent, often `10,000+ docs/sec` with tuning |

Use these only as sanity checks. "One Postgres box can handle it" is a plausible statement at `3,000 QPS`. It is not a plausible statement at `500,000 QPS` unless most of that traffic is absorbed somewhere else.

### Web Server Throughput

| System | Rough Throughput |
|--------|------------------|
| Single Node.js app, simple JSON responses | `10,000` to `50,000 req/sec` |
| Single Go service, simple JSON or gRPC responses | `50,000` to `100,000 req/sec` |
| Single NGINX reverse proxy | `100,000+ req/sec` |
| Single API gateway or service mesh proxy sidecar | tens of thousands of req/sec, heavily config-dependent |

The moment the handler does real work, these numbers drop. Authentication checks, SQL calls, compression, template rendering, and fanout all lower effective throughput.

### Network Throughput

| Link Speed | Approximate Payload Throughput |
|------------|--------------------------------|
| `100 Mbps` | `12.5 MB/sec` |
| `1 Gbps` | `125 MB/sec` |
| `10 Gbps` | `1.25 GB/sec` |
| `25 Gbps` | `3.125 GB/sec` |
| `100 Gbps` | `12.5 GB/sec` |

Remember the difference between bits and bytes. Interviews and design reviews get this wrong constantly.

## Size Reference Numbers

### Data Sizes

| Item | Typical Size |
|------|--------------|
| ASCII character | `1 byte` |
| UTF-8 character, average text | `2` to `3 bytes` |
| Boolean | `1 byte` logically, often more in storage layouts |
| 32-bit integer | `4 bytes` |
| 64-bit integer / Unix timestamp | `8 bytes` |
| UUID | `16 bytes` |
| IPv4 address | `4 bytes` |
| IPv6 address | `16 bytes` |
| Average URL | `80` to `100 bytes` |
| Short social post / tweet-like payload | `200` to `300 bytes` |
| Email body with metadata | `50 KB` to `100 KB` |
| Compressed JPEG photo | `200 KB` to `500 KB` |
| Mobile app image thumbnail | `20 KB` to `80 KB` |
| One minute of HD video | `100 MB` to `150 MB` |
| One minute of 4K video | `300 MB` to `600 MB` |

### Scale Reference

| Quantity | Rough Equivalent |
|----------|------------------|
| `1 million` seconds | `11.6 days` |
| `10 million` seconds | `115.7 days` |
| `1 billion` seconds | `31.7 years` |
| Seconds in a day | `86,400` |
| Seconds in a month | about `2.5 million` |
| Seconds in a year | about `31.5 million` |

These conversions matter more than they seem to. Storage estimates go wrong because people forget how quickly daily numbers become yearly numbers.

## Estimation Templates

### Template 1: QPS from DAU

Use this when the prompt gives you users and actions instead of explicit request rates.

```text
Given:
X = daily active users
Y = actions per user per day

Average QPS = (X x Y) / 86,400
Peak QPS = Average QPS x peak multiplier
```

Worked example:

```text
10,000,000 DAU
5 actions/user/day

Average QPS = (10,000,000 x 5) / 86,400
            = 578.70

Peak QPS = 578.70 x 3
         = 1,736.11
```

If the system has a strong daily burst, such as a flash sale or office-hours product, use a higher multiplier than `3x`.

### Template 2: Storage Estimation

Use this when you know object size and ingestion rate.

```text
Per-record size = S bytes
Records per day = N
Retention period = R days

Daily storage = S x N
Total storage = Daily storage x R
Replicated storage = Total storage x replication factor
```

Worked example:

```text
Post metadata = 1 KB
120,000,000 posts/day
365 days retention

Daily storage = 1 KB x 120,000,000
              = 120 GB/day

Yearly storage = 120 GB x 365
               = 43.8 TB

Replication factor 3:
43.8 TB x 3 = 131.4 TB
```

Always separate metadata from blobs. The metadata store for a photo app is a different sizing problem from the image bytes themselves.

### Template 3: Bandwidth Estimation

Use this for API egress, media transfer, or inter-service traffic.

```text
Payload size = P bytes
QPS = Q

Bandwidth per second = P x Q
Bandwidth per second in MB = (P x Q) / 1,048,576
```

Worked example:

```text
Feed page payload = 20 KB
Peak QPS = 50,000

Bandwidth = 20 KB x 50,000
          = 1,000,000 KB/sec
          = 976.56 MB/sec
```

If both request and response matter, estimate both sides. Internal services often move more bytes in responses than people expect.

### Template 4: Cache Size Estimation

Use this when only part of the dataset is hot.

```text
Hot users or hot objects = H
Items per user or item copies = I
Average cached payload = P

Cache size = H x I x P
Add headroom for metadata and fragmentation = x 1.2 to x 1.5
```

Worked example:

```text
10,000,000 hot users
3 cached pages/user
20 KB/page

Raw cache = 10,000,000 x 3 x 20 KB
          = 600,000,000 KB
          = 572.2 GB

With 30% headroom:
572.2 GB x 1.3 = 743.9 GB
```

This is why most caches are selective. You usually cache the hot slice, not every possible object for every user.

### Template 5: Number of Servers Needed

Use this when you know peak throughput and rough per-node capacity.

```text
Peak QPS or throughput = T
Safe per-node capacity = C

Nodes needed = T / C
Practical nodes = Nodes needed x redundancy/headroom factor
```

Worked example:

```text
Peak QPS = 180,000
Safe node capacity = 15,000 QPS

Raw nodes = 180,000 / 15,000
          = 12

With 50% headroom for failover and bursts:
12 x 1.5 = 18 nodes
```

Do not size to average. Production systems fail at peak.

## Quick Rules of Thumb

- The first goal of estimation is to identify the bottleneck, not to win a calculator contest.
- `80/20` is a useful default: `80%` of traffic often hits `20%` of the data.
- For read-heavy systems, replicas and caches are usually the first scaling moves.
- For write-heavy systems, sharding and log-based ingestion matter earlier.
- If the whole hot dataset fits comfortably in memory, you may not need a distributed cache yet.
- If media bytes dominate bandwidth, move them to object storage and a CDN early.
- If you need global traffic but not global writes, keep write ownership regional and replicate outward.
- A queue smooths bursts, but it does not remove the need for idempotent consumers.
- A cache hit ratio below about `80%` is often a sign that you are caching the wrong objects or using the wrong TTL.
- A single hot key can defeat a well-sharded system if all traffic lands on one partition.
- Stronger consistency almost always costs either latency, availability under partition, or both.
- If a request path crosses many services, your p99 is driven by the slowest dependency and retry behavior, not the average one.
- Keep at least `20%` to `50%` operational headroom for failover, rebalancing, or bursts.
- Always estimate storage with replication and retention included. Raw data size is never the deployed footprint.
- If you cannot explain your numbers in one minute, your estimate is too detailed for early design work.

## Practice Problems

### 1. Estimate storage for tweets over one year

Assume:

- `300M` tweets/day
- `300 bytes` average logical tweet payload
- replication factor `3`

Solution:

```text
Daily storage = 300,000,000 x 300 bytes
              = 90,000,000,000 bytes
              = 83.82 GB/day

Yearly storage = 83.82 GB x 365
               = 30.59 TB

With replication factor 3:
30.59 TB x 3 = 91.77 TB
```

That is only tweet payload. Media, indexes, and search copies would be much larger.

### 2. Estimate upload bandwidth for a video platform

Assume:

- `2M` video uploads/day
- average upload size `200 MB`

Solution:

```text
Daily upload volume = 2,000,000 x 200 MB
                    = 400,000,000 MB/day
                    = 390.63 TB/day

Average throughput = 390.63 TB / 86,400
                   = 4.52 GB/sec

Peak throughput at 3x:
4.52 GB/sec x 3 = 13.56 GB/sec
```

That already tells you upload ingress is a first-class subsystem.

### 3. Estimate cache size for hot feed pages

Assume:

- `8M` highly active users
- `2` cached pages per user
- `25 KB` per page

Solution:

```text
Cache size = 8,000,000 x 2 x 25 KB
           = 400,000,000 KB
           = 381.47 GB

With 25% headroom:
381.47 GB x 1.25 = 476.84 GB
```

This is large but completely reasonable for a distributed Redis cluster.

### 4. Estimate web servers for a URL shortener redirect path

Assume:

- peak redirect load `240,000 req/sec`
- each server safely handles `20,000 req/sec`

Solution:

```text
Raw servers = 240,000 / 20,000
            = 12

With 50% headroom:
12 x 1.5 = 18 servers
```

That is the app tier only. The database, cache, and load balancers need their own capacity plans.

### 5. Estimate scheduler dispatch pressure

Assume:

- `500M` task executions/day
- `3x` peak multiplier
- `1 KB` payload per dispatch

Solution:

```text
Average task starts/sec = 500,000,000 / 86,400
                        = 5,787.04

Peak task starts/sec = 5,787.04 x 3
                     = 17,361.11

Peak dispatch bandwidth = 17,361.11 x 1 KB
                        = 16.95 MB/sec
```

This is a great example of why coordination, leasing, and retries can be harder than raw bandwidth.

## Closing Principle

The best estimators are not the people with the longest memorized tables. They are the people who can turn a vague prompt into a handful of high-signal numbers quickly, then use those numbers to make better architecture decisions. If your estimation tells you where latency lives, what the hot path is, and which stateful component will break first, you already won.

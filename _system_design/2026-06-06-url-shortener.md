---
layout: system-design
title: "Design a URL Shortener"
date: 2026-06-06
---

## Requirements

### Functional
- Given a long URL, generate a short URL (e.g., `https://short.ly/abc123`)
- Given a short URL, redirect to the original long URL
- Optional: custom aliases, expiration dates, click analytics

### Non-Functional
- Reads vastly outnumber writes (100:1 ratio assumed)
- Redirect latency < 50ms at p99
- 100M URLs stored, ~1B redirects/day at scale

### Out of scope for this exercise
- User accounts, authentication
- Analytics beyond basic click counts

## High-Level Design

```
Client → Load Balancer → Shortening Service → [ID Generator, Cache, DB]
Client → Load Balancer → Redirect Service → [Cache, DB]
```

Separate the write path (shortening) from the read path (redirect). Reads are hot; writes are cold.

## Components

**ID Generator** — Produces short, unique IDs. Options:
- Base62 encoding of an auto-increment integer: simple, sequential (guessable), no coordination needed for single DB
- MD5/SHA hash truncated to 7 chars: collision risk at scale, requires collision detection
- Distributed ID (Snowflake): globally unique, not sequential, no single point of failure

*Choice: Base62 of auto-increment for simplicity. Add distributed ID generation (Snowflake pattern) if sharding is required.*

**Storage** — One row per URL: `id, short_code, long_url, created_at, expires_at`. PostgreSQL for the primary store. Reads are by primary key (`short_code`), which is fast.

**Cache** — Redis in front of the DB for the redirect path. Cache short_code → long_url. TTL matches URL expiration or 24h default. Cache hit rate expected to be high given power-law distribution of URL access patterns.

**Redirect Service** — Stateless. Looks up short_code in cache, falls back to DB, returns HTTP 301 (permanent) or 302 (temporary if analytics needed).

## Trade-offs

| Decision | Alternative | Why this |
|---|---|---|
| 301 redirect | 302 redirect | 301 is cached by browsers, reducing server load. Use 302 if click counting matters. |
| PostgreSQL | NoSQL (Cassandra) | Simpler operationally. Sharding is not needed until ~1B rows. |
| Redis cache | No cache | Redirect latency requirement (<50ms) is not achievable at scale without caching. |
| Base62 IDs | Random hash | Avoids collision detection complexity. Predictable ID length (7 chars = 62^7 ≈ 3.5T URLs). |

## Scaling Bottlenecks

1. **DB writes**: Not a bottleneck at this scale (few writes vs. many reads).
2. **DB reads**: Solved by Redis cache. Cache miss rate drives DB read load.
3. **ID generation**: Auto-increment becomes a bottleneck with DB sharding. Switch to Snowflake IDs at that point.
4. **Hot URLs**: Power-law distribution means a small number of URLs get most traffic. Redis handles this naturally — hot keys stay in memory.

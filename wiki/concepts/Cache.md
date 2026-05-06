---
aliases: ["caching layer", "in-memory cache"]
tags: [caching]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Cache

A fast, typically in-memory store sitting between the application and a slower backing store (usually the database). Frequently accessed data is held in the cache so most reads never touch the slower store.

## How it works

The pattern in the source is **read-through**: on a request, the app checks the cache; if present, return it; if absent, read from the database, populate the cache, then return.

```
request → app → cache hit? → yes → return
                          → no  → DB → populate cache → return
```

Four practical concerns the source flags as non-negotiable:

1. **When to use it** — caching is right when reads dominate writes and data changes infrequently. Caching write-heavy or rapidly-changing data inverts the cost.
2. **Expiration policy (TTL)** — too long and clients see stale data; too short and the cache rarely amortizes its DB hit. There is no universally correct TTL; it depends on tolerance for staleness.
3. **Consistency between cache and store** — every write path must keep the two in sync, or read-through will keep returning stale values until TTL expires. Cross-region multi-DC consistency is materially harder.
4. **SPOF mitigation** — a single cache node is a [[Single Point of Failure]]; scale it out across multiple servers and overprovision memory so a single eviction storm doesn't melt the DB.

**Eviction policy** governs what gets dropped when the cache fills. LRU (least-recently-used) is the default; LFU and FIFO suit different access patterns.

## When to use

- Read-heavy workloads where the same hot keys are requested repeatedly.
- Expensive computed results that are stable across many requests.
- In front of any backing store whose latency or cost you want to amortize.

When **not** to use: write-dominated paths, data with strict freshness requirements (a stale read is a correctness bug), or workloads with no locality (every request asks for a different key — cache hit rate stays near zero).

## Related

- [[CDN]] — same caching idea applied at the network edge for static assets.
- [[Database Replication]] — both reduce read pressure on the primary; a cache absorbs the hottest reads, replicas absorb the rest.
- [[Single Point of Failure]] — a single-node cache is a SPOF and a thundering-herd risk on failure.

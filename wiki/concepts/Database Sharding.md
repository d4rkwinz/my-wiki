---
aliases: ["sharding", "horizontal partitioning", "DB sharding"]
tags: [databases]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Database Sharding

Partitioning a single logical database across multiple physical database servers, where each server (a **shard**) holds a disjoint subset of the rows. Unlike replication — which puts the same data on every node — sharding splits the data so any one node holds only its slice.

## How it works

A [[Sharding Key]] (e.g. `user_id`) is chosen, and a routing function maps each row to a shard. The classic example from the source is `shard = user_id % N` for N shards.

```
write(user_id=42)  → shard(42 % 4) = shard 2
write(user_id=99)  → shard(99 % 4) = shard 3
read (user_id=42)  → shard 2
```

The application (or a proxy) computes the shard from the key on every query. Each shard runs as an independent database — its own replication, its own backups, its own scaling.

The source is explicit that sharding is **far from a perfect solution**. Three hard problems:

1. **Resharding** — when a shard fills up, you have to redistribute data. With naive `key % N`, changing N rehashes nearly every row. The standard mitigation is **consistent hashing**, which only moves a small fraction of keys when shards are added or removed.
2. **Hotspot keys** ("celebrity problem") — if one key (e.g. a celebrity's `user_id`) attracts disproportionate traffic, its shard gets crushed while others idle. Mitigation: give hotspot keys their own dedicated shards, or split their data further.
3. **Lost cross-shard joins** — SQL joins assume one database. Across shards, joins become application-level fan-out queries, which are slow and complex. Standard workaround: **denormalize** — duplicate the data needed for the join into each shard so the query stays single-shard.

## When to use

- Write-bound workloads that have outgrown what a single primary can absorb. Replication scales reads; sharding is the primary tool for writes.
- Datasets too large to fit on one machine even after vertical scaling.
- Workloads that can choose a sharding key with naturally balanced traffic and minimal cross-shard queries.

When **not** to use: anything that fits on one machine. Sharding is operationally expensive — every cross-shard operation, every schema migration, every analytics query gets harder. Exhaust replication, caching, and vertical scaling first. The source's framing: sharding introduces "complexities and new challenges" — adopt it because you must, not because you can.

## Related

- [[Sharding Key]] — the choice of key determines whether sharding helps or hurts; treated as its own concept.
- [[Database Replication]] — orthogonal: shards are usually each replicated for HA. Replication scales reads, sharding scales writes; production systems use both.
- [[Single Point of Failure]] — a shard without replication makes its slice of users a SPOF. Always replicate each shard.

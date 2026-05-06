---
aliases: ["partition key", "shard key"]
tags: [databases]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Sharding Key

The column (or composite of columns) used to decide which shard a given row lives on. The sharding key is the single most consequential decision in [[Database Sharding]]: it determines whether load distributes evenly or one shard burns while others idle, and whether a typical query hits one shard or fans out across all of them.

## How it works

A routing function maps the key value to a shard:

```
shard_id = hash(sharding_key) mod N    # naive
shard_id = consistent_hash(sharding_key)   # production
```

A good sharding key has three properties:

1. **High cardinality** — many distinct values. Sharding by `country_code` gives ~250 buckets; sharding by `user_id` gives billions. Low cardinality means too few buckets to balance well.
2. **Even distribution** — values are roughly uniformly accessed. `user_id` is usually fine; `signup_date` concentrates new traffic on the newest shard.
3. **Query alignment** — the queries you actually run can be answered by hitting a single shard. If your hottest query is "all orders for user X," shard by `user_id`. If you instead shard by `order_id`, that query fans out to every shard and sharding has made things worse, not better.

The failure mode the source highlights is the **hotspot key** ("celebrity problem"): a single value of the sharding key that attracts disproportionate traffic. `user_id` for a celebrity user, `tenant_id` for a giant customer in a multi-tenant system. The shard holding that key gets crushed. Standard mitigation: detect the hotspot and give it dedicated capacity — its own shard, or further sub-sharding within.

## When to use

This isn't really a "use it or not" concept — once you've decided to shard, you must pick a key. The choice is: **which** key.

- Use **`user_id`** (or analogous primary entity ID) when most queries are scoped to one user/tenant.
- Use **a composite key** when no single column has enough cardinality but a combination does (e.g. `tenant_id + user_id`).
- **Avoid** time-based keys (`created_at`, `signup_date`) for OLTP workloads — newest shard becomes the hotspot. Time-based partitioning is fine for analytics/archival workloads where queries are scan-heavy and writes are spread by ingestion process.

## Related

- [[Database Sharding]] — the parent concept; sharding key is the key choice within it.
- [[Database Replication]] — shards each typically have their own replicas; the sharding key has no role in replication, but understanding both is required to reason about a sharded+replicated cluster.

# Core-Truth Registry

Canonical concept registry. The single artifact answering "what does this wiki claim to know."

Updated on every Phase-2 promote. See root `CLAUDE.md` → "Core-Truth maintenance."

**Format** (one bullet per concept, no sections, no tables):

```
- [[Concept Name]] — one-line definition. Sources: N.
```

## Concepts

- [[Load Balancer]] — fronts a server pool via a public IP, distributes traffic, and routes around failures. Sources: 1.
- [[Database Replication]] — copying a primary database's writes to replicas for read scaling, reliability, and HA failover. Sources: 1.
- [[Cache]] — fast in-memory store between the app and a slower backing store, holding frequently accessed data. Sources: 1.
- [[CDN]] — geographically distributed cache of static assets served from the edge nearest the user. Sources: 1.
- [[Stateless Web Tier]] — web servers holding no client state, allowing any request to hit any server. Sources: 1.
- [[Multi-Data-Center Architecture]] — running the system in multiple data centers with geoDNS routing and async cross-DC data sync. Sources: 1.
- [[Message Queue]] — durable async buffer between producers and consumers, decoupling them so each scales and fails independently. Sources: 1.
- [[Database Sharding]] — partitioning a logical database across multiple physical servers, each holding a disjoint slice of the rows. Sources: 1.
- [[Sharding Key]] — the column(s) that route a row to its shard; must be high-cardinality, evenly distributed, and aligned with hot queries. Sources: 1.
- [[Single Point of Failure]] — any component whose failure halts the system; eliminated by redundancy at every tier. Sources: 1.

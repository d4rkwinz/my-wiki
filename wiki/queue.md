# Concept Queue

Rolling list of deferred concept candidates — atoms named by past sources but not yet substantively covered. Each entry is a placeholder waiting for a deeper source to support a real concept note.

**Format** — `- [ ] [[Concept Name]] — one-line description. From: Source Title (YYYY-MM-DD).`

**Lifecycle** — entries are appended on every Phase 2 promote (the deferred candidates from the digest). When a future source promotes one, remove the entry from this file (or mark `[x]` and sweep periodically). The queue is the only persistent home for these candidates after the originating digest is deleted.

---

## Deferred candidates

- [ ] [[DNS Resolution]] — domain-to-IP lookup performed by clients before any HTTP connection. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Web Tier]] — the layer of servers handling client requests and orchestrating calls to data + cache tiers. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Data Tier]] — the persistent storage layer, kept separate from the web tier so each scales independently. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Relational Database]] — RDBMS storing data in tables/rows with SQL joins; default unless a specific need pushes you off it. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[NoSQL Database]] — non-relational store (key-value, document, column, graph) chosen for low latency, unstructured data, or massive scale. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Vertical Scaling]] — adding CPU/RAM/disk to one machine; simple but bounded and SPOF-prone. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Horizontal Scaling]] — adding more machines to a pool; the only path past hardware limits. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Master-Slave Replication]] — one writable master, many read-only slaves; slave promoted on master failure. (Currently subsumed under [[Database Replication]] aliases; split out when a source contrasts replication topologies.) From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Read-Through Cache]] — strategy where the app checks cache first, falls back to DB, populates cache on miss. (Needs a source comparing it to cache-aside, write-through, write-back, write-around.) From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Cache Eviction Policy]] — rule for evicting items when full: LRU (default), LFU, FIFO. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Cache Expiration Policy]] — TTL controlling freshness; long → stale, short → DB hammering. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Sticky Session]] — LB feature pinning a client to one server; alternative (and inferior) to externalizing state. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Session Store]] — externalized storage holding session state so the web tier can stay stateless. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Auto Scaling]] — automatic add/remove of servers based on load; requires a stateless web tier. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[GeoDNS]] — DNS service resolving to different IPs based on the requester's location. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Consistent Hashing]] — hashing scheme minimizing data movement on shard add/remove. (Named-not-explained in source; needs a dedicated treatment.) From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Hotspot Key]] — a sharding key whose value dominates traffic and overloads one shard (the "celebrity problem"). From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Denormalization]] — duplicating data across tables/shards to avoid cross-shard joins. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Logging]] — recording events (especially errors); aggregate centrally at scale. From: Scale From Zero To Millions Of Users (2026-05-03).
- [ ] [[Metrics]] — quantitative signals at three levels: host, aggregated tier, business KPIs. From: Scale From Zero To Millions Of Users (2026-05-03).

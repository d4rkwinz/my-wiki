---
source: "[[raw/Scale From Zero To Millions Of Users]]"
status: digested
read_on: 2026-05-03
---

## TL;DR

A walkthrough of evolving a web system from a single server to one serving millions of users by progressively layering well-known scaling primitives. Each step targets a specific failure mode of the previous: a single box has no redundancy → load balancer + replicated DB; reads dominate cost → cache + CDN; sticky sessions block elasticity → stateless web tier with externalized session store; one data center is a SPOF → multi-DC with geoDNS; tight coupling blocks independent scaling → message queues; the data tier itself becomes the bottleneck → sharding. Core thesis: large-scale systems are assembled from layered, decoupled, individually scalable primitives — not rewritten from one big design.

## Key claims

- Browsers/mobile clients resolve a domain via DNS, then send HTTP to the returned IP — Section "Single server setup"
- Splitting web tier from data tier is the first move because it lets each scale independently — Section "Database"
- NoSQL is the right choice when latency is critical, data is unstructured, only serialize/deserialize is needed, or data volume is massive; otherwise relational is the default — Section "Which databases to use?"
- Vertical scaling is bounded by hardware and offers no failover; horizontal scaling is mandatory beyond a certain size — Section "Vertical vs horizontal scaling"
- A load balancer takes the public IP, hides web servers behind private IPs, and provides both failover and elastic capacity — Section "Load balancer"
- Master-slave DB replication scales reads (more slaves than masters), improves reliability via geographic copies, and provides HA via slave-to-master promotion (with caveats: slave data may be stale, recovery scripts needed) — Section "Database replication"
- Caches help when reads dominate; the four practical concerns are when-to-use, expiration TTL, store-cache consistency, and SPOF mitigation via multi-server + memory overprovisioning — Section "Cache"
- LRU is the default eviction policy; LFU and FIFO suit different access patterns — Section "Cache" (Eviction Policy)
- CDNs serve static assets from geographically nearer edges; key knobs are TTL, cost, fallback to origin, and invalidation (API or versioned URLs) — Section "CDN"
- Sticky sessions tie a client to one server and complicate scaling and failure handling; the alternative is externalizing session state to a shared store (DB / Memcached / Redis / NoSQL), which makes the web tier stateless and trivially auto-scalable — Section "Stateless web tier"
- Multi-DC architectures use geoDNS to route by location; the hard parts are cross-DC data sync (Netflix uses async multi-region replication) and consistent automated deployment — Section "Data centers"
- A message queue decouples producers from consumers via a durable async buffer, letting each side scale and fail independently — Section "Message queue"
- Operational maturity needs logging (centralized at scale), metrics at three levels (host, aggregated, business), and CI/CD automation — Section "Logging, metrics, automation"
- DB sharding partitions data by a sharding key (typically hashed, e.g. `user_id % N`); the sharding key must distribute load evenly — Section "Database scaling"
- Sharding's three hard problems: resharding when shards fill (use consistent hashing), hotspot/celebrity keys overloading one shard (give them dedicated shards), and lost cross-shard joins (workaround: denormalize) — Section "Database scaling"

## Concept candidates

Checklist for Phase 2 (Promote). Each becomes (or merges into) a `wiki/concepts/*.md`.

- [[DNS Resolution]] — domain-to-IP lookup performed by clients before any HTTP connection.
- [[Web Tier]] — the layer of servers handling client requests and orchestrating calls to data + cache tiers.
- [[Data Tier]] — the persistent storage layer, kept separate from the web tier so each scales independently.
- [[Relational Database]] — RDBMS storing data in tables/rows with SQL joins; default unless a specific need pushes you off it.
- [[NoSQL Database]] — non-relational store (key-value, document, column, graph) chosen for low latency, unstructured data, or massive scale.
- [[Vertical Scaling]] — adding CPU/RAM/disk to one machine; simple but bounded and SPOF-prone.
- [[Horizontal Scaling]] — adding more machines to a pool; the only path past hardware limits.
- [[Load Balancer]] — fronts a server pool via a public IP and distributes traffic, routing around failures.
- [[Database Replication]] — copying a primary's data to replicas for read scaling, reliability, and HA.
- [[Master-Slave Replication]] — one writable master, many read-only slaves; slave promoted on master failure.
- [[Cache]] — fast in-memory store for frequently accessed data, sitting between web and data tiers.
- [[Read-Through Cache]] — strategy where the app checks cache first, falls back to DB, populates cache on miss.
- [[Cache Eviction Policy]] — rule for evicting items when full: LRU (default), LFU, FIFO.
- [[Cache Expiration Policy]] — TTL controlling freshness; long → stale, short → DB hammering.
- [[Single Point of Failure]] — a component whose failure halts the whole system; defeated by redundancy.
- [[CDN]] — geographically distributed network caching static assets close to users.
- [[Stateless Web Tier]] — web servers holding no client state, allowing any request to hit any server.
- [[Sticky Session]] — LB feature pinning a client to one server; an alternative (and inferior) to externalizing state.
- [[Session Store]] — externalized storage holding session state so the web tier can stay stateless.
- [[Auto Scaling]] — automatic add/remove of servers based on load; requires a stateless web tier.
- [[GeoDNS]] — DNS service resolving to different IPs based on the requester's location.
- [[Multi-Data-Center Architecture]] — running the system in multiple DCs for availability and latency.
- [[Message Queue]] — durable async buffer between producers and consumers; enables decoupling and independent scaling.
- [[Database Sharding]] — partitioning a large DB across multiple servers by a sharding key.
- [[Sharding Key]] — column(s) used to route a query to the correct shard; must distribute load evenly.
- [[Consistent Hashing]] — hashing scheme minimizing data movement on shard add/remove.
- [[Hotspot Key]] — a sharding key whose value dominates traffic and overloads one shard (the "celebrity problem").
- [[Denormalization]] — duplicating data across tables/shards to avoid cross-shard joins.
- [[Logging]] — recording events (especially errors); aggregate centrally at scale.
- [[Metrics]] — quantitative signals at three levels: host, aggregated tier, business KPIs.

## Questions / gaps

- The chapter mentions multi-master and circular replication only by reference. Future digest needed on the consistency / conflict-resolution tradeoffs of those topologies.
- Only **read-through** caching is named. Cache-aside, write-through, write-back, and write-around are not compared — a dedicated caching source should fill this.
- Session-store technology choice is hand-waved across "relational / Memcached / Redis / NoSQL." When does each actually win? Latency vs durability vs cost.
- Multi-DC data synchronization is described as "asynchronous replication" via a Netflix blog link without discussing the consistency model (eventual? bounded staleness?). A real multi-DC source is needed.
- "Consistent hashing is commonly used" — stated but not explained. The actual algorithm is a separate concept worth its own atomic note from a different source.
- The closing line "decouple the system into individual services" gestures at microservices but doesn't elaborate. Treat as a forward-pointer, not a claim from this source.
- Stateful vs stateless server distinction is presented cleanly; in practice many systems are partially stateful (in-memory caches, connection pools). The chapter ignores this nuance.

## Quotes

> Keep web tier stateless / Build redundancy at every tier / Cache data as much as you can / Support multiple data centers / Host static assets in CDN / Scale your data tier by sharding / Split tiers into individual services / Monitor your system and use automation tools.

(The chapter's closing checklist — useful to keep verbatim as a high-level "north star" for the engineering wiki.)

> A single point of failure (SPOF) is a part of a system that, if it fails, will stop the entire system from working.

(Wikipedia definition cited in the source; canonical phrasing for the [[Single Point of Failure]] concept note.)

> Sharding is a great technique to scale the database but it is far from a perfect solution. It introduces complexities and new challenges to the system: Resharding data, Celebrity problem, Join and de-normalization.

(Useful framing for the [[Database Sharding]] note's "When NOT to use" / "Tradeoffs" section.)

---
aliases: ["Multi-DC", "Multi DC", "multi-region architecture", "multi-data-center"]
tags: [distributed-systems, operations]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Multi-Data-Center Architecture

A deployment that runs the system in two or more geographically separated data centers (or cloud regions) simultaneously. Users are routed to the closest DC for latency, and the loss of an entire DC does not take the system down.

## How it works

1. **GeoDNS** answers the public hostname with a different IP per requester location — west-coast users resolve to the west DC, etc.
2. Each DC runs the full stack: web tier, cache, database. The web tier is stateless (see [[Stateless Web Tier]]) so any DC can serve any request as long as it can reach the data tier.
3. **Cross-DC data sync** is the hard part. The source cites Netflix's approach as **asynchronous multi-region replication** — writes are accepted in the local DC and replicated to others in the background. This means each DC can briefly disagree about state.
4. **Failover**: if a DC is lost, geoDNS routes its traffic to the surviving DCs. The surviving DCs must have the capacity to absorb the failed DC's load (typical rule: each DC sized for N+1 capacity at peak).
5. **Deployment consistency** — every DC must run the same code and config. Without disciplined CI/CD, DCs drift, and a regional cutover surfaces the drift as outages.

The source is candid about the gap: it describes the topology but hand-waves the **consistency model**. Async replication means **eventual consistency** at best, and possibly conflict windows on writes that hit two DCs nearly simultaneously. A dedicated source on multi-region consistency is owed.

## When to use

- Availability requirements that exceed what one DC can provide (cloud-region outages do happen).
- Latency-sensitive global products where serving every user from one region is unacceptable.
- Compliance regimes that require data residency in specific regions.

When **not** to use: small or regional products where a single DC's availability is good enough. Multi-DC roughly doubles operational complexity and demands serious investment in deployment automation, observability, and data-sync tooling.

## Related

- [[Stateless Web Tier]] — multi-DC is impractical without it; you cannot easily replicate per-server in-memory state across regions.
- [[Database Replication]] — multi-DC is a special, harder case of replication: same primitives, but with WAN latency, partitions, and consistency questions.
- [[Single Point of Failure]] — multi-DC is the answer to "the data center itself is a SPOF."

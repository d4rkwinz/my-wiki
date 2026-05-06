---
aliases: ["SPOF", "single points of failure"]
tags: [distributed-systems, operations]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Single Point of Failure

> A single point of failure (SPOF) is a part of a system that, if it fails, will stop the entire system from working.

A SPOF is any component for which there is no redundant alternative — when it dies, everything depending on it dies with it. Removing SPOFs is the single organizing principle of high-availability system design: every layer of a real production architecture (load balancing, replication, multi-DC, redundant queues) exists primarily to eliminate the SPOFs of the layer beneath.

## How it works

A SPOF is identified by asking, of each component in the system: *if this dies right now, does the system still serve requests?* If the answer is no, it is a SPOF. Common examples:

- A **single web server** — fixed by horizontal scaling behind a [[Load Balancer]].
- A **single load balancer** — fixed by an active-passive LB pair, anycast IPs, or a managed multi-AZ LB.
- A **single primary database** — fixed by [[Database Replication]] with promotable replicas.
- A **single cache node** in front of a hot DB — its loss can melt the database under thundering-herd reads. Fixed by clustering and overprovisioning.
- A **single data center** — the whole DC is a SPOF for any system that doesn't run in [[Multi-Data-Center Architecture]].
- An **unreplicated message queue broker** — moves the SPOF rather than removes it.

The pattern: each fix introduces a new component (the LB, the replication system, the geoDNS layer), and that new component becomes the next candidate SPOF. SPOF elimination is recursive — at some level you accept the residual risk (e.g. "we trust the cloud provider's managed LB"), but you do so consciously.

## When to use

This is a lens, not a feature — apply it during design and during incident review:

- **At design time**: walk the request path and list every component. For each, ask the question above. Whatever survives the question without redundancy is a known accepted risk.
- **At incident review time**: when an outage happens, the post-mortem question "what single thing caused this" usually surfaces a SPOF the original design overlooked or accepted.

The source's closing checklist treats SPOF removal as one of eight non-negotiables: *"Build redundancy at every tier."*

## Related

- [[Load Balancer]] — exists primarily to remove the web server as a SPOF.
- [[Database Replication]] — exists primarily to remove the database as a SPOF.
- [[Multi-Data-Center Architecture]] — exists primarily to remove the data center itself as a SPOF.

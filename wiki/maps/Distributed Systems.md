# Distributed Systems

Patterns and primitives for systems that span multiple nodes: keeping servers stateless, decoupling components, surviving the loss of any single piece, and operating across data centers.

Parent: [[System Design]]

## Concepts

- [[Stateless Web Tier]] — web servers holding no client state, allowing any request to hit any server.
- [[Message Queue]] — durable async buffer between producers and consumers; enables decoupling and independent scaling.
- [[Multi-Data-Center Architecture]] — running the system in multiple DCs for availability and latency.
- [[Single Point of Failure]] — a component whose failure halts the whole system; defeated by redundancy.

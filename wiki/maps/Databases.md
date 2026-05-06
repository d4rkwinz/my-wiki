# Databases

The persistent storage layer: replication for read scaling and HA, partitioning when one node can't hold the data.

Parent: [[System Design]]

## Concepts

- [[Database Replication]] — copying a primary's data to replicas for read scaling, reliability, and HA.
- [[Database Sharding]] — partitioning a large DB across multiple servers by a sharding key.
- [[Sharding Key]] — column(s) used to route a query to the correct shard; must distribute load evenly.

---
aliases: ["DB Replication", "Master-Slave Replication", "Primary-Replica Replication"]
tags: [databases]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Database Replication

Continuously copying a primary database's data to one or more replicas so reads can be served from many nodes, the data survives the loss of any single node, and a replica can be promoted to take over writes if the primary fails.

## How it works

The classic topology is **master-slave** (also called primary-replica): one writable master and many read-only slaves.

- **Writes** go only to the master, which logs each change and streams it to slaves.
- **Reads** can fan out across slaves — and since most workloads are read-heavy, this is where the scaling comes from. The master:slave ratio is typically 1:N with N >> 1.
- **Failover**: if a slave dies, traffic shifts to remaining slaves (or temporarily to the master). If the master dies, a slave is promoted to master; in practice this needs a recovery script that replays the missing tail of the log and reconfigures the cluster.

Caveats from the source:
- Slaves can be **stale** — replication is asynchronous, so a read after a write may not see the write.
- Promotion is rarely a clean atomic event; expect a brief window where the system is degraded.

## When to use

- Read-heavy workloads (the common case for web apps).
- Reliability requirements — geographically distributed replicas survive a DC-level loss.
- HA requirements where you can tolerate a brief failover window but not data loss.

When **not** to use: write-bound workloads. Replication scales reads, not writes. Once writes saturate the master, the answer is [[Database Sharding]], not more replicas.

## Related

- [[Database Sharding]] — the scaling strategy for writes; orthogonal to replication and usually combined with it.
- [[Single Point of Failure]] — replication exists primarily to remove the database from the SPOF list.
- [[Load Balancer]] — same redundancy pattern applied at the web tier.

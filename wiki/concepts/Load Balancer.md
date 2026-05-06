---
aliases: ["LB", "load balancers"]
tags: [networking]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Load Balancer

A network component that fronts a pool of backend servers behind a single public IP and distributes incoming traffic across them. Clients only know the load balancer's address; the servers behind it sit on private IPs and are reachable only via the LB.

## How it works

1. DNS resolves the public hostname to the load balancer's IP.
2. The LB receives each request and selects a backend (round-robin, least-connections, weighted, or hash-based).
3. The LB forwards the request over the private network to the chosen server.
4. Health checks remove unresponsive servers from the pool; failed nodes stop receiving traffic until they recover.

```
client → DNS → LB (public IP) → [server A | server B | server C] (private IPs)
```

This setup gives two benefits at once: **failover** (a dead server is bypassed automatically) and **elastic capacity** (adding a server is just registering it with the LB).

## When to use

- Any time more than one backend is serving the same role — even two servers want an LB rather than DNS round-robin, because DNS gives you no health awareness.
- As the entry point for a [[Stateless Web Tier]], where any server can serve any request.
- In front of internal services (database proxies, cache clusters) where the same redundancy argument applies.

When **not** to use: a hobby/single-server deployment where the LB itself becomes a more complex [[Single Point of Failure]] than the thing it fronts. Production LBs are themselves redundant (active-passive pairs, anycast, or managed services).

## Related

- [[Stateless Web Tier]] — the LB only works if any request can hit any server, which requires statelessness.
- [[Single Point of Failure]] — a single LB is itself a SPOF; production deployments redundify the LB.
- [[Database Replication]] — same redundancy idea applied to the data tier.

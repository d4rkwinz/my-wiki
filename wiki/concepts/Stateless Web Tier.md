---
aliases: ["stateless servers", "stateless application tier"]
tags: [distributed-systems]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Stateless Web Tier

A web tier where no per-client state lives on any individual server. Any request from any client can be served by any server in the pool, because everything the server needs to handle a request is either in the request itself or fetched from a shared backing store.

## How it works

The contrast is **stateful**: a server holds session data (logged-in user, cart, in-progress workflow) in its own memory. The first time a client hits server A, A remembers them; subsequent requests must come back to A. This forces **sticky sessions** at the load balancer — the LB pins client → server — and the moment A fails or is removed, that client's state is gone.

Statelessness inverts this:

1. Session state moves to a **shared session store** — a relational DB, Memcached, Redis, or a NoSQL store — accessible to every web server.
2. Each request carries an identifier (cookie, JWT, session ID) the server uses to look up state from the shared store.
3. The load balancer becomes free to send any request to any server. Sticky sessions are no longer needed.
4. Auto-scaling becomes trivial: spinning up a new server is just attaching it to the LB; tearing one down loses no client state.

```
client → LB → any web server → session store (shared)
                            → DB / cache / etc.
```

## When to use

- Always, for any system that wants to horizontally scale or auto-scale the web tier.
- Any system that wants graceful failure of individual web servers.
- Multi-DC deployments — replication of the session store is far simpler than synchronizing in-memory state across regions.

When **not** to use: genuinely single-server systems where the operational cost of running a session store exceeds the value of statelessness. Even there, the source's view is that this is a temporary state.

A practical caveat the source glosses over: in practice many "stateless" tiers hold connection pools, in-memory caches, or rate-limit counters. Strict statelessness is rare; the rule is that **client-affecting** state must be externalized.

## Related

- [[Load Balancer]] — the LB only realizes its full benefit when the tier behind it is stateless.
- [[Multi-Data-Center Architecture]] — multi-DC effectively requires statelessness; you can't easily replicate in-memory session data across regions.
- [[Message Queue]] — another decoupling primitive that pushes state out of the web tier and into a durable shared component.

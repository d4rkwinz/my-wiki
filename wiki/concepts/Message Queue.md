---
aliases: ["MQ", "message queues", "queue"]
tags: [distributed-systems]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# Message Queue

A durable asynchronous buffer between two components. **Producers** publish messages to the queue without waiting for them to be processed; **consumers** read messages from the queue at their own pace. The queue persists messages so neither side has to be available at the same time.

## How it works

```
producer ──▶ [ msg | msg | msg | msg ] ──▶ consumer
                  (durable queue)
```

- Producers don't know who will consume — they just publish.
- Consumers don't know who produced — they just pull (or get pushed) work.
- The queue itself owns the durability guarantee: a message survives until at least one consumer has acknowledged processing it (semantics vary: at-most-once, at-least-once, exactly-once).
- Producers and consumers can scale **independently**. If consumers fall behind, the queue absorbs the backlog. If producers spike, consumers eventually drain it. Either side can crash and recover without losing in-flight work.

This decoupling is the core value: synchronous service-to-service calls require both to be up and sized for the same peak load. A queue removes both constraints.

## When to use

- Workloads where the producer's request latency must be fast but the work itself is slow (image resizing, email sending, webhook delivery, video transcoding).
- Spiky workloads where producer rate is bursty but consumers should run at steady cost.
- Decoupling services that should evolve independently — adding a new consumer is just adding a subscriber, with no producer change.
- Reliability requirements where dropping work on a downstream outage is unacceptable; the queue holds it until the downstream recovers.

When **not** to use: synchronous request-response flows where the caller genuinely needs the result before continuing. A queue forces async semantics on the caller; faking sync over a queue (poll for result) is usually worse than just calling the service directly.

## Related

- [[Stateless Web Tier]] — moving slow work onto a queue is one of the standard ways to keep the web tier fast and stateless.
- [[Single Point of Failure]] — the queue itself must be redundant; an unreplicated queue trades one SPOF (the consumer) for another (the broker).

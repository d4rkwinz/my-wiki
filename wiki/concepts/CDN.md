---
aliases: ["Content Delivery Network", "Content Distribution Network"]
tags: [networking, caching]
sources:
  - "Scale From Zero To Millions Of Users (2026-05-03)"
status: draft
updated: 2026-05-03
---

# CDN

A geographically distributed network of edge servers that cache static assets — images, video, CSS, JS, sometimes HTML — close to end users. Requests are answered by the nearest edge instead of traveling to the origin server.

## How it works

1. The first request for an asset in a given region is a **cache miss** at the local edge; the edge fetches the asset from the origin, stores it, and serves it to the client.
2. Subsequent requests in that region are **cache hits** — served entirely from the edge with no origin traffic.
3. Each cached asset has a **TTL** governing freshness; on expiry the edge revalidates against the origin.
4. **Invalidation** before TTL expires is done either via the CDN's API (purge by URL/pattern) or by **versioned URLs** (`/static/app.v123.js` → bumping the version forces a fresh fetch).
5. **Origin fallback** — if the CDN is unhealthy, clients should still be able to reach the origin directly (matters for the [[Single Point of Failure]] discussion).

Key knobs the source calls out: **TTL** (freshness vs origin load), **cost** (CDN bandwidth is metered), **fallback behavior**, and the **invalidation strategy**.

## When to use

- Static assets served to a geographically distributed audience — the latency win compounds with distance.
- High-traffic assets where offloading bytes from the origin is a measurable cost reduction.
- Anything you can confidently version (bumping the URL is the cleanest invalidation).

When **not** to use: highly personalized or rapidly mutating content (a CDN that constantly invalidates is just a slow proxy), strict-locality compliance scenarios where data must not leave a region, and tiny-scale sites where the CDN cost outweighs origin cost.

## Related

- [[Cache]] — a CDN is conceptually a cache layer; the same TTL/invalidation/consistency tradeoffs apply, just at network scale.
- [[Load Balancer]] — sits in front of the origin once a CDN miss falls through; both shape how traffic actually reaches your servers.

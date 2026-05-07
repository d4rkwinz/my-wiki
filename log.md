# Operations Log

Append-only record of structural changes to the vault. Newest entries at the top.

**Operations** — `ingest` (raw → digest), `promote` (digest → wiki), `lint` (graph audit), `restructure` (schema / folder change).

**Format** — date heading, then one bullet per operation. Keep summaries scannable; details live in the affected files, not here.

---

## 2026-05-03

- **restructure** · added `notes/` as a top-level peer of `wiki/` for source-shaped reference material that resists atomization (cheat sheets, guides, runbooks). Created `notes/notes.md` as the root index. CLAUDE.md updated: schema diagram, new `notes/` section under "Vault model", split "Ingestion workflow" into concept-shaped (two-phase) vs note-shaped (direct path), added "Notes-shaped sources: direct path" subsection. `wiki/index.md` now links `[[notes]]`.

- **note** · `raw/One Page Cheat Sheet.md` → `notes/Using AI Coding Tools Effectively.md` (no distillation; source was already note-shaped — a cheat sheet on LLM coding workflow whose value is in the failure-mode table + Phase 0–5 loop). Added frontmatter (tags, source title, status, updated). Linked from `notes/notes.md` under "AI-assisted development".

- **restructure** · pipeline change: raw/ and digest/ are now temporary working areas, deleted at the end of Phase 2. Provenance moves to `log.md` + plain-string `sources:` frontmatter on concepts. Deferred candidates moved to a new permanent file `wiki/queue.md`. Concept Template `sources:` field updated to plain-string format. CLAUDE.md updated: pipeline description, Phase 2 cleanup steps, Things to avoid, lint checks 2 + 5. Added "flat folders by default" trigger rule under Graph hygiene (split when concepts > 100 or any MOC > 40).

- **cleanup** · `raw/Scale From Zero To Millions Of Users.md` and `digest/Scale From Zero To Millions Of Users.md` deleted post-promote. Source title preserved on each promoted concept's `sources:` frontmatter as `"Scale From Zero To Millions Of Users (2026-05-03)"`. 20 deferred candidates from the digest moved to `wiki/queue.md`.

- **promote** · source: *Scale From Zero To Millions Of Users* (read 2026-05-03; strategy B — promote substantively-covered atoms only)
  - Created 10 concepts in `wiki/concepts/`: Load Balancer, Database Replication, Cache, CDN, Stateless Web Tier, Multi-Data-Center Architecture, Message Queue, Database Sharding, Sharding Key, Single Point of Failure
  - Created 5 MOCs in `wiki/maps/` under `System Design`: Networking, Databases, Caching, Distributed Systems, Operations
  - Deferred 20 candidates in the digest as `[ ]` TODO (waiting on deeper sources)
  - Digest status flipped to `promoted`

- **ingest** · `raw/Scale From Zero To Millions Of Users.md` → `digest/Scale From Zero To Millions Of Users.md` (status: `digested`)

- **restructure** · added `log.md` (this file) and `## Lint workflow` to `CLAUDE.md` — closing the gap with the canonical LLM-wiki pattern (Karpathy gist) by introducing an append-only ops record and a callable graph audit.

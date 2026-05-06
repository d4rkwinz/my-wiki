# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

An **Obsidian vault** serving as a personal engineering wiki, stored in iCloud Drive — not a software project. There is no build, lint, or test command. Tasks here are content-shaped: ingesting source material from the web (articles, YouTube transcripts, books), distilling it into reading notes, promoting concepts into atomic linked notes, and maintaining the resulting knowledge graph.

The vault is synced via Obsidian Sync (`sync: true` in `.obsidian/core-plugins.json`); every edit propagates to the user's other devices within seconds.

## Vault model: the raw → digest → wiki pipeline

The folder structure is a deliberate three-stage pipeline. Treat the boundaries as enforceable rules, not suggestions.

```
raw/        verbatim sources                       (READ-ONLY)
digest/     reading notes, one per source          (working area)
wiki/       published knowledge graph
  concepts/   atomic evergreen notes (one concept per note)
  maps/       Maps of Content (topical hubs); maps.md is the root MOC
  core.md     canonical concept registry
  index.md    vault landing page
templates/  Obsidian templates: Digest Template, Concept Template
log.md      append-only operations record (vault root)
```

### `raw/` — sources (TEMPORARY, read-only while present)

Imports from the `obsidian-importer` community plugin (web clippings, YouTube transcripts) plus manual book/article captures. Files may contain HTML artifacts, image-CDN URLs (e.g. `bytebytego.com/_next/image?...`), and run-on prose. **Never edit files in `raw/`.** If a typo or formatting issue annoys you, the fix lands in the corresponding `digest/` file as a quote, not in raw.

`raw/` is a working area, not an archive. Each file lives only until its digest is fully promoted; at that point both the raw file and the digest are deleted (see Phase 2 cleanup). The canonical record of what was ingested lives in `log.md`. Concepts retain provenance via the source title in their `sources:` frontmatter, not via wikilinks to deleted files.

### `digest/` — distillation (TEMPORARY working area)

One file per `raw/` source, mirroring the filename (e.g. `raw/Scale From Zero To Millions Of Users.md` → `digest/Scale From Zero To Millions Of Users.md`). This is the only place to "think out loud." Digests are **not** part of the published graph — they are scaffolding for promotion and are deleted once promotion completes. Deferred concept candidates from a digest are moved to `wiki/queue.md` before deletion so the future-ingestion queue is preserved.

Every digest follows the contract in `templates/Digest Template.md`:

- **Frontmatter** — `source: "[[raw/...]]"`, `status: reading | digested | promoted`, `read_on: YYYY-MM-DD`
- **TL;DR** — 3–5 lines, what the source actually claims
- **Key claims** — bulleted, each tagged with section/timestamp/page
- **Concept candidates** — atoms that should become `wiki/concepts/*.md`, one line each. This is the bridge to wiki.
- **Questions / gaps** — hand-waves, contradictions with other digests, your own doubts
- **Quotes** — verbatim snippets worth retaining outside the noisy raw file

### `wiki/` — published knowledge

- **`concepts/`** — Atomic evergreen notes. One concept per file. Filename = concept name in Title Case (`Load Balancer.md`, not `load-balancer.md`). See `templates/Concept Template.md`.
- **`maps/`** — Maps of Content. A MOC is a topical hub note that links concepts thematically (`Networking.md`, `Databases.md`). `maps/maps.md` is the root MOC.
- **`core.md`** — Canonical concept registry. One line per concept. Updated on every promote.
- **`queue.md`** — Rolling list of deferred concept candidates (atoms named by past sources but not yet substantively covered). Append on promote; tick off / remove when a future source promotes one.
- **`index.md`** — Vault landing page. Points to `core.md`, `maps.md`, and `queue.md`.

### `templates/`

Obsidian templates instantiated via the core `templates` plugin. Currently `Digest Template.md` and `Concept Template.md`.

## Ingestion workflow

Two explicit phases. Don't skip phase 1 — going raw → wiki directly produces shallow, low-fidelity concepts.

### Phase 1: Distill (`raw/X.md` → `digest/X.md`)

1. Read the raw source closely.
2. Create `digest/X.md` from `templates/Digest Template.md` (mirror the raw filename).
3. Fill in TL;DR, key claims, concept candidates, questions, quotes.
4. Set frontmatter `status: digested`.

### Phase 2: Promote (`digest/X.md` → `wiki/concepts/*.md` + MOCs + `core.md`)

For each item in the digest's "Concept candidates" list:

1. **Search first.** Check `core.md` and `wiki/concepts/` (including `aliases:` frontmatter) for an existing concept covering it. Prefer merging over duplicating.
2. **Create or update** `wiki/concepts/Concept Name.md` from `templates/Concept Template.md`. Add the source title (plain string, not a wikilink) to the concept's `sources:` frontmatter — format: `"Source Title (YYYY-MM-DD)"` where the date is the digest's `read_on`. Plain strings are intentional: the digest file will be deleted, and we want no broken wikilinks left behind.
3. **Link.** Add `[[wikilinks]]` to ≥2 related concepts in the body. No orphans.
4. **MOC.** Add the concept to the relevant `wiki/maps/*.md` MOC, creating a new MOC only if no existing topical hub fits.
5. **`core.md`.** Add a new line for new concepts; bump the source count for reinforced ones; tighten the definition if the new source sharpens it.

**After all candidates are processed:**

6. **Move deferred candidates to `wiki/queue.md`.** Any unchecked `[ ]` candidate from the digest becomes a queue entry: `- [ ] [[Concept Name]] — one-line description. From: Source Title (YYYY-MM-DD).`
7. **Append a promote entry to `log.md`** — list the source title, new concepts created, MOCs touched, and how many candidates were deferred to the queue.
8. **Cleanup.** Delete `raw/X.md` and `digest/X.md`. Provenance now lives in `log.md` (chronological) and each concept's `sources:` frontmatter (per-concept). The raw and digest files have no further role.

A promote is **not done** until `core.md` reflects it, `log.md` records it, the queue absorbs deferred candidates, and the raw + digest files are deleted. If you stop mid-promote, leave `status: digested` on the digest and skip cleanup so it's obvious work remains.

## Note conventions

- **Obsidian-flavored markdown:** `[[Note Name]]` for internal links (not `[text](path.md)`), `![[image.png]]` for embeds, `#tag` for tags, YAML frontmatter for properties (the `properties` core plugin is enabled).
- **Filenames:** human-readable, Title Case, spaces allowed (`Load Balancer.md`). No kebab-case.
- **Concept names:** singular nouns (`Load Balancer`, not `Load Balancers`). Use `aliases:` frontmatter for variants and acronyms (`["LB", "load balancers"]`).
- **Linking vs tagging:** `[[wikilinks]]` for concept-to-concept relationships — these are what power the graph view. Use `#tags` only for cross-cutting topics (`#networking`, `#databases`), not as a substitute for links.
- **Style:** match the surrounding note's tone. Existing notes use emoji-laden section headers (`## 🧠 Core Truth`); don't impose a uniform house style across the vault.

## Graph hygiene rules

- **No orphan concepts.** Every `wiki/concepts/*.md` must (a) be linked from at least one MOC and (b) link to ≥2 other concepts.
- **One concept per note.** If a note grows two distinct ideas, split it. If two notes overlap heavily, merge them and add an alias.
- **Prefer merging over duplicating.** Always search `core.md` and existing aliases before creating a new concept.
- **MOC-rooted.** A reader should reach any concept from `wiki/index.md` → `maps.md` → MOC → concept in ≤3 hops.
- **Flat folders by default.** `wiki/concepts/` and `wiki/maps/` are flat. The MOC tree is the only hierarchy; folders are storage. **Trigger to reconsider:** when `wiki/concepts/` exceeds ~100 files **or** any single MOC's concept list exceeds ~40 entries, evaluate splitting that domain into a sub-folder (e.g. `wiki/concepts/networking/`). Until both thresholds are far away, stay flat — premature folder hierarchies duplicate the MOC tree and create cross-cutting-concept ambiguity (where does `CDN` go when it belongs to both Networking and Caching?).

## Core-Truth maintenance

`wiki/core.md` is the single artifact answering "what does this wiki actually claim to know." It is updated on every promote step (Phase 2.5). Format is fixed — one bullet per concept: `- [[Concept Name]] — one-line definition. Sources: N.` Don't add columns, sections, or HTML; keep it scannable. Stale or wrong entries get rewritten in place, not appended-to.

## Commands

- `rtk ls *` and `rtk find *` are pre-allowed in `.claude/settings.local.json`. The repo inherits the global RTK token-killer setup — `git`, `find`, etc. are auto-rewritten through `rtk` by hook.
- **No git repository** is initialized here yet. Decision pending; do not run `git init` without explicit user approval. Versioning currently relies on Obsidian's `file-recovery` plugin and iCloud history.

## Operations log

`log.md` at the vault root is the append-only audit trail. Without it, "what was promoted when, from which digest" is irrecoverable in a vault with no git history.

**When to append:**

- After every Phase 2 promote — list new concepts, new MOCs, deferred candidates, and the digest the changes came from.
- After every ingest (Phase 1 complete) — one line is enough.
- After every lint run — note what was checked and what was fixed.
- After any structural change to schema, folders, templates, or this file.

**Format** — date heading at the top, one bullet per operation, newest entries on top. Keep summaries scannable; the details live in the files themselves, not in the log.

**Do not** use the log as a notes scratchpad or a TODO list. Open questions belong in the digest's "Questions / gaps" section; future-source TODOs belong in the digest's deferred candidates list.

**Rotation.** When `log.md` exceeds ~1000 lines **or** crosses a calendar-year boundary (whichever comes first), archive prior years into `log/YYYY.md` (creating the `log/` folder on first rotation) and leave `log.md` holding only the current year. Add a short "Previous years" section at the bottom of `log.md` with `[[log/2026|2026]]`-style links to each archive. Rotation is ~1 minute of copy-paste; do not skip it once triggered, and do not pre-rotate before the threshold — premature splitting just multiplies files. Cross-year search stays trivial via `grep -r` across `log.md` and `log/`.

## Lint workflow

A callable graph audit. Run on request ("lint the vault"), at the end of a multi-promote session, or before any restructure. Lint is **read + report + propose**, not silent auto-fix — surface findings, then ask before mutating the graph.

**Checks:**

1. **Orphan concepts.** Every `wiki/concepts/*.md` must (a) be linked from at least one `wiki/maps/*.md` MOC and (b) contain ≥2 outgoing `[[wikilinks]]` to other concepts. Flag violations.
2. **Dead wikilinks.** Every `[[Note Name]]` resolves to an existing note (or a `[ ]`-checked entry in `wiki/queue.md`). Unresolved links not on the queue are bugs. Also flag any wikilink pointing into `raw/` or `digest/` — those should be plain-string source titles, not links.
3. **core.md ↔ concepts symmetry.** Every concept file has a `core.md` entry; every `core.md` entry has a concept file. No drift in either direction.
4. **Source-count drift.** Each `core.md` entry's `Sources: N` matches the count of distinct entries in that concept's `sources:` frontmatter.
5. **Stale digest / leftover raw.** Any file in `digest/` or `raw/` is in-flight work. A digest with `status: promoted` is a bug — promoted digests should have been deleted in Phase 2 cleanup. A digest with `status: digested` whose every candidate now exists in `wiki/concepts/` and has no remaining `[ ]` candidates should also be cleaned up (move any deferred items to `queue.md`, then delete raw + digest).
6. **MOC reachability.** Every concept reachable from `wiki/index.md` → `maps.md` → MOC → concept in ≤3 hops.
7. **Frontmatter completeness.** Concept notes have all five fields (`aliases`, `tags`, `sources`, `status`, `updated`); digests have all three (`source`, `status`, `read_on`).
8. **Contradiction sweep** *(judgment, not mechanical)*. Skim concept definitions for direct contradictions across notes. Surface candidates; do not auto-resolve.

**Output:** an inline report listing each violation with file path, the rule, and a one-line proposed fix. After the user confirms, apply the fixes and append a `lint` entry to `log.md` listing what was fixed.

## Things to avoid

- **Never edit files in `raw/` while they exist.** Raw is read-only during its short lifetime — fixes to typos or formatting go in the digest as quotes. Raw and digest are deleted at the end of Phase 2; do not delete them earlier (you'll lose work in progress) and do not preserve them after (the wiki + log are the canonical record).
- **Don't skip the digest step.** Going raw → wiki directly produces shallow concepts.
- **Don't put `[[wikilinks]]` to `raw/` or `digest/` files in the wiki.** They will become broken links the moment cleanup runs. Use plain-string source titles in `sources:` frontmatter instead.
- **Don't create a top-level `README.md`** — Obsidian vaults use `index.md` (in `wiki/`) as the landing page.
- **Don't reorganize folders.** The raw/digest/wiki split is load-bearing.
- **Don't edit `.obsidian/`** unless the user explicitly asks; changes propagate via Sync to all their devices.

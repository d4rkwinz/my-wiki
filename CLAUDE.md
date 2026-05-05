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
  maps/       Maps of Content (topical hubs); Maps.md is the root MOC
  CORE.md     canonical concept registry
  index.md    vault landing page
templates/  Obsidian templates: Digest Template, Concept Template
```

### `raw/` — sources (READ-ONLY)

Imports from the `obsidian-importer` community plugin (web clippings, YouTube transcripts) plus manual book/article captures. Files may contain HTML artifacts, image-CDN URLs (e.g. `bytebytego.com/_next/image?...`), and run-on prose. **Never edit files in `raw/`.** If a typo or formatting issue annoys you, the fix lands in the corresponding `digest/` file as a quote, not in raw. Raw is the immutable source of truth — preserving it lets us re-ingest with a sharper eye later.

### `digest/` — distillation (working area)

One file per `raw/` source, mirroring the filename (e.g. `raw/Scale From Zero To Millions Of Users.md` → `digest/Scale From Zero To Millions Of Users.md`). This is the only place to "think out loud." Digests are **not** part of the published graph — concepts cite them, but they don't act as graph hubs themselves.

Every digest follows the contract in `templates/Digest Template.md`:

- **Frontmatter** — `source: "[[raw/...]]"`, `status: reading | digested | promoted`, `read_on: YYYY-MM-DD`
- **TL;DR** — 3–5 lines, what the source actually claims
- **Key claims** — bulleted, each tagged with section/timestamp/page
- **Concept candidates** — atoms that should become `wiki/concepts/*.md`, one line each. This is the bridge to wiki.
- **Questions / gaps** — hand-waves, contradictions with other digests, your own doubts
- **Quotes** — verbatim snippets worth retaining outside the noisy raw file

### `wiki/` — published knowledge

- **`concepts/`** — Atomic evergreen notes. One concept per file. Filename = concept name in Title Case (`Load Balancer.md`, not `load-balancer.md`). See `templates/Concept Template.md`.
- **`maps/`** — Maps of Content. A MOC is a topical hub note that links concepts thematically (`Networking.md`, `Databases.md`). `maps/Maps.md` is the root MOC.
- **`CORE.md`** — Canonical concept registry. One line per concept. Updated on every promote.
- **`index.md`** — Vault landing page. Points to `CORE` and `Maps`.

### `templates/`

Obsidian templates instantiated via the core `templates` plugin. Currently `Digest Template.md` and `Concept Template.md`.

## Ingestion workflow

Two explicit phases. Don't skip phase 1 — going raw → wiki directly produces shallow, low-fidelity concepts.

### Phase 1: Distill (`raw/X.md` → `digest/X.md`)

1. Read the raw source closely.
2. Create `digest/X.md` from `templates/Digest Template.md` (mirror the raw filename).
3. Fill in TL;DR, key claims, concept candidates, questions, quotes.
4. Set frontmatter `status: digested`.

### Phase 2: Promote (`digest/X.md` → `wiki/concepts/*.md` + MOCs + `CORE.md`)

For each item in the digest's "Concept candidates" list:

1. **Search first.** Check `CORE.md` and `wiki/concepts/` (including `aliases:` frontmatter) for an existing concept covering it. Prefer merging over duplicating.
2. **Create or update** `wiki/concepts/Concept Name.md` from `templates/Concept Template.md`. Add the digest to the concept's `sources:` frontmatter.
3. **Link.** Add `[[wikilinks]]` to ≥2 related concepts in the body. No orphans.
4. **MOC.** Add the concept to the relevant `wiki/maps/*.md` MOC, creating a new MOC only if no existing topical hub fits.
5. **`CORE.md`.** Add a new line for new concepts; bump the source count for reinforced ones; tighten the definition if the new source sharpens it.

After all candidates are processed, set the digest's frontmatter `status: promoted`. A digest is **not done** until `CORE.md` reflects it. If you stop mid-promote, leave `status: digested` so it's obvious work remains.

## Note conventions

- **Obsidian-flavored markdown:** `[[Note Name]]` for internal links (not `[text](path.md)`), `![[image.png]]` for embeds, `#tag` for tags, YAML frontmatter for properties (the `properties` core plugin is enabled).
- **Filenames:** human-readable, Title Case, spaces allowed (`Load Balancer.md`). No kebab-case.
- **Concept names:** singular nouns (`Load Balancer`, not `Load Balancers`). Use `aliases:` frontmatter for variants and acronyms (`["LB", "load balancers"]`).
- **Linking vs tagging:** `[[wikilinks]]` for concept-to-concept relationships — these are what power the graph view. Use `#tags` only for cross-cutting topics (`#networking`, `#databases`), not as a substitute for links.
- **Style:** match the surrounding note's tone. Existing notes use emoji-laden section headers (`## 🧠 Core Truth`); don't impose a uniform house style across the vault.

## Graph hygiene rules

- **No orphan concepts.** Every `wiki/concepts/*.md` must (a) be linked from at least one MOC and (b) link to ≥2 other concepts.
- **One concept per note.** If a note grows two distinct ideas, split it. If two notes overlap heavily, merge them and add an alias.
- **Prefer merging over duplicating.** Always search `CORE.md` and existing aliases before creating a new concept.
- **MOC-rooted.** A reader should reach any concept from `wiki/index.md` → `Maps` → MOC → concept in ≤3 hops.

## Core-Truth maintenance

`wiki/CORE.md` is the single artifact answering "what does this wiki actually claim to know." It is updated on every promote step (Phase 2.5). Format is fixed — one bullet per concept: `- [[Concept Name]] — one-line definition. Sources: N.` Don't add columns, sections, or HTML; keep it scannable. Stale or wrong entries get rewritten in place, not appended-to.

## Commands

- `rtk ls *` and `rtk find *` are pre-allowed in `.claude/settings.local.json`. The repo inherits the global RTK token-killer setup — `git`, `find`, etc. are auto-rewritten through `rtk` by hook.
- **No git repository** is initialized here yet. Decision pending; do not run `git init` without explicit user approval. Versioning currently relies on Obsidian's `file-recovery` plugin and iCloud history.

## Things to avoid

- **Never write to `raw/`.** It is the immutable source of truth.
- **Don't skip the digest step.** Going raw → wiki directly produces shallow concepts.
- **Don't create a top-level `README.md`** — Obsidian vaults use `index.md` (in `wiki/`) as the landing page.
- **Don't reorganize folders.** The raw/digest/wiki split is load-bearing.
- **Don't edit `.obsidian/`** unless the user explicitly asks; changes propagate via Sync to all their devices.

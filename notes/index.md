# Notes

Root index for `notes/` — source-shaped reference material that resists atomization: cheat sheets, opinionated guides, runbooks, personal heuristics, condensed summaries.

This is a peer of [[wiki/index|the wiki]], not a part of it. Notes are useful **as-is** rather than as raw material for atomic concepts. They live outside `wiki/concepts/` because they don't follow the atomic-note rules (one concept per file, ≥2 outgoing links, listed in `core.md`).

**When does something belong here vs. in `wiki/concepts/`?**

- If the value is in the **shape** of the document — a workflow loop, a checklist, a failure-mode table — it's a note.
- If the value is in **one idea** that should compose with others — it's a concept; promote into `wiki/concepts/` via the normal pipeline.
- If unsure: would atomizing it lose information? If yes → note. If no → concept.

---

## Sub-indexes

Domain-scoped landing pages within `notes/`. Each holds its own indexed material.

- 🤖 [[notes/ai/index|AI]] — AI-assisted development: tools, workflows, agent patterns, LLMOps foundations.
- 🏗️ [[notes/startups/index|Startups & Business]] — company playbooks, pricing patterns, growth strategies, founder lessons.
- 📚 [[notes/repos/index|Repos]] — curated GitHub bookmarks, one line per repo, sectioned by domain.
- 🔖 [[notes/bookmarks/index|Bookmarks]] — non-GitHub references: service taxonomies, tool comparisons, reading lists, free-tier guides.

---

## Conventions

- **Index files** are `index.md` per folder (lowercase). The root is this file (`notes/index.md`).
- **Content notes** use Title Case filenames (`Business Playbooks.md`, `Top 1% Claude Code Playbook.md`).
- **Linking to a sub-index** — path-qualify: `[[notes/startups/index|Startups]]`. Avoid bare `[[index]]` since multiple files share that basename.
- **Linking to content notes** — basenames are unique vault-wide, so `[[Business Playbooks]]` works from anywhere.
- **Adding a new domain** — create the folder, write its `index.md` describing scope + parent link, then link from the Sub-indexes section above.
- **Source-grouped silos** — multi-part series from one publisher may live in a publisher folder (e.g. `notes/DDoDS/LLMOps/`) so future parts file alongside Part 1. The domain sub-index still links to them; folder is just storage.

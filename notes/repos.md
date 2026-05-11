---
tags: [bookmarks, references]
status: published
updated: 2026-05-11
---

# Repos

Curated GitHub bookmarks. One line per repo. Sections by domain; tags for cross-cutting search.

**Format** — `- [author/name](https://github.com/author/name) — one-line value-prop (what this gives you that you can't get elsewhere). #lang #domain #attribute`

**Rules:**

- One line per repo, no exceptions. If it grows past one line, it belongs in `notes/` or `wiki/concepts/`, not here.
- Describe the **value-prop**, not the README's tagline. Why would I reach for this over alternatives?
- Tags are flat: `#lang #domain #attribute`. Don't nest. Use lowercase.
- Stale entries get rewritten in place, not appended-to.
- When you actually use a repo and learn from it, the *patterns* go to `wiki/concepts/` via the normal pipeline; the bookmark stays here.

---

## CLI tools

_(empty)_

## Web — frameworks, libraries, servers

_(empty)_

## Databases & storage

_(empty)_

## Distributed systems & infrastructure

_(empty)_

## DevOps & observability

_(empty)_

## AI agents — MCP, skills, harnesses

Tooling that extends or controls LLM coding agents (Claude Code, Gemini CLI, etc.): MCP servers that bridge agents to external systems, skill packs, and workflow harnesses. Distinct from the **ML & AI** section below, which is for models / training / inference.

- [googlecolab/colab-mcp](https://github.com/googlecolab/colab-mcp) — MCP server bridging a local agent (Claude Code, Gemini CLI) to a Google Colab session; gives your LLM agent control of Colab's free GPU/TPU runtime without leaving the editor. #python #mcp #ai #colab
- [mobile-next/mobile-mcp](https://github.com/mobile-next/mobile-mcp) — MCP server letting an LLM agent drive iOS/Android apps on simulators, emulators, or real devices via accessibility trees + screenshots; useful for automated mobile QA and end-to-end flows from prompts. #typescript #mcp #ai #mobile #automation
- [mattpocock/skills](https://github.com/mattpocock/skills) — Shell-installable Claude Code skill pack from Matt Pocock (`/tdd`, `/diagnose`, `/improve-codebase-architecture`, `/grill-me`); senior-engineer practices encoded as agent slash commands. #shell #claude-code #skills #ai
- [Chachamaru127/claude-code-harness](https://github.com/Chachamaru127/claude-code-harness) — Plan → Work → Review → Release framework wrapping Claude Code; enforces multi-perspective review and parallel-task safety guardrails. Go + Shell, no Node required (v4+). #go #shell #claude-code #framework #ai
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) — Drop-in `CLAUDE.md` with four coding principles (Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution) distilled from Karpathy's observations on LLM coding pitfalls. #claude-code #principles #ai
- [atilaahmettaner/tradingview-mcp](https://github.com/atilaahmettaner/tradingview-mcp) — MCP server exposing real-time crypto/stock screening, technical indicators (Bollinger Bands, candlestick patterns), and multi-exchange data (Binance, KuCoin, Bybit) to Claude Desktop; lets an LLM agent query live markets without leaving the editor. #python #mcp #ai #trading
- [elementalsouls/Claude-OSINT](https://github.com/elementalsouls/Claude-OSINT) — Drop-in Claude `SKILL.md` files for authorized red-team / bug-bounty external recon: 90+ recon modules, 48 secret-regex patterns, 80+ dorks, 9 read-only credential validators, 27 attack-path templates. #python #claude-code #skills #ai #security #osint
- [ruvnet/ruflo](https://github.com/ruvnet/ruflo) — Multi-agent orchestration platform for Claude with swarm intelligence, RAG integration, and native Claude Code / Codex MCP integration; for building conversational AI systems beyond single-agent loops. #typescript #claude-code #framework #mcp #ai #multi-agent
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — Production-grade engineering skills for AI coding agents (Claude Code, Cursor, Antigravity); shell-installable skill pack from Addy Osmani. #shell #claude-code #cursor #skills #ai

## ML & AI

For models, training, inference, datasets, and **domain-specific** agents (finance, healthcare, etc.). Coding-agent tooling lives in the section above.

- [brokermr810/QuantDinger](https://github.com/brokermr810/QuantDinger) — AI quant trading platform spanning crypto / stocks / forex with backtesting, live trading (IBKR), and multi-agent research; single codebase alternative to stitching together QuantConnect + custom data pipelines. #python #ai #trading #quant
- [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) — Multi-agent LLM framework for financial trading; agents take roles (analyst, researcher, trader, risk manager) and coordinate decisions — useful as a reference architecture for domain-specific multi-agent systems. #python #llm #ai #multi-agent #trading
- [virattt/dexter](https://github.com/virattt/dexter) — Autonomous TypeScript agent for deep financial research; small, readable example of a focused single-purpose research agent vs. general-purpose frameworks. #typescript #ai #agent #finance

## Languages & runtimes

_(empty)_

## Editor & dev tooling

_(empty)_

## Security

_(empty)_

## Misc / uncategorized

Holding pen for repos that haven't found their section yet. Re-categorize during periodic review (or during a vault lint).

_(empty)_

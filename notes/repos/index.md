---
tags: [bookmarks, references]
status: published
updated: 2026-05-17
---

# 📚 Repos

Parent: [[notes/index|notes]]

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

- [D4Vinci/Scrapling](https://github.com/D4Vinci/Scrapling) — Adaptive Python web-scraping framework that survives layout changes via auto-healing selectors; scales from single request to full crawl with stealth mode, Playwright integration, and a built-in MCP server for LLM agents. #python #scraping #automation #mcp
- [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) — Web scraping API/SDK purpose-built for LLM agents: crawls a site and returns clean Markdown ready to feed into a model (HTML stripped, content extracted, structured). Hosted + self-hostable. The "give me a website as Markdown" service every RAG pipeline ends up needing. #typescript #scraping #ai #markdown #rag
- [cporter202/social-media-scraping-apis](https://github.com/cporter202/social-media-scraping-apis) — Curated awesome-style list of scraping APIs and tools for Instagram, LinkedIn, X/Twitter, TikTok, YouTube, Facebook; reference for choosing which third-party API to use per platform rather than reinventing per-site scrapers. #javascript #scraping #social-media #awesome-list

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
- [LvcidPsyche/auto-browser](https://github.com/LvcidPsyche/auto-browser) — Open-source MCP-native browser agent giving an LLM a real Playwright browser with human-in-the-loop oversight; self-hosted Docker + FastAPI + noVNC stack you can attach to from Claude or any MCP client. #python #mcp #ai #browser-automation #self-hosted
- [bgauryy/octocode-mcp](https://github.com/bgauryy/octocode-mcp) — MCP server for semantic code search and context generation across public + private GitHub repos (respects your permissions); turns any accessible codebase into AI-optimized context for finding real implementations and live docs. #typescript #mcp #ai #code-search #github
- [wanshuiyin/Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep) — ARIS: markdown-only skill pack for autonomous overnight ML research with Claude Code / Codex / any LLM agent; cross-model review loops, idea discovery, paper review, experiment automation. No framework lock-in. #python #claude-code #skills #ai #ml-research
- [punitarani/fli](https://github.com/punitarani/fli) — Google Flights MCP server + Python library; lets an LLM agent search flights, fares, and routes via Google's underlying flight API. #python #mcp #ai #flights

## ML & AI

For models, training, inference, datasets, and **domain-specific** agents (finance, healthcare, etc.). Coding-agent tooling lives in the section above.

- [brokermr810/QuantDinger](https://github.com/brokermr810/QuantDinger) — AI quant trading platform spanning crypto / stocks / forex with backtesting, live trading (IBKR), and multi-agent research; single codebase alternative to stitching together QuantConnect + custom data pipelines. #python #ai #trading #quant
- [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) — Multi-agent LLM framework for financial trading; agents take roles (analyst, researcher, trader, risk manager) and coordinate decisions — useful as a reference architecture for domain-specific multi-agent systems. #python #llm #ai #multi-agent #trading
- [virattt/dexter](https://github.com/virattt/dexter) — Autonomous TypeScript agent for deep financial research; small, readable example of a focused single-purpose research agent vs. general-purpose frameworks. #typescript #ai #agent #finance
- [bytedance/Dolphin](https://github.com/bytedance/Dolphin) — ByteDance's ACL 2025 document parsing model (VLM-based OCR + layout analysis); turns PDFs/images into structured output via heterogeneous anchor prompting — research-grade alternative to MinerU or Marker. #python #ai #ocr #pdf #vlm
- [YSLAB-ai/scenario-lab](https://github.com/YSLAB-ai/scenario-lab) — Local-first scenario analysis framework using Codex or Claude as the reasoning engine; Monte Carlo + MCTS for forecasting market moves, geopolitical risk, and decision trees. #python #ai #forecasting #claude #market-simulation
- [AIDC-AI/Pixelle-Video](https://github.com/AIDC-AI/Pixelle-Video) — Fully automated short-video generation pipeline (Alibaba AIDC); ComfyUI-based image gen + TTS + video assembly — open-source alternative to commercial AI shorts tools. #python #ai #video-generation #comfyui #aigc

## Languages & runtimes

_(empty)_

## Editor & dev tooling

- [Vrun-design/openflowkit](https://github.com/Vrun-design/openflowkit) — Local-first, open-source AI diagramming tool for architecture diagrams and flowcharts; code-to-diagram, Mermaid-compatible, animated exports — free alternative to Eraser / Excalidraw+ for system-design work. #typescript #react #ai #diagramming #system-design #local-first
- [rixinhahaha/snip](https://github.com/rixinhahaha/snip) — Electron menu-bar app (macOS/Linux) for screenshot capture + annotation + Mermaid diagram rendering + AI organization via local Ollama; positioned as the "visual communication layer between humans and AI agents". #javascript #electron #ai #screenshot #ollama #local-first

## Security

- [megadose/holehe](https://github.com/megadose/holehe) — OSINT tool that checks whether an email is registered on 100+ sites (Twitter, Instagram, etc.) by probing forgotten-password flows; classic email reconnaissance for authorized recon. #python #osint #security #email #recon

## Misc / uncategorized

Holding pen for repos that haven't found their section yet. Re-categorize during periodic review (or during a vault lint).

- [Stnslv-k/shorts-saver-bot](https://github.com/Stnslv-k/shorts-saver-bot) — Self-hosted Telegram bot saving YouTube Shorts to Notion or Obsidian with AI extraction (Whisper + modular LLM backends); useful pattern for piping ephemeral content into a PKM. Docker. #python #docker #telegram #obsidian #notion #pkm #self-hosted

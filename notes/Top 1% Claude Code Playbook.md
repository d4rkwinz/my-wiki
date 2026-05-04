---
tags: [ai, claude-code, workflow, tooling]
sources:
  - "Becoming a top 1% Claude Code user: the complete playbook no one else is sharing (2026-05-04)"
status: published
updated: 2026-05-04
---

## 🚀 Top 1% Claude Code Playbook

> **Source:** allglenn, *Towards AI*, 2026-04-12 — [pub.towardsai.net](https://pub.towardsai.net/becoming-a-top-1-claude-code-user-the-complete-playbook-no-one-else-is-sharing-96057be1468e)

---

### 🧠 Core Truth

> **Claude Code is not a coding assistant — it's an agent orchestration framework.**
> The bottleneck isn't the model; it's whether you've built the road.

| Mindset      | What they do                                                          |
| ------------ | --------------------------------------------------------------------- |
| Everyone     | "I'll give Claude a task and see how it does."                        |
| Top 1%       | "I'll design a system where Claude operates with minimum supervision." |

Compounding effect: invest once in CLAUDE.md, hooks, subagents, MCP → every future session is faster *and* higher quality.

---

## 🆚 Claude Code vs. the world

| Tool        | Operates at      | Memory   | Runs commands | Hooks/Subagents | Best for                               |
| ----------- | ---------------- | -------- | ------------- | --------------- | -------------------------------------- |
| Copilot     | Line-level       | None     | No            | No              | Inline autocomplete                    |
| Cursor      | File-level       | Session  | Limited       | No              | Polished GUI for moderate tasks        |
| Claude Code | **Project-level** | Persistent (CLAUDE.md) | Yes | Yes | Hand off complete tasks, steer outcomes |

Pragmatic Engineer survey (Dec 2025): Claude Code became #1 AI coding tool within 8 months of launch.

---

## 📜 CLAUDE.md mastery

### The instruction budget rule

CLAUDE.md has **~150–200 instruction budget**, ~50 already used by the system prompt. Every wasted line dilutes the ones that matter.

**The test before committing any line:**
> "Would Claude make a mistake on my codebase WITHOUT this?" If no → delete.

### WHAT / WHY / HOW structure

- **WHAT** — stack. Reference, don't embed: `See @package.json for dependencies`.
- **WHY** — purpose behind decisions. *"SSR because users are on slow rural connections"* > *"use SSR"*. 10x leverage on micro-decisions.
- **HOW** — document what Claude **gets wrong on YOUR codebase**, not what it does right. If it never fails to use TypeScript, don't say "use TypeScript". If it keeps writing CommonJS in an ESM project, say that.

### File hierarchy (underused)

```
~/.claude/CLAUDE.md          ← global, all sessions
./CLAUDE.md                  ← project root, commit
./CLAUDE.local.md            ← personal overrides, gitignore
./src/api/CLAUDE.md          ← loads ON-DEMAND in that dir
./src/db/CLAUDE.md           ← loads ON-DEMAND for DB work
```

Subdirectory CLAUDE.md files are the trick to avoid blowing the root budget.

### Anti-patterns

- ❌ Stuffing everything in one file (>200 lines → instruction dropout)
- ❌ Documenting what Claude already does right
- ❌ Vague prohibitions: *"Never use --foo-bar"* → leaves Claude stuck. Better: *"Never use `--foo-bar`; prefer `--baz` instead"*
- ❌ Using CLAUDE.md for behaviors that **must** happen → use `settings.json` (deterministic). CLAUDE.md is **advisory**.

### Minimum viable CLAUDE.md (under 30 lines)

```md
# Project: [Name]

## Stack
- Node.js 22, TypeScript 5.4, Fastify 4
- PostgreSQL 16 + Drizzle ORM, Redis 7, Jest
See @package.json for all deps. See @docs/architecture.md for design.

## How to work
- Test: `npm test`  | Single: `npm test -- --testPathPattern=auth`
- Typecheck: `npm run typecheck`  | Lint: `npm run lint`

## Things to get right
- Always ESM imports (not CommonJS require)
- Redis keys must include version prefix: `v2:user:{id}:...`
- Auth middleware runs BEFORE rate limiting
- All DB queries go through service layer, never directly in routes

## Git
- Never commit to main. Branches: `feat/`, `fix/`, `chore/`. Conventional commits.
```

---

## 🪝 Hooks — deterministic guardrails

> **Hooks don't rely on Claude's judgment. They run whether Claude wants them to or not. That's the point.**

12 lifecycle events: `PreToolUse`, `PostToolUse`, `Stop`, `SubagentStop`, `SessionStart`, etc.

### Example: `.claude/settings.json`

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": [{ "type": "command", "command": "cd $PROJECT_ROOT && npm run lint --fix" }]
    }],
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{ "type": "command", "command": "python .claude/hooks/block_dangerous.py" }]
    }],
    "Stop": [{
      "hooks": [{ "type": "command", "command": "python .claude/hooks/session_summary.py" }]
    }]
  }
}
```

### Block-dangerous pattern

`block_dangerous.py` reads `tool_input.command` from stdin, checks against blocklist (`rm -rf`, `git push --force`, `DROP TABLE`):
- Exit code **2** → blocks + sends error message back to Claude as feedback
- Exit code **0** → allows

### CI/CD chain pattern

`SubagentStop` reads next command from a queue file and prints to stdout — Claude sees it as the next suggested action. Approve (or auto-approve) → pipeline continues without human glue.

---

## 🤖 Subagents — parallel isolated contexts

Each subagent has its **own** context window, system prompt, tool permissions, and model.

### Example: `.claude/agents/code-reviewer.md`

```md
---
name: code-reviewer
description: Reviews code for style, correctness, security, performance. Use after any implementation.
tools: Read, Grep, Glob, Bash
model: claude-opus-4-6
---

You are a staff engineer doing a thorough code review. Challenge every shortcut.

For each file changed, check:
1. Correctness — does this actually do what's intended?
2. Edge cases — what inputs would break this?
3. Security — injection vectors, exposed secrets, auth gaps?
4. Performance — O(n²) loops, unnecessary DB calls, memory leaks?
5. Readability — will a new team member understand this in 6 months?

Output: structured report with MUST FIX, SHOULD FIX, CONSIDER sections.
```

### 🔑 Highest-leverage technique: two-Claude review

| Session   | Context              | Role                                                               |
| --------- | -------------------- | ------------------------------------------------------------------ |
| Session A | Has all context, made tradeoffs, took shortcuts | Implements feature                          |
| Session B | **Fresh, reads diff cold**                      | Surfaces every shortcut and assumption A took for granted |

```bash
# Terminal 1
claude "implement the payment webhook handler, write tests, commit when passing"

# Terminal 2 (new session, no context)
claude "review the last commit on this branch as a staff engineer.
Check correctness, security, edge cases. Be harsh — this is going to production."
```

### Tool scoping (critical)

Subagents inherit ALL tools by default — including MCP. Lock them down:

```yaml
tools: Read, Grep, Glob   # research-only, can't modify anything
```

Or inherit-then-strip via `disallowedTools`.

---

## 🔌 MCP — bridge to the real world

Model Context Protocol turns external systems (GitHub, Postgres, Jira, Slack) into native Claude tools.

### Example: `settings.json`

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_..." }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "POSTGRES_CONNECTION_STRING": "postgresql://user:pass@localhost/mydb" }
    }
  }
}
```

Then natural-language usage:
- *"Check the last 5 failing GitHub Actions runs and identify the common pattern"*
- *"Query the users table to understand the schema before writing the migration"*
- *"Find the Jira ticket for this bug and add a comment with the fix approach"*

### 🔐 Principle of least privilege

> **Read-only by default.** Two MCP configs: one read-only for exploration, one read-write gated behind explicit permission. Code-reviewer subagent → read-only DB. Implementer subagent → write to dev DB only, never prod.

### Skills vs. MCP

| Use a **skill** | Use **MCP**                          |
| --------------- | ------------------------------------ |
| Workflows, patterns, knowledge   | Live data, live actions |
| *"How we deploy to k8s"*         | *"Query current prod state"* |
| Auditable markdown               | Black box                    |

**Prefer skills when in doubt.**

---

## 🔄 Worked example: full feature pipeline

Build `/api/v2/recommendations` end-to-end with personalized content, Redis cache, auth, tests.

| Step | Action                                                                                       |
| ---- | -------------------------------------------------------------------------------------------- |
| 0    | CLAUDE.md already loaded — zero per-session setup                                            |
| 1    | `claude "Interview me with AskUserQuestion about auth, caching, response shape, edge cases. Don't assume. Write to SPEC.md."` |
| 2    | Implement — `PostToolUse` hooks auto-lint every file; `PreToolUse` blocks dangerous commands |
| 3    | `claude "Use the code-reviewer subagent on the last commit"` — runs in parallel, returns MUST FIX / SHOULD FIX / CONSIDER |
| 4    | Back in main session: *"Reviewer found Redis leak on error path and auth-middleware order. Fix both, re-run tests."* |
| 5    | `claude "Use the security-auditor subagent on this feature"`                                 |
| 6    | `claude "Create a PR. Include spec, what changed and why, test coverage, known limitations."` Uses GitHub MCP + Jira MCP |

**Claimed result:** ~25 min for a 2–3 hour feature. The point isn't speed — it's that **quality gates ran automatically without being asked**.

---

## ⚙️ Operational patterns

### Context management

- Run `/compact` **manually at ~50% usage** (control what gets preserved instead of letting auto-compact decide).
- Add to CLAUDE.md: *"When compacting, always preserve: list of modified files, current test status, unresolved issues."*

### Background loops

```bash
/loop 5m  check if CI on branch feat/recommendations passed and report back
/loop 30m check for any new failing tests on main
```

### Per-task model selection

```bash
claude --model claude-sonnet-4-6   # default, most coding
claude --model claude-opus-4-6     # complex architecture, multi-file refactors
claude --model claude-haiku-4-5    # quick lookups, simple fixes
```

Set per-subagent in frontmatter so Opus calls are bounded.

### Other

- `claude remote-control` — run on your machine, attend from claude.ai or iOS app
- `/voice` — push-to-talk; hold space, describe, release. Faster for exploratory thinking-out-loud.

---

## 📁 Production-grade project layout

```
your-project/
├── CLAUDE.md                      ← project memory (commit)
├── CLAUDE.local.md                ← personal overrides (gitignore)
├── .claude/
│   ├── settings.json              ← hooks, models, permissions
│   ├── agents/
│   │   ├── code-reviewer.md
│   │   ├── test-writer.md
│   │   ├── security-auditor.md
│   │   └── pm-spec.md
│   ├── skills/
│   │   ├── deploy.md
│   │   ├── database-patterns.md
│   │   └── api-design.md
│   ├── commands/
│   │   ├── review-pr.md           ← /review-pr $ARGUMENTS
│   │   ├── ship.md                ← /ship — full pipeline
│   │   └── diagnose.md
│   └── hooks/
│       ├── block_dangerous.py
│       ├── auto_format.sh
│       └── session_summary.py
```

---

## 📅 5-day rollout

| Day | Action                                                                                          |
| --- | ----------------------------------------------------------------------------------------------- |
| 1   | Run `/init`. Delete 70% of generated CLAUDE.md. Add what Claude gets wrong. Keep <50 lines.     |
| 2   | First hook — `PostToolUse` on `Write` running your linter.                                      |
| 3   | Two-Claude review on next feature. Compare what cold-Session-B finds vs. what you'd have found. |
| 4   | First subagent — `code-reviewer` in `.claude/agents/`. Use on a PR.                             |
| 5   | First MCP — GitHub. Let Claude fetch issue context before implementing fixes.                   |
| 6+  | Iterate. Refine CLAUDE.md based on recurring mistakes. Add domain skills. Build full pipeline.  |

---

## 🔖 Resources

- **Official docs** — [code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview)
- **Best practices** — [code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)
- **claude-code-best-practice** (84 community practices, 20k★) — [github.com/shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice)
- **ClaudeLog** — [claudelog.com](https://claudelog.com/) — independent deep-dives & configuration guides
- **awesome-claude-code** — [github.com/hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — curated skills/hooks/plugins/MCPs
- **Ultimate guide** — [github.com/FlorianBruniaux/claude-code-ultimate-guide](https://github.com/FlorianBruniaux/claude-code-ultimate-guide)
- **Agent SDK tutorial** — [letsdatascience.com/blog/claude-agent-sdk-tutorial](https://letsdatascience.com/blog/claude-agent-sdk-tutorial)
- **Writing a good CLAUDE.md** (rigorous budget analysis) — [humanlayer.dev/blog/writing-a-good-claude-md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- **MCP spec** — [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- **Anthropic advanced patterns webinar** — [anthropic.com/webinars/claude-code-advanced-patterns](https://www.anthropic.com/webinars/claude-code-advanced-patterns)

---

## 🎯 The three ideas worth internalizing

1. **CLAUDE.md instruction budget** — document corrections, not confirmations. Subdirectory files for module-specific rules.
2. **Hooks vs. CLAUDE.md** — settings.json is deterministic; CLAUDE.md is advisory. Anything that *must* always happen belongs in hooks.
3. **Two-Claude review** — fresh-context Session B reading the diff cold is the most honest review you'll ever get.

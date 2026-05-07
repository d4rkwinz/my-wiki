---
tags: [ai, workflow, coding]
sources:
  - "One Page Cheat Sheet (2026-05-03)"
status: published
updated: 2026-05-03
---

## 🧾 Using AI Coding Tools Effectively

---

### 🧠 Core Truth

> **AI = fast junior dev with bad memory + overconfidence**

---

## ⚠️ The 7 Failure Modes

| Problem                 | What Happens                   | What To Do                 |
| ----------------------- | ------------------------------ | -------------------------- |
| ❌ False success         | Says “done” but code is broken | Always compile, lint, test |
| 🧠 Context loss         | Forgets things mid-task        | Keep tasks small           |
| 🩹 Lazy fixes           | Band-aid solutions             | Ask for senior-level fixes |
| 👤 Single-agent limit   | Runs out of memory             | Split work into chunks     |
| 📄 File limits          | Can’t see full files           | Read large files in parts  |
| 🔍 Truncated results    | Misses data silently           | Re-run searches narrowly   |
| ❌ No real understanding | Misses hidden references       | Manually verify changes    |

---

## 🔧 Golden Rules

### 1. 🔍 Always Verify

- `tsc` / compile
- `eslint`
- run app/tests  
    👉 Never trust “done”

---

### 2. ✂️ Clean First (STEP 0)

- Remove:
    - unused imports
    - dead code
    - debug logs  
        👉 Cleaner code = better AI output

---

### 3. 🧩 Work in Small Chunks

- Max **3–5 files per task**
- Break big work into phases

---

### 4. 🧠 Force Better Thinking

Instead of:

> “Fix this bug”

Say:

> “Fix this like a senior engineer. Refactor if needed.”

---

### 5. 🔁 Re-read Before Editing

- AI forgets context
- Always reload files before changes

---

### 6. 📄 Handle Large Files Properly

- > 500 lines → read in chunks
    
- Never assume AI saw everything

---

### 7. 🔍 Don’t Trust Search Fully

When renaming/changing:

- check:
    - direct usage
    - types
    - strings
    - dynamic imports
    - tests

---

## 🧠 Simple Mental Checklist

Before trusting AI output, ask:

- ✅ Did it compile?
- ✅ Did I verify results?
- ✅ Is this a real fix or a patch?
- ✅ Could it have missed something?

---

# 🔄 Daily Workflow You Can Follow

---

## 🟢 Phase 0 — Setup (1–2 min)

**Define clearly:**

- What’s the goal?
- Which files are involved?

---

## 🟡 Phase 1 — Cleanup (STEP 0)

Ask AI:

> “Remove dead code, unused imports, debug logs only. No refactoring.”

✔ Commit separately

---

## 🔵 Phase 2 — Plan (Important)

Ask:

> “Break this task into phases of ≤5 files each”

✔ Review plan before execution

---

## 🟣 Phase 3 — Execute (Loop)

For each phase:

### Step 1: Re-load context

> “Re-read these files before editing”

---

### Step 2: Execute changes

> “Implement Phase X like a senior engineer. Fix root causes, not patches.”

---

### Step 3: Force verification

Ask:

> “Run through compile/lint checks mentally and fix all issues”

(Or actually run locally)

---

### Step 4: Review yourself

- Scan for:
    - missing references
    - weird hacks
    - inconsistencies

---

## 🔴 Phase 4 — Cross-check (Critical)

After all phases:

- Search manually for:
    - missed references
    - broken imports
    - edge cases

---

## ⚫ Phase 5 — Final Validation

- Run app
- Run tests
- Try edge cases

---

# 🧠 Pro Tip (High Impact)

If something feels off:

> **Assume the AI missed something — because it probably did**

---

## 📌 Simple Loop to Remember

> **Clean → Plan → Small Steps → Verify → Double-check**
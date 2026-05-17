---
tags: [ai, llm, llmops, ai-engineering, survey]
sources:
  - "Foundations of AI Engineering and LLMs — LLMOps Crash Course Part 1 (2026-05-15)"
status: published
updated: 2026-05-15
---

## 🗺️ AI Engineering & LLMOps — Foundations

> **Source:** Avi Chawla, *Daily Dose of DS*, 2025-12-07 — [dailydoseofds.com/llmops-crash-course-part-1](https://www.dailydoseofds.com/llmops-crash-course-part-1/)
>
> Map-of-the-territory note. Kept as a survey because it's a domain-overview chapter — atomizing it would produce 22 shallow stub concepts. Use the follow-up checklist at the bottom as the ingestion queue: when a dedicated source lands for one of those concepts, promote it through the normal `raw → digest → wiki` pipeline.

---

### 🧠 Core Truth

> **AI engineering ≠ ML engineering.** ML engineering trains bespoke models from scratch (`data → model → product`). AI engineering adapts pre-trained foundation models (`product → data → model`).
>
> **LLMOps ⊂ AI engineering.** AI engineering is the broader discipline; LLMOps is the operational subset — deploying, monitoring, and maintaining LLM-powered systems in production. Extends MLOps; doesn't replace it.

---

## 🥪 The AI Application Stack

Three layers, like any software system:

| Layer            | What lives here                                                              | Who owns it traditionally     |
| ---------------- | ---------------------------------------------------------------------------- | ----------------------------- |
| **Application**  | UI, integration logic, prompts, user-input handling, output presentation     | Product / app engineers       |
| **Model dev**    | Model selection, fine-tuning, compression, dataset prep, eval                | ML engineers / researchers    |
| **Infrastructure** | Serving stack, GPU/TPU provisioning, vector DBs, observability, pipelines  | Platform / infra engineers    |

Since foundation models are pre-trained and available via API/libraries, **the application layer has exploded** — most new AI work happens here. Model dev shifts from training-from-scratch to *adapting* existing models.

---

## 🔄 The flow inversion

```
Classical ML:        data  →  model  →  product
Foundation models:   product →  data  →  model
```

You begin with a powerful general model, build a product fast, *then* gather feedback data or fine-tune if needed. This shortens prototyping but complicates deployment (huge models, GPU demands, open-ended evaluation).

---

## 🎛️ The Three Levers of AI Engineering

When an AI system underperforms, ask **in order** — cheapest lever first:

| #   | Lever            | What it means                                              | When to reach for it                                |
| --- | ---------------- | ---------------------------------------------------------- | --------------------------------------------------- |
| 1   | **Instructions** | Reshape the prompt (clarity, role/tone, few-shot examples) | Default starting point. Fast, free, no infra.       |
| 2   | **Context**      | Inject info the model didn't have (RAG, tools, history)    | When the model lacks knowledge, not capability.    |
| 3   | **Model**        | Switch model OR fine-tune OR compress                      | Last resort. Expensive but baked-in behavior.       |

**Rule of thumb:** exhaust 1 + 2 before touching 3. *"You'd be surprised how much you can get out of a model with clever instruction design."*

### Lever 1: Instructions (Prompt Engineering)

- **Clarity & specificity.** Ambiguous prompts → unpredictable outputs.
- **Role/tone via system message.** *"You are an expert financial advisor"* → answers shift register and depth.
- **Few-shot examples in prompt.** Show 2–3 input→output pairs; the model infers the pattern.
- **Iterate like code.** Read the output, diagnose what the model misunderstood, refine.
- **Limits.** Prompts can't add knowledge the model never saw, and very long behavioral prompts → consider fine-tuning instead.

### Lever 2: Context

Two forms:

- **Information context** — RAG, knowledge lookups, tool outputs, web search results
- **Interaction context** — conversation history, prior user inputs

**RAG in one paragraph:** user asks question → system retrieves relevant docs from external store (vector DB, search index) → docs + question both go into the prompt → model answers with grounded context. Extends model knowledge beyond training cutoff without retraining.

**Context engineering** = deciding what to include vs. exclude. Input length is finite; adding more text introduces noise *and* cost. Analogous to feature engineering in classical ML.

### Lever 3: Model

- **Selection.** Trade-offs across size, knowledge cutoff, architecture, license, hosting environment. *A smaller fine-tuned model often beats a larger general one on a specific domain.*
- **Fine-tuning.** Continues training on additional data; changes weights. Bakes behavior in so prompts can shrink. Requires training data + infrastructure + risk of overfitting.
- **Compression.** Quantization (8-bit / 4-bit weights), distillation (small student approximates large teacher), pruning.

**Fine-tune vs prompt heuristic:** if a 7B model hits 95% and a 70B model hits 97%, ship the 7B. The extra 2% rarely justifies the resource difference unless mission-critical.

---

## 📖 Glossary — what the article actually defines

Compact definitions to anchor reading. Promote individually only when a deeper source supports each.

**Disciplines**
- **AI Engineering** — discipline of building real-world applications around pre-trained foundation models; blends software engineering, data engineering, ML adaptation.
- **LLMOps** — operational subset of AI engineering focused on deploying, monitoring, and maintaining LLM-powered systems in production.
- **MLOps** — operational discipline for classical ML models (training pipelines, model registry, monitoring, retraining).

**Models**
- **Foundation Model** — massive pre-trained AI system; versatile base adaptable to many downstream tasks.
- **Large Language Model (LLM)** — foundation model specialized for language; autoregressive transformer trained on massive text corpora.
- **Transformer** — neural architecture (Vaswani et al., 2017) using self-attention; replaced RNNs because it scales and parallelizes.
- **Self-Attention** — mechanism weighing the relevance of different tokens in the input when producing each output token; captures long-range dependencies.
- **Autoregressive LM** — generates one token at a time conditioned on prior tokens (e.g., GPT family).
- **Masked LM** — predicts missing tokens using bidirectional context (e.g., BERT); used for non-generative tasks like sentiment analysis, code understanding.

**Behaviors & failure modes**
- **Emergent Abilities** — multi-step reasoning, chain-of-thought, complex instruction following — appear only above certain scale thresholds, absent in smaller models.
- **Zero-Shot Learning** — task done by an LLM with no examples, only a natural-language instruction.
- **Few-Shot Learning** — small number of input→output examples supplied in the prompt to steer output.
- **Hallucination** — coherent but factually wrong output; LLMs have no internal truth-checking. Mitigated via RAG and tool use.

**Techniques**
- **Prompt Engineering** — crafting instructions (system messages, role/tone, few-shot, iteration) to get desired output without changing weights.
- **Context Engineering** — deciding what info to include/exclude from the model's input to optimize relevance, cost, output quality.
- **RAG (Retrieval-Augmented Generation)** — retrieve relevant docs from an external store, inject into prompt; answers with grounded, up-to-date knowledge.
- **Fine-Tuning** — continue training a pre-trained model on additional data; changes weights, specializes for task/domain.
- **Model Quantization** — reduce weight precision (8-bit, 4-bit) to shrink memory and speed up inference.
- **Model Distillation** — train a smaller student to approximate a larger teacher; trades quality for cost/latency.
- **RLHF** — Reinforcement Learning from Human Feedback; fine-tunes LLM using human preference data to align outputs.

---

## ⚖️ MLOps vs LLMOps — key differences

| Axis              | MLOps                                                        | LLMOps                                                                        |
| ----------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| **Iteration focus** | Model and data                                              | Prompts and retrieved context (model fixed until fine-tune is justified)      |
| **Data needs**    | Labeled training data                                        | Unlabeled (leverages foundation model); collects feedback data over time      |
| **Deployment unit** | Model binary or inference pipeline                         | Pipeline of prompt templates + model (often an API) + context stores          |
| **Monitoring**    | Numeric performance metrics (accuracy, F1, latency)          | Qualitative outputs + specialized numeric metrics + user satisfaction signals |

> *"The differences don't mean we abandon MLOps knowledge — we extend it."*

---

## 🚧 Open evaluation challenges

LLMs produce open-ended text → traditional `accuracy / F1 / mean error` don't apply directly. Engineers face:

- **Custom success criteria** per use case
- **Human feedback** at scale (→ RLHF, preference data collection)
- **Unexpected failure modes** — hallucination, prompt injection, inappropriate content
- **Guardrails** — content filters, output validators, fallbacks

The article promises a dedicated chapter on LLM evaluation; this note will follow-up there.

---

## 🪜 Engineering wisdom worth keeping

- **Smallest model that hits target performance wins.** Diminishing returns at scale: 100M→10B is a bigger jump than 10B→50B. Choose for latency/cost unless the last few percent are mission-critical.
- **Use the highest lever only as needed.** Prompts → context → model, in that order.
- **Foundation-model engineering is less about creating models, more about adapting and governing them.** Most time goes to prompts + retrieval pipelines, not architecture.
- **LLMs are confidently wrong.** They predict text patterns; there's no truth-checking. Ground them with context or tools for high-stakes work.
- **Most problems can be solved through better prompting or smarter context.** Only a minority require model customization.

---

## 📋 Follow-up — concepts to promote later

When a dedicated source arrives for any of these, run the standard `raw → digest → wiki` pipeline. Use this list as the **reading queue priority** — pick sources that fill these gaps next.

### High priority (foundational, will be referenced everywhere)
- [ ] `[[AI Engineering]]` · `[[LLMOps]]` · `[[MLOps]]` — disciplines triangle
- [ ] `[[Foundation Model]]` · `[[Large Language Model]]` — model class hierarchy
- [ ] `[[Transformer Architecture]]` · `[[Self-Attention]]` — needs *Attention Is All You Need* or equivalent
- [ ] `[[Three Levers of AI Engineering]]` — frame concept; depends on individual technique concepts existing

### Medium priority (technique-level, ingest when a deep source lands)
- [ ] `[[Prompt Engineering]]` — needs a dedicated guide (Anthropic's prompt engineering docs, *Prompt Engineering Guide* book)
- [ ] `[[Context Engineering]]` — emerging discipline; few canonical sources yet, watch for one
- [ ] `[[RAG]]` — needs a real RAG paper or implementation guide; this article hand-waves the retrieval mechanics
- [ ] `[[Fine-Tuning]]` — needs treatment of LoRA, QLoRA, full fine-tune trade-offs
- [ ] `[[Hallucination]]` — needs a source treating mitigations, not just naming the problem
- [ ] `[[RLHF]]` — needs the InstructGPT paper or an RLHF tutorial

### Lower priority (well-defined elsewhere, ingest opportunistically)
- [ ] `[[Autoregressive Language Model]]` · `[[Masked Language Model]]` — distinction matters for understanding LLM vs BERT-class models
- [ ] `[[Emergent Abilities]]` — Wei et al. paper is the canonical source
- [ ] `[[Zero-Shot Learning]]` · `[[Few-Shot Learning]]` — could be one merged concept "Few-Shot / Zero-Shot Prompting"
- [ ] `[[AI Application Stack]]` — useful frame concept if a future source reinforces it
- [ ] `[[Model Quantization]]` · `[[Model Distillation]]` — needs a source on inference optimization

### Concepts named but not even defined here (queue.md candidates if surfaced again)
- Vector Database · Embedding · Tool Use / AI Agents · Guardrails · LLM Evaluation · Model Compression · Inference Optimization

---

## 🔖 Related (when they exist)

- [[Top 1% Claude Code Playbook]] — practical workflow patterns for LLM coding agents (different layer of the stack — this is "how to use the application layer effectively")
- [[Using AI Coding Tools Effectively]] — failure modes and a phase-by-phase workflow

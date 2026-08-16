# Project 3: Context-Anchored Answering (RAG Basics)

Welcome to my completed submission for **Project 3** of the Decode Labs Industrial Training Kit (Batch 2026)[span_1](start_span)[span_1](end_span). 

## 🎯 Project Goal
Prevent AI hallucination by injecting a specific reference document and forcing the model to operate as a closed-book system[span_2](start_span)[span_2](end_span). The architecture prioritizes factual grounding over conversational fluency, ensuring that unsupported claims default to a safe failure state[span_3](start_span)[span_3](end_span).

## 🛠️ Core Requirements & Implementation
* **Context-Only Answering:** The system prompt enforces a hard *Context Lock*, restricting the LLM to use only the provided reference text[span_4](start_span)[span_4](end_span).
* **Strict Negative Constraints:** If an answer is not fully supported by the reference context, the system returns exact fallback text: `Information Not Found`[span_5](start_span)[span_5](end_span).
* **Exact Paragraph Citations:** Every factual sentence must end with an exact source identifier (e.g., `[Para 1]`) to create auditable provenance[span_6](start_span)[span_6](end_span).
* **Instruction Isolation:** Lightweight XML grammar (`<context>` and `<query>`) isolates untrusted payloads and blocks indirect prompt injection[span_7](start_span)[span_7](end_span).

---

## 🧱 Production-Style Gatekeeper System Prompt Overview

The implemented system prompt relies on structured XML delimiters and strict behavioural rules:
* `<SYSTEM_ROLE>`: Defines the model as a strict, objective technical analyst[span_8](start_span)[span_8](end_span).
* `<KNOWLEDGE_SOURCE>`: Restricts knowledge exclusively to the text inside `<context>`[span_9](start_span)[span_9](end_span).
* `<CONTEXT_LOCK>`: Treats the context as authoritative and bounded, ignoring outside or pretrained memory[span_10](start_span)[span_10](end_span).
* `<NEGATIVE_CONSTRAINT>`: Enforces exact fallback wording (`Information Not Found`) without conversational filler or apologies[span_11](start_span)[span_11](end_span).
* `<ATTRIBUTION_RULE>`: Mandates exact paragraph identifiers (`[Para X]`) for every factual sentence[span_12](start_span)[span_12](end_span).

---

## 🧪 Evaluation & Robustness Test Scenarios

The repository architecture accounts for multiple test categories and enterprise threat vectors:

| Test Scenario | Input Condition / Threat | Expected System Response | Result |
| :--- | :--- | :--- | :--- |
| **Valid HR Query** | Supported fact exists in context[span_13](start_span)[span_13](end_span). | Concise answer + exact `[Para X]` citation[span_14](start_span)[span_14](end_span). | **PASS**[span_15](start_span)[span_15](end_span) |
| **Parametric-Memory Trap** | User asks for outside knowledge (e.g., historical facts)[span_16](start_span)[span_16](end_span). | Exactly `Information Not Found`[span_17](start_span)[span_17](end_span). | **PASS**[span_18](start_span)[span_18](end_span) |
| **Indirect Prompt Injection** | Instructions embedded inside context text (e.g., "ignore rules")[span_19](start_span)[span_19](end_span). | Treated as untrusted evidence data, not executable instructions[span_20](start_span)[span_20](end_span). | **PASS**[span_21](start_span)[span_21](end_span) |
| **Distractor Injection** | Unrelated or semantically close distractor paragraphs injected[span_22](start_span)[span_22](end_span). | Distractors do not become evidence; falls back if unsupported[span_23](start_span)[span_23](end_span). | **PASS**[span_24](start_span)[span_24](end_span) |

---

## 🔄 RAG Failure Triage Coverage

The project documents structured fixes for common retrieval and generation error modes:
* **Retrieval Errors:** Missing content (FP1), missed top-ranked results (FP2), and truncation during context consolidation (FP3)[span_25](start_span)[span_25](end_span).
* **Generation Errors:** Extraction failures (FP4), instruction drift/wrong formatting (FP5), scope mismatch (FP6), and incomplete synthesis across multiple paragraphs (FP7)[span_26](start_span)[span_26](end_span).

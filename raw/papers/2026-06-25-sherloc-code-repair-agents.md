---
source_url: https://arxiv.org/abs/2606.24820
ingested: 2026-06-25
sha256: a70a04606360266e9bce84f0d9e90cc3b
---

# SHERLOC: Structured Diagnostic Localization for Code Repair Agents

**Authors:** Hovhannes Tamoyan (NVIDIA, TU Darmstadt), Sean Narenthiran (NVIDIA), Erik Arakelyan (NVIDIA), Mira Mezini (TU Darmstadt / Hessian AI), Boris Ginsburg (NVIDIA)
**arXiv:** 2606.24151v1 [cs.CL] | 23 Jun 2026

## Abstract

LLM agents solve repository-level coding tasks through multi-turn tool use, but **utilize half their budget on locating faults before editing**. Dedicated localization frameworks have emerged yet are still evaluated as file retrieval rather than actionable diagnosis — producing locations without the diagnostic context a repair agent needs.

**SHERLOC (Structured Hypothesis-driven Exploration and Reasoning for Localization)** is a training-free framework pairing a reasoning LLM with compact repository tools and self-recovery, without fine-tuning or multi-agent orchestration.

**Results:**
- **84.33% accuracy@1** on SWE-Bench Lite
- **81.27% recall@1** on SWE-Bench Verified
- At ~30B parameters, matches or outperforms other agentic methods
- Injecting SHERLOC locations + diagnostic findings into repair agents yields significant downstream improvements

## 1 Introduction

### Problem
LLM agents solving repository-level coding tasks waste ~50% of their budget on locating faults before editing. Existing fault localization approaches produce file-level locations without the diagnostic context needed by repair agents.

### Key Insight
Localization should be evaluated not as file retrieval but as **actionable diagnosis** — locations must come with the diagnostic context a repair agent needs to actually fix the fault.

## 3 Method

### Design Principle: Reasoning-Native Tool Use

### 3.1 Initialization and Context
### 3.2 Tool Suite
### 3.3 Iterative Interaction Loop
### 3.4 Self-Recovery Mechanisms

## 4 Results

### 4.1 Localization Performance
- SWE-Bench Lite: **84.33% accuracy@1**
- SWE-Bench Verified: **81.27% recall@1**

### 4.2 Component Ablations
### 4.3 Implicit Knowledge vs. Tool-Assisted Reasoning
### 4.4 Backbone Generalization
### 4.5 Downstream Transfer to Code Repair
Injecting SHERLOC locations and diagnostic findings into repair agents yields significant average improvements.

## Limitations
- Implicit-knowledge confound
- Benchmark and framework scope
- Compute cost of headline configuration
- Cross-model prompt sensitivity
- Quality-based selection is not yet deployable

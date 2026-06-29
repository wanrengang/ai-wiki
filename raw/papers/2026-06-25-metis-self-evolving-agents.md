---
source_url: https://arxiv.org/abs/2606.24151
ingested: 2026-06-25
sha256: 592258f81d25111a6a8e14d7e2e6b1c3f
---

# Metis: Bridging Text and Code Memory for Self-Evolving Agents

**Authors:** Zijie Dai, Siuhin He, Hui Li, Qihui Zhou, Jiajun Li, Mingcong Song, Guoping Long, Hongjie Si, Xin Yao, Lin Zhang, James Cheng, Xiao Yan
**Institution:** The Chinese University of Hong Kong, Huawei, Wuhan University
**arXiv:** 2606.24151v1 [cs.CL] | 23 Jun 2026

## Abstract

Self-evolving agents improve over time by distilling experience from past executions and reusing it in future tasks. Existing systems represent such experience either as natural-language text injected into the agent context or as code exposed as callable tools. However, the choice between these representations is typically made at design time rather than derived from the characteristics of the experience itself.

We present **Metis**, the first controlled study that isolates text memory and code memory over an identical set of experiences. Results show the two forms exhibit complementary trade-offs in construction cost, execution efficiency, and transferability — neither representation alone is sufficient.

Metis builds a **hierarchical dual-representation memory**: textual experience organized into execution plans, environment facts, and common pitfalls; selectively crystallizes recurring plans into validated callable tools.

**Evaluation on AppWorld benchmark:** Metis improves task accuracy by **20.6% over ReAct** while reducing execution cost by **22.8%**.

## 1 Introduction

Current LLM-based agents are stateless — experience accumulated during task execution is only retained within the finite context window. Once interaction history is no longer accessible, the agent loses task-relevant knowledge.

Two existing experience representations:
- **Text memory:** experiences stored as natural-language knowledge, injected into context at runtime. Agent reads and reasons over them.
- **Code memory:** past routines encapsulated as callable tools/MCP services, invoked directly without re-deriving the procedure.

## 3 The Metis System

### 3.1 System Overview
Metis is built on a hierarchical dual-representation memory combining:
- **Text memory:** broad applicability, lower construction cost, better transfer reliability
- **Code memory:** higher execution efficiency, justified only when repeated reuse warrants the generation cost

### 3.2 Differentiated Text Reflection
Organizes textual experience into:
- Execution plans
- Environment facts
- Common pitfalls

### 3.3 Pattern-Aware Code Generation
Selectively crystallizes recurring plans into validated callable tools.

### 3.4 Memory Manager
Hierarchical organization of text vs. code memory based on reuse patterns.

### 3.5 Reflection Harness
Guides when to convert text memory → code memory.

## 4 Experiments

### 4.1 Experimental Setup
Evaluated on **AppWorld**, a challenging benchmark for interactive agents.

### 4.2 Main Results
- **+20.6% task accuracy** over ReAct
- **−22.8% execution cost** vs ReAct
- Better accuracy/cost tradeoff than representative self-evolving agent systems

### 4.3 Ablations
- Text memory: cheaper to build, better transfer reliability
- Code memory: accelerates execution significantly
- The combination is essential — neither alone suffices

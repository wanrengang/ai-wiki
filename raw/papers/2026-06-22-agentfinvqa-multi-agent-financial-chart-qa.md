---
source_url: https://arxiv.org/abs/2606.19782
ingested: 2026-06-22
sha256: 6d9265564df78a62c2e108f13fbb3dc5d1ad0875f08497160628239eff16d352
---
AgentFinVQA: A Deployable Multi-Agent Pipeline for Auditable Financial Chart QA
Aravind Narayanan  Shaina Raza
Vector Institute {aravind.narayanan,shaina.raza}@vectorinstitute.ai

Abstract

Financial chart question answering in regulated settings demands more than accuracy: practitioners must know which answers to trust before acting on them, and many institutions cannot send client data to external model providers. Yet existing chart-QA agents are accuracy-focused and opaque, and most assume proprietary API access; to our knowledge, none combines auditability with on-premise deployability without significant accuracy compromise. We present AgentFinVQA, a multi-agent pipeline that decomposes each query into planning, OCR, legend grounding, visual inspection, and verification, recording every step in a traceable Model Evaluation Packet (MEP) per sample. On FinMME, AgentFinVQA improves +7.68 pp over a primary-backbone matched zero-shot baseline with a proprietary backbone (Gemini-3 Flash; 71.24% vs. 63.56%, McNemar p≈1.1×10⁻¹⁶), and +4.84 pp with open-weights Qwen3.6-27B-FP8 served locally. The verifier's verdict also serves as a useful confidence signal (68.2% vs. 55.6% exact accuracy on confirmed vs. revised answers), enabling human-in-the-loop review routing. Error analysis shows that question misunderstanding, legend confusion and extraction error account for nearly two-thirds of failures and are the categories least detected by the verifier, identifying clear directions for future work. Together these results show that auditable, on-premise financial chart QA is practical and that the open-weights system keeps most of the accuracy gains while enabling full data residency. We release our code to support reproducible evaluation.

1 Introduction

Financial charts are a primary medium for communicating economic data, and automated question answering over such charts could substantially reduce the analytical burden on practitioners Shu et al. (2025). However, deployment in financial settings demands more than raw accuracy. Recent work shows that such systems exhibit significant hallucination rates on financial tasks, posing direct operational and regulatory risk to practitioners Zhang et al. (2025). A system that is frequently correct but unpredictably wrong is difficult to trust, and one that gives no visibility into its reasoning is impossible to audit.

Agentic decomposition, which breaks a chart question into specialised reasoning steps, has improved accuracy on general chart QA. The closest systems are agentic chart-QA frameworks such as ChartAgent (Kaur et al., 2026), which decompose queries into visual subtasks but rely on local computer-vision tools to manipulate the image, and ChartSketcher (Huang et al., 2026), which requires a two-stage fine-tune on hundreds of thousands of annotated samples. To our knowledge, no prior system delivers chart QA that is at once prompting-only (no local segmentation models or task-specific fine-tuning), auditable per answer, and deployable on open weights in-house, the combination that regulated financial settings require.

Contributions: (1) AgentFinVQA, a multi-agent chart-QA pipeline whose every step is recorded in a traceable MEP; (2) evidence that accuracy gains transfer from proprietary to locally-served open-weights models; (3) a verifier verdict that acts as a confidence signal for human review routing.

3 AgentFinVQA Framework

Problem setup: Given a financial chart image c, a natural-language question q, and an optional set of MCQ choices O = {o₁, …, o_k}, the task is to produce an answer a together with an auditable trace ℳ. Two deployment constraints: (i) prompting-only, no task-specific fine-tuning and no local computer-vision models; (ii) runnable on a self-hosted open-weights backbone.

Pipeline stages (in fixed order, with gated stages skipped when trigger conditions not met): Plan → OCR → Ground → Colour-Area → Inspect → Verify.

Planner: Text-only LLM receives the question and MCQ choices but does not see the chart image. Outputs structured JSON inspection plan with 2-3 focus points, question type classification, and answerability assessment. For MCQ questions, instructs vision agent to check each choice independently.

OCR Reader: Single focused VLM call transcribes all visible text — chart type, axis labels and ticks, legend entries, data labels, annotations.

Legend Grounder: Targeted VLM call maps each legend entry to its visual properties (colour description, approximate RGB, line style, confidence). Compliance check verifies the vision agent references at least one legend label by name; compliance retries fired on 13.2% of MEPs.

Colour-Area Tool: Deterministic pixel-counting stage using HSV colour masks (±10 hue, ±40 saturation/value) to count matched pixels per legend entry. No GPU, no trained model, no segmentation infrastructure. Gated conservatively: bar/pie/donut charts with comparison keywords and no colour ambiguity.

Vision Agent: CrewAI-orchestrated agent executes the inspection plan with chart image, OCR metadata, legend map, and colour-area hint. Produces draft answer, explanation, and per-choice confidence scores. Forced-choice retry reduces UNANSWERABLE rate on MCQ.

Verifier: Second independent VLM call audits the draft answer against the chart image. Produces CONFIRM or REVISE verdict and self-reported confidence score. Confidence gate downgrades revisions with confidence < 0.75 to confirmations. Verified answers: 68.2% exact accuracy vs. revised answers: 55.6%.

Backend flexibility: Configurable backend abstraction supporting Gemini API, OpenAI-compatible endpoints, and locally-served models via vLLM. Different stages can use different models. Open-weights evaluation: Qwen3.6-27B-FP8 on single A100-80G.

4 Results

Results on FinMME benchmark:

| System | Backbone | Mean Acc. | Exact | MCQ Mean | Open Question Mean |
|--------|----------|-----------|-------|----------|-------------------|
| Zero-shot | Gemini-3 Flash | 63.56% | 56.40% | 64.4% | 54.0% |
| AgentFinVQA | Gemini-3 Flash | 71.24% | 65.12% | 72.5% | 57.0% |
| Δ | | +7.68 pp | +8.72 pp | +8.1 pp | +3.0 pp |
| Zero-shot | Qwen3.6-27B-FP8 | 61.68% | 53.52% | 62.8% | 49.0% |
| AgentFinVQA | Qwen3.6-27B-FP8 | 66.52% | 60.24% | 68.1% | 48.0% |
| Δ | | +4.84 pp | +6.72 pp | +5.3 pp | -1.0 pp† |

† Standard Δ for Qwen within noise (CI ≈ ±10 pp).

Ablation study component contributions:
- MCQ-aware planning + forced-choice retry: Near-elimination of MCQ abstentions
- Multi-select MCQ support: +23.3 pp on multiple_choice
- Verifier with confidence gate: Confirmed answers are reliably more accurate than revised answers (68.2% vs. 55.6%)

Error analysis: Question misunderstanding, legend confusion and extraction error account for nearly two-thirds of failures and are least detected by the verifier.
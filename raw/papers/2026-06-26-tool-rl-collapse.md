---
source_url: https://arxiv.org/abs/2606.26027
ingested: 2026-06-26
sha256: ca19f28b70e97d81b7020a344929f5609ab68ff5548c1cc58e1349f7ff75b603
---
Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It
Yupu Hao1,2, Zhuoran Jin1,2, Huanxuan Liao1,2, Kang Liu1,2, Jun Zhao1,2
1The Key Laboratory of Cognition and Decision Intelligence for Complex Systems,
Institute of Automation, Chinese Academy of Sciences, Beijing, China 2School of Artificial Intelligence, University of Chinese Academy of Sciences, Beijing, China

Abstract

Tool use enables large language models (LLMs) to perform complex tasks, and recent agentic reinforcement learning (RL) methods show promise for enhancing model capabilities. However, RL alone often leads to instability or limited gains in tool-use tasks. In our experiments, some models exhibit catastrophic collapse, where performance abruptly drops and tool-invocation structures fail. The analysis reveals that these failures stem from unexpected probability spikes in specific control tokens, disrupting structured execution, yet the underlying tool-use capability remains intact, merely obscured by specific formats. To address this, we systematically investigate a diverse set of supervisory signals, including off-policy supervision, hint-based guidance, erroneous example supervision, and others, applied under both synchronous and interleaved training schemes. We find that interleaving supervised fine-tuning (SFT) with RL substantially improves stability, but exhibits degraded performance under format and content out-of-distribution (OOD) evaluation. We also analyze the impact of learning rates and generalization across settings. These results highlight the importance of understanding RL failures and demonstrate how diverse supervisory signals can guide exploratory learning, enabling robust training of LLMs for complex, multi-step tool-use tasks. Our Code is available at https://github.com/hypasd-art/Tool-RL-Box.

1 Introduction

Tool use plays an increasingly important role in large language models (LLMs). By leveraging external tools, LLMs can function as intelligent agents capable of autonomously performing complex interactive tasks. However, the multi-step and structured nature of tool-based interactions, together with the diversity of tool feedback, introduces substantial challenges for improving model capabilities. To mitigate these issues, prior works have focused on improving tool-use performance through optimized interaction frameworks, large-scale synthesis of high-quality trajectories, and refined supervised or unsupervised training methodologies. Particularly recent advances in agentic RL have motivated growing interest in RL as a more principled framework for agentic interaction, as agentic RL has demonstrated strong performance across a variety of complex tasks.

However, in practice, RL-based optimization often suffers from training instability and limited performance gains in tool-use settings. In our experiments, we observe a surprising failure mode where model performance can abruptly collapse to near zero, accompanied by a breakdown of valid tool-invocation structures. Through detailed analysis, we find that these failures are not caused by a loss of reasoning ability, but are instead driven by unexpected probability amplification of specific control tokens, which gradually distorts structured generation into degenerate execution patterns.

These observations indicate that RL alone is insufficient for stable, long-horizon structured generation. We find supervised fine-tuning on high-quality tool-use trajectories provides a strong initialization, improving early performance and preventing collapse. Nonetheless, prior work suggests that models trained with SFT can be limited in terms of generalization and performance compared to RL. Therefore, we aim to investigate how different supervisory signals interact with RL optimization and whether supervision can fundamentally stabilize multi-turn agentic training, attempting to combine them to mitigate their shortcomings.

To address this issue, we systematically investigate a broad family of supervisory signals, including SFT then RL, off-policy supervision, and erroneous trajectory supervision, under both synchronous and interleaved training paradigms. Additionally, we introduce Hint-based guidance by prepending correct hints before the model generates responses, and remove these hints during optimization. We also propose Process Reflection Supervision, which extracts reflections from intermediate reasoning steps in the training process to construct SFT data, further improves both stability and final performance.

Our work makes three key contributions:
• We identify structural collapse as a fundamental failure mode in multi-turn agentic RL. We show that RL on tool-use tasks does not primarily degrade reasoning ability, but induces a structural collapse in which generation degenerates into malformed control-token sequences, breaking tool-invocation structure while preserving underlying task competence.
• We uncover a token-level mechanism underlying RL instability. We find that RL disproportionately amplifies special control tokens, leading to a redistribution of policy mass. This reveals that instability arises from control-token dynamics rather than capability degradation.
• We conduct a systematic study of supervisory signals for stabilizing multi-turn agentic RL. We explore six supervisory configurations across two model families and categorize them into synchronous and interleaved paradigms.

2 Related Works

2.1 Tool Learning
Recent advances have extended LLMs with tool-use capabilities, enabling interaction with external APIs and environments beyond text generation. Through tool invocation, models can perform task execution, information retrieval, and planning, improving their ability to solve complex tasks efficiently. Prior works enhance tool-use capabilities through reasoning frameworks, supervised fine-tuning, and reinforcement learning. Although RL improves tool-invocation performance, its effectiveness strongly depends on the base model's prior tool knowledge.

2.2 Expert Trajectories in Reinforcement Learning
RL improves models through trajectory exploration and reward-based updates, but can stagnate when sampling quality is poor. Recent works mitigate this by incorporating expert or ground-truth trajectories into RL. However, these methods are mainly studied in single-turn reasoning tasks (e.g., mathematics), and their effectiveness in multi-turn tool-use settings remains underexplored.

3 Preliminary
We represent multi-turn tool utilization trajectory τ by LLMs as a sequence of actions and feedbacks: τ=(a1,r1,a2,r2,…,aT,rT). Each action at ∈ A is drawn from an action space that includes both feasible tool invocations and natural language responses. At each step, the feedback rt ∈ R is given based on current action and the preceding trajectory.

4 Method

4.1 Experiment Setup
Dataset: BFCL-V3, a multi-turn tool-use benchmark where models must invoke multiple tools across interactive environments. Four settings: Base, Miss Func, Miss Param, and Long Context.
Model: Qwen2.5-1.5B-Instruct and Qwen3-1.7B (non-thinking variant).

4.2 Problem of Catastrophic Collapse in RL
Direct RL on Qwen2.5-1.5B-Instruct often causes catastrophic collapse, with sudden reward drops and sustained KL divergence spikes. Qwen3-1.7B exhibits milder instability, but pure RL gains remain limited. Applying SFT on an additional tool-use dataset before RL stabilizes training.

4.3 Analysis of Collapse Reasons
The collapse does not primarily reflect degradation of the model's underlying capability. Instead, the collapse progressively shifts generation toward malformed control-token patterns that corrupt structural integrity of tool invocation. Special control tokens used in tool-calling formats (such as <tool_call> and <|im_end|>) become increasingly confused during training. As RL optimization proceeds, these tokens are over-amplified and incorrectly combined, causing the model to drift from valid tool-use trajectories toward degenerate termination patterns.

We categorize model outputs into four structural states:
- Healthy Tool Call: well-formed tool invocations that strictly follow the required schema
- Healthy Response: natural-language responses containing no tool-related artifacts
- Text Pollution: intermediate failure stage where incomplete tool tags or misplaced control tokens begin to appear
- Collapsed: severe degeneration

4.4 Supervisory Signal Configurations
We systematically explore six supervisory configurations:
1. SFT then RL (synchronous): standard two-stage approach
2. Off-policy supervision: mix expert trajectories into RL replay buffer
3. Hint-based guidance: prepend correct hints before generation, remove during optimization
4. Erroneous trajectory supervision: train on failure examples to recognize and avoid errors
5. Interleaved SFT+RL: alternate between SFT and RL training
6. Process Reflection Supervision: extract reflections from intermediate reasoning steps as textual supervisory signals

Key Finding: Interleaved training consistently improves stability and generalization, while synchronous methods are prone to distribution mismatch. Simply mixing supervision with RL is insufficient: synchronous methods often suffer from distribution mismatch, while interleaved training with SFT significantly improves stability but may degrade performance under format- and content-level OOD evaluation.

5 Conclusions
Our work highlights that agentic RL failure is primarily a structural collapse problem rather than a capability limitation. Carefully designed supervisory signals can effectively regulate token-level execution patterns, enabling more stable and robust tool-use learning in LLMs.
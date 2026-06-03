---
title: RSAgent
created: 2026-05-29
updated: 2026-05-29
type: concept
tags: [agent, multimodal, reasoning, training, research]
sources: [raw/articles/2026-05-29-rsagent-visual-segmentation-icml2026.md]
confidence: high
---

# RSAgent

让多模态大模型通过多轮工具调用，完成开放语义文本引导分割的 Agent 框架。ICML 2026 论文。

## 核心问题

**开放语义分割难在哪里：**
- 用户输入可能是"图中左侧正在被人拿起的物体"（需要空间关系理解）或"找出湍急水流中保障个人安全的装备"（需要场景常识和用途推理）
- 模型必须在"语义理解"和"准确掩码"之间完成可靠转换
- 此前路线短板：**不是"不能产生 mask"，而是"缺少确认与纠错过程"**

## 方案

RSAgent 的关键不是把 MLLM 直接改造成 mask decoder，而是**让它成为能够调度视觉工具的智能体**。

### 四步循环
1. **Observation**：读取图像与历史结果
2. **Thought**：用自然语言分析当前候选区域是否满足指令
3. **Action**：选择工具和像素提示
4. **Feedback**：接收工具输出并写入上下文

### 训练策略
- **Cold-start SFT**：约 5K 条高质量多轮推理轨迹，解决"会不会按格式工作"
- **Agentic RL**：约 2K RL 示例 + 8K RefCOCOg 训练样本，解决"怎样做得更好"

## 核心创新

| 要素 | 描述 |
|------|------|
| 任务类型 | 开放语义文本引导分割（text-guided segmentation） |
| 核心机制 | 多轮工具调用 + 迭代修正 |
| 基础模型 | Qwen2.5-VL-7B-Instruct |
| 分割工具 | SAM2-large |

## 实验结果

**ReasonSeg test：**
- RSAgent：**66.5% gIoU**
- Seg-Zero-7B：57.5% gIoU
- **提升：+9.0%**

**RefCOCOg：**
- RSAgent：**81.5%** 平均 cIoU

**关键发现：** 先让模型学会规范工具调用，再通过强化学习优化长程决策，是成立的关键。

消融实验：
- 无训练 tool-agent：30.1 cIoU
- + Cold-start SFT：55.4（+25.3）
- + Agentic RL：**57.9**

## 核心价值

RSAgent 展示了一条从"看图问答"走向"视觉行动"的路径——模型可以围绕文本目标持续观察、调用工具、接受反馈、修正假设，并把最终判断落实到图像像素。

**适用场景：** 数据标注、机器人感知（执行前重新确认目标区域）、设计编辑、科学图像分析

## 相关概念

[[agentic-code-reasoning]] — 同样关注 Agent 的推理与行动能力，Semi-formal Reasoning 让 LLM Agent 像形式化验证一样做代码分析

[[test-time-compute-scaling]] — RSAgent 的多轮迭代修正可视为 test-time compute scaling 在视觉 Agent 领域的具体实现

[[faster-vla-action-sampling]] — FASTER 的 TTFA 3 倍加速与 RSAgent 的动作空间加速有相似目标

[[embodied-ai-safety-survey]] — 具身智能安全框架，RSAgent 的视觉 Agent 能力演进需要安全考量

[[muse-autoskill-agent]] — MUSE-Autoskill 的 Skill 全生命周期管理 vs RSAgent 的工具调用与修正能力
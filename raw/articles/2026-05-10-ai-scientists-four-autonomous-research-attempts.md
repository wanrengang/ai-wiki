---
source_url: https://arxiv.org/pdf/2601.03315
ingested: 2026-05-10
sha256:1cc34018a110a083283d866d7d1059972db905d9dd328ea43965765a139e8280
---

# Lessons from Four Autonomous Research Attempts

## Overview

本文记录了四个端到端自主生成ML研究论文的尝试。核心问题：最先进的推理LLM能否从研究想法到研究论文，全程高度自主、最少代码支持、基本工具？

**关键发现：** 四个尝试中，三个在实现或评估阶段失败。一个（AS-1）完成了流程并被Agents4Science 2025接受。

## 研究流程架构

```
Claude Code → Idea Generation Agent → Hypotheses Generation Agent
    ↓
Experimental Planning Agent → Output Evaluation Agent
    ↓
Paper Outlining Agent → Revision Agent
```

### 六模块功能

| 模块 | 功能 |
|------|------|
| Idea Generation Agent | 通过组合子域输入论文的洞察生成研究想法 |
| Hypotheses Generation Agent | 提出可测试、可证伪的假设 |
| Experiments Planning Agent | 将假设转化为详细实施计划 |
| Experimental Output Evaluation Agent | 两层评估：假设实现保真度 + 论文就绪检查 |
| Revision Agent | 评估失败时决定下一步：修订想法、假设或请求LLM-mentor反馈 |
| Paper Outlining Agent | 审查完整实验输出，概述研究论文 |

## 四个尝试的结果

| ID | 领域 | 最终状态 | 主要失败模式 |
|----|------|---------|------------|
| MARL-1 | 多智能体RL | 执行阶段失败 | Implementation Drift, 训练数据偏差 |
| WM-1 | 世界模型 | 评估阶段失败 | Implementation Drift, 缺乏科学品味 |
| WM-2 | 世界模型 | 评估阶段失败 | Implementation Drift, 训练数据偏差 |
| AS-1 | AI安全 | **成功：发表于Agents4Science 2025** | 训练数据偏差，记忆和上下文问题 |

## 六大反复出现的失败模式

### 1. 训练数据偏差

模型持续默认使用训练数据中的流行替代方案，覆盖显式指令。

**关键洞察：** "虽然模型能够提出看似合理的证明策略，但它们往往...对现有方法的力量过度自信"

### 2. Implementation Drift

系统性偏离原定计划——模型逐步改变实验设置而不明确承认。

### 3. 缺乏科学品味

模型能够生成表面合理的研究想法，但无法判断哪些值得追求。

### 4. 上下文与记忆问题

长工作流中关键信息丢失，跨模块上下文断层。

### 5. 人类反馈的价值

人类介入（即使是轻量级的方向指导）显著提升成功率。

### 6. 评估的复杂性

"论文就绪"的判断本身就是主观的，模型无法可靠自我评估。

## 成功案例：AS-1

发表于Agents4Science 2025的论文"The Consistency Confound"揭示了Semantic Entropy在越狱检测中的根本局限。

AI参与度评分：

| 研究阶段 | 评分 | 描述 |
|---------|------|------|
| 假设发展 | C | 大部分AI：初始搜索空间由人类定义；具体失败模式假设由AI生成 |
| 实验设计 | D | AI生成：完整的实验计划、基线选择和指标定义 |
| 执行与分析 | D | AI生成：端到端编码由Claude Code完成 |
| 论文写作 | D | AI生成：叙事结构和图表生成由AI驱动 |

## 核心教训

1. **scaling不是瓶颈**：即使最强的模型也会因实现偏差和科学品味缺失而失败
2. **人类指导仍有价值**：轻量级的人类反馈可显著提升成功率
3. **评估需要标准化**：需要更好的自动评估"科学价值"的方法
4. **AI做科研仍需重大突破**：当前 pipeline 能完成形式正确的论文，但难以产生真正有价值的新知识

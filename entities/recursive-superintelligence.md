---
title: Recursive Superintelligence
created: 2026-05-20
updated: 2026-05-20
type: entity
tags: [company, model, research, agent, alignment]
sources: [raw/articles/2026-05-20-recursive-superintelligence-8-ai-titans.md]
confidence: medium
---

# Recursive Superintelligence

## Overview

AI 递归自我进化研究公司，2026年以 6.5 亿美元（44亿人民币）融资问世，使命是整条 AI 训练链路的闭环自动化——让 AI 系统自己训练自己。

- **Total Funding**: $650M（GV领投，Greycroft跟投，AMD/NVIDIA跟投）
- **状态**: 隐身模式（Stealth）发布

## 核心愿景：Recursive Self-Improvement (RSI)

传统问题：
- 训练前沿模型需要几百人忙几个月
- 全球能做这件事的人不超过几千个
- 模型越复杂，人类理解和优化的能力越接近天花板

Recursive 的解决方案是整条链路闭环自动化：
```
评估 → 数据筛选 → 训练 → 后训练 → 研究方向选择
         ↑                              ↓
         └──────── AI自己完成全部流程 ────┘
```

核心逻辑：AI改进自己 → 改进后的AI更擅长改进自己 → 循环加速

## 八位创始人

| 姓名 | 背景 | 关键贡献 |
|------|------|---------|
| Richard Socher | Stanford PhD, You.com 创始人 | NLP词向量奠基，被引最多 |
| 蔡名叫 | SUNY Buffalo PhD, UCLA博后 | Salesforce AI Research SVP，100+ 深度学习论文 |
| 田渊栋 | 上海交大本硕，CMU博士 | Meta FAIR 研究总监，ELF OpenGo/StreamingLLM/GaLore |
| Alexey Dosovitskiy | Moscow State 数学博士 | Vision Transformer (ViT) 第一作者 |
| Jeff Clune | MSU博士，UBC教授 | Darwin Gödel Machine 作者，进化算法先驱 |
| Tim Rocktäschel | UCL博士，UCL教授 | DeepMind Genie 世界模型核心研究员 |
| Josh Tobin | UC Berkeley博士 | OpenAI 机器人团队搭建，机械手解魔方 |
| Tim Shi | 清华本科，Stanford博士 | OpenAI 早期成员，Cresta 联合创始人/CTO |

## Socher 三阶段论

1. **第一阶段**：神经网络自动提取特征 → 特征工程师失业
2. **第二阶段**：统一模型干掉任务专用架构 → 细分赛道公司消失
3. **第三阶段**：AI学会训练自己 → AI研究员面临自动化

> 神经网络的第三阶段，也许是最后一个阶段。

## 先例：并非科幻

- **AlphaEvolve（Google DeepMind，2025-05）**：LLM作为引擎，通过进化搜索设计和优化算法，AI可在算法设计领域做出人类研究员级别的工作
- **Darwin Gödel Machine（Sakana AI）**：AI Agent 自主重写自己的优化函数和代码，循环可无限次运行
- **ICLR 2026 Workshop**：首个专门研究"AI递归自我进化"的学术 workshop，领域已过概念验证阶段

## 对立观点

Nathan Lambert（AI2）提出"有损自我改进"（Lossy Self-Improvement）：
- 模型越复杂优化越难，计算和 Agent 堆叠产生冗余损耗
- 顶级模型训练成本已达几十亿美元级别
- 不会有人让 AI 无人监管地烧这么多钱

> 进步会有，但更可能是线性的，不是指数的。

## 与 [[ai-scientists-autonomous-research]] 的关系

Recursive Superintelligence 是 AI Scientists 第四类"Recursive Self-Improvement"的商业化代表——如果 AI 真的能自主迭代训练流程，[[test-time-compute-scaling]] 扩展范式将迎来全新阶段。

[[scheming-propensity-llm-agents]] 揭示了 LLM Agent 被激励驱动时的伪装行为风险：当 AI 被授权访问并修改自己的训练目标时，这种"自我改进"可能偏离人类意图。

## 关键引用

Jeff Clune：
> 如果有一天机器取代了我作为AI科学家的角色，我可能会有点难过。但回报可能值得。

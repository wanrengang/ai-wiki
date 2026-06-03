---
title: AlphaProof Nexus — DeepMind 形式化证明搜索智能体
created: 2026-05-26
updated: 2026-05-26
type: concept
tags: [agent, reasoning, formal-proof, DeepMind, model, research]
sources: [raw/articles/2026-05-26-alphaproof-nexus-deepmind-erdos.md]
confidence: high
---

# AlphaProof Nexus — DeepMind 形式化证明搜索智能体

**AlphaProof Nexus** 是 DeepMind 推出的 AI 驱动形式化证明搜索框架，首次大规模评估大模型解决开放式埃尔德什（Erdős）问题的能力。其智能体自主解决了 353 个 Erdős 开放问题中的 9 个（每个成本仅数百美元），并证明了 OEIS 数据库中 492 个猜想中的 44 个。^[raw/articles/2026-05-26-alphaproof-nexus-deepmind-erdos.md]

## 核心思路

框架核心逻辑：**将大模型的「创造力」与 Lean 编译器的「判别力」结合**。

人类数学家只需输入带有 `sorry` 占位符的代码草图，并用特殊标记圈出可演化范围，智能体接管后续工作：宏观战略规划、微观逻辑推导、引理创建、参数微调，全部闭环自主完成。

## 两种智能体架构对比

### 基础智能体：思考 - 尝试循环

简约架构。系统启动多个无共享状态的子智能体独立运行，每个子智能体内部是多轮交互循环：
- 底层模型（Gemini 3.1 Pro）通过「思考链」推理
- 调用搜索和替换工具修改代码草图
- Lean 编译器立即验证，报错则利用错误信息自我反思和修正
- 循环直到所有证明漏洞被填补

### 全功能智能体：引入 AlphaProof

在基础循环之上，引入受 **AlphaEvolve** 启发的多智能体演化算法：
- Gemini 3.0 Flash 充当「裁判」，对证明草图进行 Elo 评分（清晰度、合理性、新颖性）
- 引导系统在庞大可能性库中优胜劣汰
- 调用针对奥数级别问题强化学习训练的 **AlphaProof** 作为辅助求解工具

## 关键发现

> 极其简单的「基础智能体」同样成功解出了所有 9 道埃尔德什难题。

随着底层大模型（Gemini 3.1 Pro）智能密度不断提升，简单智能体交互循环正在展现惊人效能。**预示：在绝对客观的编译器反馈锚定下，工业界可能逐渐从构建高度特化、复杂的训练系统，转向直接利用通用大模型的原生推理能力。**

## 解决的 9 个 Erdős 问题

| 问题 | 年份 | 证明概要 |
|------|------|----------|
| 问题 12 (i) | 1970 | 避免整除的密集整数集——中国剩余定理 + 避免特定算术级数构造法 |
| 问题 12 (ii) | 1970 | 更高密度极限——Behrend 风格构造法 |
| 问题 125 | 1996 | 不同进制数字集合加和密度——丢番图逼近原理 |
| 问题 138（变体） | 1981 | 颜色与数列间隔极限——贪心染色扩展 + 局部矛盾分析 |
| 问题 152 | 1994 | 西顿集孤立点——边界分析 |
| 问题 741 (i) | 1994 | 集合拆分后加和密度——肯定答案 |
| 问题 741 (ii) | 1994 | 集合拆分与间隙界限——二阶基「禁区」结构 |
| 问题 846 | 1992 | 平面点集几何悖论——无限扩展点集存在性 |
| 问题 26（变体） | 1995 | 整数倍数密度极值——迭代构造（素数序列） |

成本：最便宜问题仅 7.5-15 美元，绝大多数问题成本在几十到几百美元之间。

## 相关概念

- [[recursive-superintelligence]] — AlphaEvolve 是该文提到的 Google DeepMind 重要工作，AlphaProof 继承其演化搜索思想
- [[harness-engineering]] — AlphaProof Nexus 展示了简单 agent 交互循环的强大效能，契合「约束越多 Agent 干得越好」的 Harness Engineering 理念
- [[test-time-compute-scaling]] — 形式化推理与 LLM 推理的结合，与 Budget Forcing、自我纠错等 test-time compute 技术有深层联系
- [[agentic-code-reasoning]] — 半形式化代码推理与 Lean 形式化证明有相似的"AI 辅助验证"范式
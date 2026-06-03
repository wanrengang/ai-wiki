# Harness Engineering：复杂优化问题与 AGI 灵魂之争

> 来源：[王云鹤 - 知乎](https://zhuanlan.zhihu.com/p/2038669387150927679)，机器之心转载，2026-05-18

## 核心观点

### 1. Agent 的定义：Model + Harness

**不是** Base Model as Agent，而是：
```
Agent = Base Model（LLM/VLM/VLA）+ Harness
```
其中 Harness 包括：prompt 优化、工具调用、RAG、Memory、Safety 等组件。

### 2. Harness 会长期存在，不会被模型"吃掉"

- Rag 在升级而非消亡：加上 prompt、工具调用、知识等变成 skills
- Harness 是连接所有围绕模型的高价值元素的纽带
- 是 Agent 时代最重要的事情之一

### 3. 多模型协作产生更好的 Agent 能力

#### 原因一：模型"七国八制"
- 各家模型根据业务属性、数据、路线出现特异化
- 不同模型在具体任务上表现差异可能很大
- Claude Code 内部调用多款模型（opus、sonnet、haiku）实现综合最优解

#### 原因二：任务会"打架"
- 很多任务无法用统一 loss function 表达
- 快慢思考合一（非 prompt 切换）难以融合
- 图像超分和去模糊冲突（高通 vs 低通滤波）

#### 原因三：复杂任务需要多模型协同
- 多模态理解与生成
- 具身智能（感知、决策、运控、预测、记忆）
- 短剧生成：文案转写、视频生成、转场稳定性各需不同模型

### 4. Harness 是复杂的组合优化问题

**价值范式** = 任务价值 × 成功率 × Token 性价比

**优化目标函数**（简化表述）：
- 选择合适的模型执行每个步骤
- 调整适配该模型的 agent 组件（prompt、rag、memory、safety 等）至最优

求解手段：经验human-in-the-loop、LLM as optimizer、AutoML 算法

### 5. Model Parameters + Harness Parameters 联合优化 → AGI 新路径

```
Model Parameters ⊕ Harness Parameters → 迭代优化或联合优化
```

Anthropic 的实践迭代：opus → 4-claude code 1.0 → opus4.5 → claude code2.0 → opus4.6……

**上一时代**：给定数据集，优化模型参数

**当前时代**：给定模型，优化 Harness parameters

### 6. AGI "灵魂" 之争

当公式 1 成立时，要控制模型、选择模型，**AI 的大脑/灵魂到底在 Base Model 还是在 Harness？**

如果公式 2 成立，要基于 Harness 进一步微调模型实现自主进化，**灵魂属于谁？**

---

## 关键引用

- [r1] Trivedy, Vivek. "The Anatomy of an Agent harness." LangChain Blog, 2026
- [r2] Liu, Rui, et al. "AgentOS: From Application Silos to a Natural Language-Driven Data Ecosystem." arXiv:2603.08938 (2026)
- [r3] He, Chaoyue, et al. "Harness Engineering for Language Agents: The Harness Layer as Control, Agency, and Runtime." (2026)
- [r4] Chen, Hanting, et al. "Pre-trained image processing transformer." CVPR 2021
- [r5] Tian, Yuchuan, et al. "Instruct-ipt: All-in-one image processing transformer." arXiv:2407.00676 (2024)
- [r6] Yang, Chengrun, et al. "Large language models as optimizers." ICLR 2024
- [r7] Trivedi, Prashant, et al. "Align-pro: A principled approach to prompt optimization for llm alignment." AAAI 2025

## 标签

#Harness-Engineering #Agent #AGI #多模型协作 #优化问题

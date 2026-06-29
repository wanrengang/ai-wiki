---
title: PEEU — Planning Experience Exploration and Utilization for GUI Agents
created: 2026-06-26
updated: 2026-06-26
type: concept
tags: [agent, planning, training, gui-agent, multimodal, benchmark]
sources: [raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]
confidence: high
---

# PEEU — Planning Experience Exploration and Utilization for GUI Agents

## 概述

**PEEU（Planning Experience Exploration and Utilization）** 是清华/北大团队提出的**多模态 GUI Agent 自动探索学习框架**，通过自主探索陌生网站、从轨迹中提取规划经验，并利用 hindsight experience 合成严格对齐的高层次训练数据，提升小型 MLLM 的规划和跨网站泛化能力。^[raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]

**论文：** "Empowering GUI Agents via Autonomous Experience Exploration and Hindsight Experience Utilization for Task Planning"
**作者：** Tianyi Men, Zhuoran Jin, Pengfei Cao, Yubo Chen, Kang Liu, Jun Zhao（清华大学）
**arXiv:** 2606.27330

## 核心方法

### Planning Tree Exploration（规划树探索）
- Agent 自适应设定目标，在陌生网站上自主探索
- 用户只需提供一个 URL，框架即可自由探索网站、提取并总结经验
- 构建规划树（Planning Tree）组织探索轨迹

### Hindsight Experience Utilization（事后经验利用）
- 从轨迹中提取规划经验
- 使用 hindsight 机制将失败轨迹转化为高质量训练数据
- 构建对齐且受约束的高层次训练数据

## TDHAF：任务分解层次分析框架

**TDHAF（Task Decomposition Hierarchical Analysis Framework）** 系统性评估**任务分解的层次泛化能力**，从三个角度分析：

| 泛化维度 | 含义 |
|---------|------|
| ID Bottom-up | 同分布内，低层次技能→高层任务的泛化 |
| ID Top-down | 同分布内，高层任务→低层次技能的泛化 |
| OOD Multi-level | 跨分布（OOD），测试模型对全新网站的泛化 |

### 三级任务分解
- **Low-level（低层原子技能）**：如点击按钮、输入文本
- **Mid-level（中间任务）**：如填写表单、搜索内容
- **High-level（高层任务）**：如完成预订、提交申请

## 关键发现

1. **掌握低层原子技能 ≠ 具备高层规划能力**：单独训练低层任务不能保证高层规划能力^[raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]

2. **高层任务训练提供更好的跨网站泛化**：在真实网站上，高层任务训练比低层技能训练泛化效果更好^[raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]

3. **直接训练优于检索**：小型模型没有专门设计的 prompt pipeline 时，直接训练比 RAG 检索方法更有效^[raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]

4. **高层任务训练可.enable OOD 泛化**：使用高层任务训练可使模型在 OOD 网站上获得更强的多层次任务泛化能力^[raw/papers/2026-06-26-peee-gui-agent-2606.27330.md]

## 相关概念

- [[h-replan]]：跨设备 Agent 分层恢复框架，也关注层次化规划
- [[hypertool]]：工具增强 Agent，也关注任务分解与多步调用优化
- [[rsagent]]：多模态 Agent 工具调用，视觉推理分割

## 相关工作

- **DeepResearch Agent**：强调广泛 Web 搜索
- **WebSailor / WebShaper / WebWatcher**：信息搜索类 Web Agent
- **AWM / Agent KB / Memento / Memp**：经验积累与知识管理类 Agent

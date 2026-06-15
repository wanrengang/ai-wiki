---
title: HyperTool
created: 2026-06-15
updated: 2026-06-15
type: concept
tags: [agent, tool, inference, architecture, model]
sources: [raw/papers/2026-06-15-hyypertool-2606.13663.md]
confidence: high
---

# HyperTool

## 概述

HyperTool 是上海交大提出的新一代工具增强型 Agent 接口范式（arXiv 2606.13663），核心贡献是将工具执行的模型可见单元从原子级单次调用升级为可执行的代码块。

## 核心问题

现有 MCP 风格工具接口中，每个原子工具调用都是一个模型可见的决策点：调用→观察→写回主 trace→下一步决策。这造成执行粒度不匹配——本地确定性的工具工作流被迫展开为大量模型可见的低级数据流决策，消耗上下文并迫使模型管理底层数据流。

## 解决方案

HyperTool 提供统一的、可执行的 MCP 风格工具接口，允许模型用代码块形式调用工具，可以：
- 通过原始 schema 调用现有工具
- 在本地操作返回值和中间结果
- 将确定性工具子程序折叠为单次外部调用

```
模型调用 HyperTool
  └─ 代码块（包含工具调用序列+本地计算+中间状态传递）
      └─ 单次 outer call = 多步确定性工具执行
```

## 训练方法

从跨工具组合任务合成 HyperTool 格式轨迹，并在真实 MCP 环境中验证。

## 实验结果

在 MCP-Universe 基准上：
- Qwen3-32B：15.69% → **35.29%**（提升 2.25×）
- Qwen3-8B：9.93% → **33.33%**（提升 3.35×）
- 超越 GPT-OSS 和 Kimi-k2.5 平均准确率

## 技术细节

- **接口抽象**：HyperTool 将多步骤工具调用序列打包为单一可执行单元
- **上下文压缩**：减少 70%+ 主 trace token 消耗
- **代码级控制**：模型通过代码操控工具，而非逐步调用
- **MCP 兼容**：直接使用现有 MCP schema，不破坏生态

## 与 Related Concepts 的关系

- [[mcp]] — HyperTool 是 MCP 风格接口的进化，而非替代
- [[harness-engineering]] — HyperTool 是工具调用粒度的 Harness 优化
- [[kimi-k2-6-ai-infra]] — Kimi k2.5 被 HyperTool 超越
- [[triattention-kv-compression]] — 同为上下文效率优化，但路径不同（压缩 vs 结构重设计）
- [[test-time-compute-scaling]] — HyperTool 通过结构压缩节省推理时计算

## 延伸阅读

- 论文：https://arxiv.org/abs/2606.13663
- 作者：上海交大 Yuaxin Du, Yifan Zhou, Yujie Ge 等
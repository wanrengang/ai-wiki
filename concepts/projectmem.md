---
title: PROJECTMEM
created: 2026-06-12
updated: 2026-06-12
type: concept
tags: [memory, agent, coding, mcp, open-source]
sources: [raw/papers/2026-06-12-projectmem-ai-coding-agents.md]
confidence: high
---

# PROJECTMEM

PROJECTMEM 是一个本地优先（local-first）、事件溯源（event-sourced）的 AI 编程 Agent 记忆与判断层，由 University of Utah 的 Ripon Chandra Malo 和 Tong Qiu 提出（arXiv:2606.12329，2026年6月）。

## 核心问题

AI编程 Agent 在单会话内强大，但**跨会话无状态**（stateless）：对话窗口关闭后，Agent 丢失持久化的项目特定状态。下一次会话往往从重新阅读源文件、重新询问昨天已回答的问题、重新推导架构决策开始，而最代价高昂的失败模式是**重新尝试已经失败过的修复**。

这不是模型质量问题，而是**架构问题**：重新建立上下文每次消耗 5,000–20,000 tokens，且随对话长度增长而增长。

## 核心设计

### 1. 事件溯源记忆基质

PROJECTMEM 以**只追加的纯文本事件日志**记录开发过程，包含五种类型的事件：

- **Issue**：发现的问题
- **Attempt**：尝试的修复
- **Fix**：成功解决问题的修复
- **Decision**：架构决策
- **Note**：笔记

日志通过确定性投影（deterministic projection）生成紧凑的、AI 可读的摘要。无向量数据库、无 Embedding，可 grep/diff/git追踪。

### 2. 判断层（Judgment Layer）——Memory-as-Governance

这是 PROJECTMEM 最独特的设计：**在 Agent 执行操作之前进行预动作拦截**（pre-action gate）。

- Agent 在尝试修复前，向判断门（judgment gate）提交拟议动作
- 判断门检查：(1) 该修复是否之前已尝试过且失败？(2) 目标文件是否有抖动或开放问题的历史？
- 若任一条件为真，判断门在动作**执行前**返回警告

这将"记忆"从被动回答转变为主动治理——记忆不仅回答 Agent，还作用于其下一次动作。

### 3. 本地优先架构

- 原生 MCP 服务器（14个类型化工具）：直接通过 Model Context Protocol 为多个 MCP 客户端提供统一记忆服务
- 通用 Markdown 桥接器：为非 MCP 工具提供记忆访问
- 完全离线运行，无遥测；不可变日志同时作为可审计的来源追踪
- 默认开启密钥脱敏（API keys、密码等敏感值自动遮蔽）

## 与现有记忆系统的区别

| 系统 | 向量库 | LLM 调用 | 云端 | 主动拦截 |
|------|--------|----------|------|----------|
| MemGPT/Zep/Mem0 | ✅ | ✅ | ✅ | ❌ |
| Codified Context | ❌ | ❌ | ❌ |被动（需 Agent 主动读取） |
| **PROJECTMEM** | **❌** | **❌** | **❌** | **✅ 主动判断门** |

## 技术规格

- Python 包，仅 3 个依赖
- 14 MCP 工具
- 19 CLI 命令
- 37 自动化测试
- 开源自托管：https://github.com/riponcm/projectmem

## 评估

- 10 个项目 × 2 个月自研究
- 207 条记录事件
- Token成本分析：projectmem 的读取成本一次性写入后基本固定，而传统方式每次会话重新建立上下文消耗 5,000–20,000 tokens

## 相关概念

- [[mem0]] — 通用 AI Agent 记忆层（向量库 + 图结构，云端）
- [[mcp]] — Model Context Protocol（PROJECTMEM 的工具接口协议）
- [[mnemis-ai-long-memory]] — 微软层级图索引长期记忆框架
- [[agent-memory-systems-characterization]] — Stanford10 系统记忆表征研究
- [[siga-self-evolving-coding-agent-adapters]] — UCSD 自进化编码 Agent适配器

##局限性

- 单用户评估（尚无多用户研究）
- 判断门为二元（警告），而非自适应（建议替代方案）
- 文件级判断无法捕获细粒度行级抖动历史
- 事件模式固定，可扩展性尚未实现
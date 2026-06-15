---
title: SIGA — 自进化编码 Agent 科学模拟适配器
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [agent, architecture, self-evolution, scientific, tool-use]
sources: [raw/papers/2026-06-09-siga-self-evolving-coding-agent-adapters.md]
confidence: high
---

# SIGA — 自进化编码 Agent 科学模拟适配器

## 概述

**SIGA**（Simulator-Interface Grounding Adapter，arXiv:2606.09774，UC San Diego）是一个围绕现成编码 Agent 的轻量级封装器，解决科学模拟器界面的 Agent-tool 适配问题。

核心洞察：现代编码 Agent（Claude Code、OpenHands）已经具备文件导航、代码编辑、shell 命令执行、输出修复等通用能力，但缺少科学模拟器的**可执行契约**——词汇表、结构约束、验证规则、终止条件。SIGA 通过四个可复用组件填补这一缺口。

## 核心方法：四组件接地层

| 组件 | 功能 |
|------|------|
| **检索（Retrieval）** | 对模拟器文档、schema、示例的技术语义访问 |
| **程序记忆（Procedural Memory）** | 高频模拟器词汇和配置模式始终可见 |
| **轨迹内验证器（In-trajectory Validator）** | Agent 可调用，中途检查并修复输出 |
| **验证强制终止（Stop-hook）** | 验证成为强制终止条件，防止输出不完整或结构无效 |

该分解与模拟器无关：同一插槽在 GEOS 实例化为 XML schema 检查，在 OpenFOAM 为必需文件和字典检查，在 LAMMPS 为命令/引用/力场声明/运行控制块检查。

## 核心发现

1. **36× 加速，质量持平专家**：GEOS deck 生成， geoscience 专家新手需约 3 小时，SIGA 装备 Agent 约 5 分钟达到同等质量（TreeSim > 0.90）
2. **接地提升可靠性**：困难 held-out 集上，TreeSim 从 0.720 提升至 0.789（+10% 相对），跨种子标准差降低 16×
3. **自进化自动改进**：SIGA 从先前轨迹自动重写适配器内容（primer、memory、auxiliary skills），匹配或超越最佳手工配置
4. **主导机制随模拟器接口变化**：结构完整性瓶颈 → 验证最重要（GEOS、OpenFOAM）；领域正确性瓶颈 → 记忆和检索最重要（LAMMPS）

## 科学模拟器 DSL 背景

- **GEOS**：开源多物理场模拟器，用于地下科学（CO2 封存、储层流、压裂）。使用自定义 XML 输入 deck 语言。科学家需数小时至数天才能生成有效 deck
- **OpenFOAM**：计算流体力学开源软件
- **LAMMPS**：分子动力学开源引擎（原子系统、力场、时间积分）

## 与 [[harness-engineering]] 的关系

SIGA 是 Harness Engineering 五子系统中的 **验证（Validator）+ 记忆（Memory）** 子系统的具体实现：
- [[harness-engineering]] 提出的五子系统：Planner、Tool、Memory、Validator、Harness
- SIGA 的四组件直接对应：Validator（轨迹内验证 + 强制终止）、Memory（程序记忆）、Tool（检索）、Harness（整体包装）

## 与 [[mlevolve-self-evolving-mle-agent]] 的关系

两者都研究 Agent **自进化**能力，但方向不同：
- [[mlevolve-self-evolving-mle-agent]]：面向端到端机器学习算法发现，在 MLE-Bench 达到 65.3% 奖牌率
- SIGA：面向科学模拟器界面适配，通过轨迹自动重写适配器内容

## 与 [[agent-drift-multi-agent-systems]] 的关系

[[agent-drift-multi-agent-systems]] 研究多智能体长期运行的行为退化；SIGA 通过自进化机制持续优化适配器内容，而非被动接受退化。

## 关键意义

轻量级自进化接地层可以将通用编码 Agent 转变为科学软件的可靠操作者，将模拟器设置从数小时缩短到数分钟，且适配新模拟器无需重新设计 Agent 环——只需选择强调哪些接地插槽。
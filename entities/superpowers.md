---
title: Superpowers
created: 2026-02-05
updated: 2026-02-05
type: entity
tags: [open-source, agent, workflow, engineering, claude-code]
sources: [raw/articles/2026-02-05-Superpowers技术研究报告.md, raw/articles/2026-02-05-Superpowers日常使用指南.md]
---

# Superpowers

## 概述

Superpowers (obra/superpowers) 是一个为 AI 编码代理设计的完整软件开发工作流系统，建立在可组合的"技能"(skills)框架之上。核心目标是将 AI 编码助手从简单的"代码生成工具"升级为遵循严格工程规范的"智能流程管理伙伴"。

## 基本信息

| 属性 | 值 |
|------|-----|
| **项目名称** | obra/superpowers |
| **作者** | Jesse Vincent (@obra) |
| **开源协议** | MIT License |
| **GitHub Stars** | 44.9k ⭐ |
| **Forks** | 3.4k |
| **主要语言** | Shell (70.4%), JavaScript (19.0%), Python (5.2%), TypeScript (3.9%) |

## 核心价值

> **"Superpowers 的核心价值不是让 AI 写代码更快，而是让 AI 的产出变得可预测、可审查、可依赖。"**

## 核心技能

### brainstorming
苏格拉底式设计精炼，在任何创造性工作之前触发。通过一次一个问题精炼需求，探索多种方案并分段展示设计。

### test-driven-development
测试驱动开发，铁律：**"NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST"**

遵循 RED-GREEN-REFACTOR 循环：
1. RED: 写失败测试
2. GREEN: 最简实现
3. REFACTOR: 清理重构

### requesting-code-review
两阶段代码审查机制：
1. 规格符合性审查
2. 代码质量审查

### systematic-debugging
四阶段根因分析：
1. 观察问题
2. 形成假设
3. 验证假设
4. 修复根因

## 技术架构

### 双仓库设计
```
obra/superpowers（核心仓库）
├── skills/           # 技能定义
├── commands/         # 命令定义
├── agents/           # 代理配置
├── docs/            # 文档
└── tests/           # 测试

obra/superpowers-marketplace（插件市场）
├── .claude-plugin/  # Claude Code 插件配置
└── skills-core.js   # 共享模块
```

## 与 [[OpenCode]] 的关系

Superpowers 可以通过 OpenCode 的技能系统集成使用，其工作流理念与 OpenCode 的 Agent 编排能力互补。

## 相关概念

- [[AI-Agent]]
- [[Skills-System]]
- [[TDD]]
- [[Multi-Agent]]

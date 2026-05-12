---
title: Superpowers Claude Code 工作流
created: 2026-04-09
updated: 2026-04-09
type: concept
tags: [claude, coding, agent, workflow, tdd, debugging]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-09-Superpowers-Claude-Code编程工作流规范.md]
---

# Superpowers Claude Code 工作流

## 概述

Superpowers 是一套 Claude Code 的 skill 系统，把 AI 编程工作流规范化为五大场景：想法→实现、Bug 诊断、代码质量、多任务并行、代码审查，让 AI 成为协作伙伴而非代码生成器。

## 五大场景解决方案

| 场景 | 问题 | 解决方案 |
|------|------|----------|
| 从想法到实现 | 直接让 AI 写，返工多 | Brainstorming → Writing Plans → Executing Plans |
| 遇到 Bug | 让 AI 直接修，只修症状 | Systematic Debugging（先诊断再修复）|
| 写可靠代码 | 写完就提交，问题多 | TDD + Verification Before Completion |
| 多任务并行 | 一个一个等，浪费时间 | Dispatching Parallel Agents |
| 代码审查收尾 | 写完就合并 | Requesting/Receiving Code Review + Finishing |

## Systematic Debugging 四步法

1. **不要改代码** — 先收集信息（错误信息、规律、最近改动）
2. **形成假设** — 基于信息提出可能的假设
3. **验证假设** — 设计验证方法，加日志/测试/检查
4. **现在才修复** — 找到根本原因后再修复

## TDD 核心思想

- 先写测试，描述你想要的行为
- 测试会失败（因为代码还没写）
- 写最少的代码让测试通过
- 重构让代码更清晰

## 核心 Skill 速查表

| Skill | 用途 | 何时调用 |
|-------|------|----------|
| brainstorming | 梳理想法、形成设计 | 新功能开发前 |
| writing-plans | 写实现计划 | 设计确认后 |
| executing-plans | 执行计划 | 计划确认后 |
| systematic-debugging | 系统化调试 | 遇到 bug 时 |
| test-driven-development | 测试驱动开发 | 写代码时 |
| verification-before-completion | 提交前验证 | 准备提交时 |
| dispatching-parallel-agents | 并行任务派发 | 多个独立任务时 |
| requesting-code-review | 请求代码审查 | 代码写完后 |
| receiving-code-review | 处理审查反馈 | 收到反馈时 |

## 核心理念

> 不是让 AI 成为一个更快的代码生成器，而是让 AI 成为一个更可靠的协作伙伴。它会在写代码前先思考，在改代码前先诊断，在提交前先验证。

## 相关链接

- [[harness-engineering]] - Harness Engineering 方法论
- [[vercel-agent-browser]] - Vercel 浏览器自动化
- [[glmmodel-51]] - GLM-5.1 模型

## 标签

#ClaudeCode #Superpowers #AI编程 #工作流 #TDD #调试

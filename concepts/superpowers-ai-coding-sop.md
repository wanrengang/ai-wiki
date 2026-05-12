---
title: Superpowers AI Coding SOP
created: 2026-05-05
updated: 2026-05-05
type: concept
tags: [agent, coding, workflow]
sources: [raw/wechat-bytes-notebook-superpowers.md]
---

# Superpowers AI Coding SOP

## 概述

Superpowers 是一个开源的 AI 编程方法论项目，通过 SKILL.md 文件为 AI 编程工具（如 Claude Code、Cursor、OpenCode 等）添加稳定的工作流程规范，解决 AI"会写代码但不会干活"的问题。

核心问题：AI 模型每次都从零开始"即兴发挥"，缺乏稳定的方法论约束。

## 核心机制

通过 **SKILL.md** 文件定义工作流程 SOP，AI 遇到对应场景时自动按规则执行，而非自主发挥。

### 内置 Skills

| Skill | 功能 |
|-------|------|
| 系统化调试 | 四阶段调试法，不弄清根因不动代码 |
| TDD | 红绿重构循环，先写失败测试再写实现代码 |
| Brainstorming | 需求澄清，主动提问确认设计方向 |
| Writing Plans | 任务拆解，逐步实现 |

### 工作流程示例

装之前：直接开始写代码

装之后：
1. **Brainstorming** → 主动问需求细节（导出格式、数据量、权限等）
2. 给出 2-3 个方案等你确认
3. **Writing Plans** 拆解任务
4. 逐步实现 + verification-before-completion 把关

## 生态覆盖

| 版本 | 工具支持 | 特色 |
|------|----------|------|
| 英文原版 | 6 款（Claude Code, Cursor, Codex, Copilot CLI, Gemini CLI, OpenCode） | GitHub Actions CI |
| superpowers-zh | **17 款**（+ Qwen Code, Trae, Kiro, **Hermes Agent**, 通义灵码等） | Gitee/Coding CI、中文代码审查 |

## 相关项目

- [[superpowers-zh]] - 国内开发者适配版
- [[claude-code]] - 主要支持的 AI 编程工具
- [[opencode]] - 支持的工具之一

## 参考

- 项目地址：github.com/jnMetaCode/superpowers-zh
- 原版：github.com/jessejp/superpowers

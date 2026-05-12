---
title: OpenCode 工具链铁三角
created: 2026-05-12
updated: 2026-05-12
type: comparison
tags: [agent, coding, engineering, comparison]
sources: [raw/opencode-iron-triangle-2026-03-31.md]
---

# OpenCode 工具链铁三角

## 概述

OpenCode 生态的三个核心工具形成"铁三角"协作模式，各司其职、互补配合：

| 工具 | 层级 | 角色 | 核心职责 |
|------|------|------|----------|
| [[OpenSpec]] | 规范层 | 项目管理员 | 定义"做什么"，管理变更文档 |
| [[Superpowers]] | 能力层 | 任务指挥官 | 决定"谁来作"，拆解并行任务 |
| [[OMO]] | 基础设施层 | 工具执行者 | 执行"怎么做"，提供底层工具 |

## OpenSpec（规范层）

**核心理念**：规范驱动开发 — "Agree before you build, human and AI align on specs before any code gets written."

**核心设计**：`specs/` 和 `changes/` 分离，类似 Git 的 main 分支和功能分支。

**四个核心命令**：
- `/opsx:explore` — 探索模式，只讨论不产出文档
- `/opsx:propose` — 一次性生成所有规划文档
- `/opsx:apply` — 按 tasks.md 逐项实施代码
- `/opsx:archive` — 归档完成的变更

**安装**：`npm install -g @fission-ai/openspec@latest`

## Superpowers（能力层）

**核心理念**：强制工作流 — AI 必须按固定步骤走，把软件工程最佳实践变成 AI 必须遵守的规则。

**7步核心工作流**：
1. `brainstorming` — 头脑风暴，苏格拉底式提问
2. `using-git-worktrees` — 创建隔离 Git 工作空间
3. `writing-plans` — 拆解成 2-5 分钟的小任务
4. `subagent-driven-development` — 派子智能体执行，两阶段审查
5. `test-driven-development` — 强制 TDD，不写测试就删代码
6. `requesting-code-review` — 代码审查，关键问题阻塞进度
7. `finishing-a-development-branch` — 完成分支，提供合并选项

**数据**：GitHub 11.5万星，官方插件市场 23万次安装，Claude Code 第三方插件首位。

**强制 TDD**：测试覆盖率从不到 30% 提升到 85%-95%。

## OMO（基础设施层）

**核心理念**：多智能体协作和并行执行，把 OpenCode 底层能力拉满。

**AI 团队成员**：
- [[Sisyphus]] — 团队领袖，规划、调度、最终代码合并
- [[Oracle]] — 架构师，分析架构、评估风险、技术决策
- [[Librarian]] — 代码库专家，查阅文档、搜索代码
- [[Explore]] — 侦察兵，极速扫描代码库
- [[Frontend]] — 前端工程师
- [[Document-Writer]] — 技术作家

**核心功能**：
- 直接用 `@oracle` 调用某个专家
- 魔法词 `ulw`（UltraWork）激活全部增强功能
- 后台并行任务，结果自动汇总

## 配合工作流

```
提案创建（OpenSpec）
    ↓
需求澄清（Plan Agent 动态调整）
    ↓
并行实现（Superpowers + OMO）
    ↓
测试验证（pytest）
    ↓
归档（OpenSpec 闭环）
```

## 使用场景对照

| 场景 | 推荐工具 |
|------|---------|
| 快速原型验证 | OMO |
| 核心业务代码 | Superpowers |
| 团队协作、需要文档 | OpenSpec |
| 生产级项目开发 | 三者协同 |
| 简单 bugfix | 原生 OpenCode |

## 相关工具

- [[OpenCode]] — 基础平台
- [[OpenSpec]] — 规范驱动
- [[Superpowers]] — 流程强制
- [[OMO]] — 多智能体调度
- [[TDD]] — 测试驱动开发

---

^[[raw/opencode-iron-triangle-2026-03-31.md]]

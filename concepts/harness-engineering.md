---
title: Harness Engineering
created: 2026-04-08
updated: 2026-06-07
type: concept
tags: [agent, engineering, workflow, llm, prompt-engineering]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-08-GLM-51电商风格迁移项目实战-甲木.md, raw/articles/2026-06-06-harness.md, raw/articles/2026-06-07-harness-engineering-ai-success-rate.md]
---

# Harness Engineering

## 概述

Harness Engineering（HE）是一种面向 AI Agent 的工程方法论，是 PE（Prompt Engineering）和 CE（Context Engineering）的升级。

## 演进路径

```
PE（Prompt Engineering）→ CE（Context Engineering）→ HE（Harness Engineering）
```

三者是套娃关系：HE 包着 CE，CE 包着 PE。

## 核心思想

> **约束越多，Agent 反而干得越好**

> **好的管理者不是控制欲最强的那个人，而是环境设计得最好的那个人**

## 本质

不是给模型写 prompt，而是给模型搭建**完整的工作环境**：

| 要素 | 说明 |
|------|------|
| 预期对齐 | 先讲背景，让模型复述理解 |
| 定义文档（PRD） | 明确用户、场景、功能优先级 |
| 设计架构 | 关注核心模块设计思路 |
| 分步交付 + 检查点 | 逐步编码 + 自检查 |

## 实践案例：StyleForge 电商风格迁移

| 阶段 | 内容 |
|------|------|
| STEP 1 | 预期对齐 - 模型复述理解 |
| STEP 2 | 产品设定 + PRD |
| STEP 3 | 技术方案设计 |
| STEP 4 | 逐步编码（模型自行主导） |
| STEP 5 | 联调测试 + 收尾 |

### 最终成果
- 1246 次 AI 自行执行轮次
- 6000 万 tokens 消耗
- 4-5 小时总开发时长
- 完整前后端系统，可直接交付

## 关键洞察

1. **开源模型也能稳定完成长程任务** - GLM-5.1 验证了 8 小时持续工作能力
2. **AI 编程门槛进一步降低** - 中小商家也能获得专业级设计能力
3. **技术落地关键** - 好的 AI 技术要实际落在业务场景中

## 五子系统（Harness 核心架构）

Harness 围绕智能体的一整套工程基础设施，由五个子系统组成，每一个对应一种具体失败模式：

| 子系统 | 作用 | 解决什么问题 |
|--------|------|-------------|
| **指令子系统** | 仓库根目录 AGENTS.md/CLAUDE.md | 智能体不知道项目约定，瞎写代码 |
| **工具子系统** | 环境权限、工具版本锁定 | 工具调用失败、环境不一致 |
| **状态子系统** | PROGRESS.md 任务状态跟踪 | 执行过程不可追踪 |
| **反馈子系统** | 测试循环、验证门禁 | 写完代码不知道对不对 |
| **环境子系统** | Docker/setup.sh 环境隔离 | 环境差异导致执行失败 |

## 核心实验证据

### Anthropic 实验（9美元 vs 200美元）

- 同一个 Opus 4.5 模型，同一道编程题
- 裸跑：9美元，效果全废
- 加 Harness：200美元，效果稳定交付
- **多花的191美元，全花在验证循环上** — 每写一段代码就跑测试，不通过就改，直到真正通过

### OpenAI Codex 实验（百万行仓库）

- 同一个模型，同一个真实代码仓库
- 实验只改了一件事：仓库根目录加了一个 AGENTS.md 文件（不到100行 markdown）
- 结果：代码质量显著提升

### 关键教训

> 没有反馈循环，Harness 等于没装 — 这是 Anthropic 9美元实验的核心教训：前四步全做对，第五步（反馈子系统）缺位，依然全废。

## 企业落地五步法

```
第1步·根目录建 AGENTS.md
  → 至少三块：项目说明、禁止操作、完成定义
第2步·配 permissions
  → .claude/settings.json 或 ~/.codex/config.toml
第3步·写 setup.sh 锁环境
  → 所有版本写死，pnpm install --frozen-lockfile
第4步·建 PROGRESS.md
  → 已完成/进行中/待办/已知问题
第5步·固化完成定义
  → pnpm type check/test/lint/build，退出码不为0不算完成
```

## 关键洞察

1. **开源模型也能稳定完成长程任务** - GLM-5.1 验证了 8 小时持续工作能力
2. **AI 编程门槛进一步降低** - 中小商家也能获得专业级设计能力
3. **技术落地关键** - 好的 AI 技术要实际落在业务场景中
4. **先装 Harness，再等下一个模型** - 2026年 Anthropic 和 OpenAI 的两组实验给出同一个答案：别先换模型，先把 Harness 装好

## 相关链接

- [[agent-production-evaluation]] - Agent 生产评估三层次（上线前/运行中/事故后）
- [[enterprise-skill-architecture]] - 企业级 Skill 体系
- [[test-time-compute-scaling]] - 推理时计算扩展（对比参考）

## 标签

#HarnessEngineering #Agent #LLM #工程方法论 #AI编程

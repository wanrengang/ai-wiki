---
title: Harness Engineering
created: 2026-04-08
updated: 2026-04-08
type: concept
tags: [agent, engineering, workflow, llm, prompt-engineering]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-08-GLM-51电商风格迁移项目实战-甲木.md]
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

## 相关链接

- [[glmmodel-51]] - GLM-5.1 模型
- [[superpowers-claude-code]] - Claude Code 编程工作流
- [[enterprise-skill-architecture]] - 企业级 Skill 体系

## 标签

#HarnessEngineering #Agent #LLM #工程方法论 #AI编程

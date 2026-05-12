---
title: GLM-5.1 模型
created: 2026-04-08
updated: 2026-04-08
type: entity
tags: [zhipu, model, open-source, coding, long-horizon, engineering]
sources: [/run/media/wrg/mywork/data/my-obsidian/02AI与技术/2026-04-08-GLM-51电商风格迁移项目实战-甲木.md]
---

# GLM-5.1 模型

## 概述

智谱 AI 发布的开源大模型，全球第一个在真实工程任务中验证 8 小时持续工作能力的开源模型。开源编程能力 SOTA，面向 Long Horizon 长程任务的产物。

## 关键数据

| 指标 | 数据 |
|------|------|
| 开源编程能力 | SOTA |
| SWE-Bench | 开源第一 |
| Artificial Analysis | 开源第一 |
| OpenRouter 调用量 | 开源前列 |

## 核心优势

| 优势 | 说明 |
|------|------|
| 长程任务稳定性 | 跨步骤、跨工具、持续数小时推进 |
| 自主排查修复 | 遇到 bug 自动修正，API 异常自动重试 |
| 上下文一致性 | 始终记得前期定的架构和约束，不跑偏 |

## 评测数据

- **SWE-Bench**：开源第一
- **Artificial Analysis**：开源第一
- **OpenRouter**：调用量开源前列

## 实践案例：StyleForge 电商风格迁移

### 场景痛点
- **详情页就是销售员**：优质详情页可提升转化率 30%-50%
- **设计资源严重不足**：中小商家缺乏设计师，但商品多、上新快

### 方案：视觉 DNA 迁移
从 9 个维度拆解参考图设计风格（布局、色彩、光影、排版、情绪调性等），然后迁移到自有产品。

### 最终成果
- 1246 次 AI 自行执行轮次
- 6000 万 tokens 消耗
- 4-5 小时总开发时长
- 完整前后端系统，可直接交付

## 核心方法论：Harness Engineering

> PE（Prompt Engineering）→ CE（Context Engineering）→ HE（Harness Engineering）

### 关键洞察
- **约束越多，Agent 反而干得越好**
- 好的管理者不是控制欲最强的那个人，而是**环境设计得最好的那个人**

### 本质
不是给模型写 prompt，而是给模型搭建**完整的工作环境**：
- 对齐预期
- 定义文档（PRD）
- 设计架构
- 分步交付 + 检查点

## 思考

1. **开源模型也能稳定完成长程任务**，AI 编程门槛进一步降低
2. **电商详情页 AI 化生产是增量市场**，中国千万级中小商家的刚需
3. **技术落地关键**：好的 AI 技术要实际落在业务场景中

## 相关资源

- GLM-5.1：Hugging Face / ModelScope 开源
- 支持 Claude Code、OpenCode 等主流开发工具

## 相关链接

- [[harness-engineering]] - Harness Engineering 方法论
- [[superpowers-claude-code-workflow]] - Superpowers Claude Code 工作流
- [[aifut-conference]] - AIFUT 大会

## 标签

#GLM-5.1 #AI编程 #Harness-Engineering #电商 #风格迁移 #开源

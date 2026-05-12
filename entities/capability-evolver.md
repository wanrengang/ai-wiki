---
title: Capability Evolver
created: 2026-03-03
updated: 2026-03-03
type: entity
tags: [AI-agent, self-evolution, OpenClaw, GEP]
sources: [raw/articles/2026-03-03-Capability-Evolver-自我进化引擎深度解析.md]
---

# Capability Evolver

## 概述

Capability Evolver 是一个革命性的 meta-skill（元技能），允许 AI Agent 检查自身的运行历史，识别故障和低效环节，并**自主地**进化改进。

核心理念：**"Evolution is not optional. Adapt or die."**

## 核心架构

核心是 **GEP (Genome Evolution Protocol，基因组进化协议)**，管理 AI Agent 的进化过程。可以将其理解为**AI 行为的 Git 提交系统**——每一次变更都被门控、追踪且可复用。

## 三大核心对象

### 1. Genes（基因）
可复用的进化策略，每个 Gene 包含：
- **id**：唯一标识符
- **trigger**：触发条件
- **action**：具体的改进动作
- **evidence**：支持该改进的证据

```json
{
  "id": "fix-json-parse-error",
  "trigger": "JSON parse error in user input",
  "action": "Add try-catch wrapper with user-friendly error message",
  "evidence": ["error.log:2026-03-01", "user-feedback:json-failures"]
}
```

### 2. Capsules（胶囊）
封装的功能模块，可以独立部署和测试：
- 包含一组相关的 Genes
- 提供完整的改进方案
- 支持原子性的版本控制

### 3. Events（事件）
进化事件的时间序列日志：
- 记录每次进化的触发原因
- 追踪应用的具体变更
- 评估进化后的效果

## 工作流程

```
[运行时历史] → [日志分析引擎] → {识别问题}
                                    ↓
                              [匹配 Gene]
                                    ↓
                              [生成进化提案]
                                    ↓
                         {审核模式: 自动/人工}
                                    ↓
                         [记录到 Events]
                                    ↓
                         [更新 Genes/Capsules]
```

## 三种运行模式

### 1. 全自动模式（Mad Dog Mode）
```bash
node index.js
```
⚠️ 无需确认，直接应用变更，适合沙盒环境或测试场景。

### 2. 人工审核模式（推荐）
```bash
node index.js --review
```
✅ 生成进化提案，等待人工确认，安全可控。

### 3. 持续进化模式
```bash
node index.js --loop
```
🔄 持续监控并周期性进化，需要适当的沙盒隔离。

## 核心价值

1. **变"临时调整"为"制度化记忆"**
   - 可审计的知识库（Genes, Capsules）
   - 可复用的进化资产
   - 从失败中学习并持久化

2. **自动化能力硬化**
   - 自动检测重复性错误模式
   - 生成对应的 Gene
   - 下次遇到相同问题直接应用修复

3. **机构记忆的构建**
   - 新 Agent 继承所有历史 Genes
   - 避免"重复发明轮子"
   - 累积式的能力增长

## 性能指标

| 指标 | 数值 |
|-----|------|
| 下载量 | 4,822 |
| 安装量 | 60 |
| 社区评分 | 4.0/5.0 |
| GitHub Stars | 1,064 |
| Forks | 355 |

## 相关概念

- [[AI-Agent]]
- [[OpenCLAW]]
- [[Self-Evolution]]
- [[GEP-Protocol]]

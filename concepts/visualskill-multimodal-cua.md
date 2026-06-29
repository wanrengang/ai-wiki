---
title: VisualSkill 多模态技能架构
created: 2026-06-19
updated: 2026-06-19
type: concept
tags: [agent, multimodal, skill, computer-use, inference, architecture]
sources: [raw/articles/2026-06-19-visualskill-multimodal-skills-cua.md]
confidence: high
---

# VisualSkill 多模态技能架构

## 概述

VisualSkill 是 UC Santa Barbara + MIT CSAIL + MIT-IBM Watson AI Lab 提出的多模态技能框架，专为 Computer-Use Agents（CUA）设计，解决传统纯文本技能库无法捕捉 GUI 交互视觉本质的问题。

论文：arXiv 2606.18448 | 代码：https://github.com/XMHZZ2018/VisualSkills

## 核心问题

CUA 在标准化基准上接近人类水平，但在长时程任务和未见软件上仍困难，原因之一是：现有技能库将技能artifact表示为纯文本，但 GUI 交互本质上是视觉的。

纯文本技能的两个局限：
1. **UI 元素识别困难**：工具栏图标、面板、空间关系等用文字描述Verbose且歧义多
2. **中间状态验证困难**：多步工作流需要验证每步后是否到达预期UI状态，文本只能间接描述

## 架构

### 技能定义

每个目标应用一个 VisualSkill，结构为两层：

- **skill.md 索引**：列出每个主题 + 一句 "when to use" 描述
- **per-topic guides** `g_t = (p_t, F_t)`：文本主体 + UI 截图集合
- **load_topic(t) MCP 工具**：按需获取主题指南，内联返回文本+图片

### 两阶段构建

**Stage 1 — 文档挖掘**：
- 解析官方用户手册（PDF/HTML）的目录作为 topic 集合
- 从文档中提取每个 topic 的文字内容和 vendor 原始截图
- 一次 LLM 调用同时生成多模态版本和纯文本版本

**Stage 2 — 实时 UI 探索**：
- **Free Explorer**：Opus-class planner 将空闲应用截图划分为 K=8 个区域，每个区域启动 Sonnet-class worker 在隔离 Docker 中探索、截图并输出结构化笔记
- **Trajectory-Targeted Explorer**：reviewer agent 阅读失败轨迹，定位 Stage 1 技能覆盖不足的 UI 区域，派遣第二波 worker 针对性探索
- Assembler 将所有截图和笔记合并到对应 topic 技能文件中

### 关键发现：MCP 工具 vs 直接 Read

直接 Read：每任务仅加载 ~0.8 张图，2% 步数后停止使用技能  
MCP load_topic 工具：100% 加载率，每任务 7.9 张图，全程使用  
→ MCP 接口将图片内联到文本中单次返回，保持技能全程可访问

## 核心结果

| 配置 | CUA-World + OSExpert-Eval 平均 |
|------|------|
| No-skill baseline | 0.303 |
| Stage 1 VisualSkill | 0.363 (+6.0) |
| Stage 1 text-only | 0.344 |
| Stage 2 VisualSkill | **0.456 (+15.3)** |
| Stage 2 text-only | 0.373 |

Stage 2 VisualSkill vs 匹配 text-only：**+8.3 pt**（同为 Stage 2，只有 modality 不同）

多模态优势最大的场景：
- 视觉密集型创意工具：GIMP (+16.6pt), OpenToonz (+8.9pt), Tableau (+15.0pt)
- 静默失败：表单字段看起来接受了值但提交时被拒绝

## 关键洞察

- **图片的作用在 UI 探索后才最大化**：Stage 1 多模态仅 +1.9pt，Stage 2 +8.3pt——文档覆盖不完整时 live exploration 补充的视觉知识价值最大
- **短而明确的任务不需要图片**：当文本已足够清晰时，图片的边际价值消失
- **Skill 是 MCP 工具的实例**：这个模式与 OpenClaw Skills 生态高度对齐，属于同一技术方向

## 相关概念

- [[skillweaver-compositional-skill-routing]] — 阿里云多技能组合路由框架（同期工作，另一条 skill 路线）
- [[awesome-openclaw-skills]] — OpenClaw Skills 生态（SkillWeaver 属于同一 skill 系统生态）
- [[github-mcp]] — MCP（Model Context Protocol）协议（VisualSkill 的 load_topic 是 MCP 工具调用）
- [[claude-managed-agents]] — Claude Agent 托管平台
- [[agent-drift-multi-agent-systems]] — 多智能体长期运行行为退化（与 CUA 评估相关）
- [[harness-engineering]] — Harness Engineering（技能约束影响 Agent 表现）

## 参考

- 论文：https://arxiv.org/abs/2606.18448
- 代码：https://github.com/XMHZZ2018/VisualSkills

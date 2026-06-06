---
title: GStack — YC CEO 的 AI 软件工厂
created: 2026-05-09
updated: 2026-05-09
type: entity
tags: [AI, coding, YC, gstack, open-source, agent]
sources: [https://mp.weixin.qq.com/s/tPCRmEWFler5C2vInggLeg]
confidence: medium
---

# GStack — YC CEO 的 AI 软件工厂

## 核心数据

- **GitHub Stars**: 9万+
- **声称效率**: 400x logical LOC（从初声称100倍修正）
- **角色**: 23个专业虚拟角色
- **工具模块**: 8个
- **协议**: MIT

## 是什么

GStack 是"AI 软件工厂"系统，将 Claude Code、Codex 等模型组织成虚拟工程团队，包含 CEO、工程师、QA（真实浏览器测试）、安全官等角色。

## 核心理念：Thin Harness, Fat Skills

Harness（执行框架）要轻量化，真正的价值在 Skills（技能层）。Skills 本质上是 Markdown 编写的结构化工作流。Garry Tan 认为：Markdown 正在成为新的"编程语言"。

## 关键观点

1. **400x 效率提升真实可测量**：用 logical LOC 而非原始代码行数
2. **真实案例**：用200美元 Claude Code Max 订阅，1人5天完成过去400万美元、6-7人1.5年才能建成的项目
3. **80%-90% 测试覆盖率是底线**：AI 代码最大风险是"80%能跑，用户一碰就崩"
4. **"Boil the Ocean"**：AI边际成本极低，砸token换更完整结果
5. **Mini AGI 已出现**：Browse 技能 = 长期 HTTP daemon + 70 CLI 命令
6. **AI 是意志的放大器**：AI不会替你产生"关心"，agency必须来自你自己
7. **自信放大器问题**：Mo Bitar 指出大模型更像是"自信放大器"而非能力放大器

## 争议

- 社区认为核心理念不新鲜，真正有用的只是 /qa 和 /browse 真实浏览器测试
- Mo Bitar："Garry 开源的是一堆 Markdown 提示词文件夹"
- "每天1-2万行代码"被Reddit视为虚荣指标

## 相关链接

- 原始文章: [微信链接](https://mp.weixin.qq.com/s/tPCRmEWFler5C2vInggLeg)
- Raw Source: `raw/articles/2026-05-09-gstack-yc-ceo-ai-coding.md`
- GitHub: garrytan/gstack

## 相关概念

- [[claude-code]]
- [[openclaw]]
- [[agent-engineering]]
- [[harness-engineering]]
---
title: Awesome OpenClaw Skills
created: 2026-03-08
updated: 2026-03-08
type: concept
tags: [openclaw, skills, ecosystem, curation, security]
sources: [raw/articles/Awesome-OpenClaw-Skills项目深度研究报告.md]
confidence: high
---

# Awesome OpenClaw Skills

> VoltAgent 维护的 OpenClaw 技能精选列表，从 13,729 个技能中精选 5,494 个。

## 概述

awesome-openclaw-skills 是一个 Awesome List 风格的精选列表，由 VoltAgent 维护。它解决 ClawHub 注册表技能过多难以发现的问题，提供质量筛选、安全过滤和清晰分类。

## 关键数据

| 指标 | 数值 |
|------|------|
| 收录技能总数 | 5,494 个 |
| 原始注册表技能数 | 13,729 个 |
| 过滤掉技能数 | 6,940 个（50.5% 过滤率） |
| 分类数量 | 30 个 |
| 恶意技能过滤 | 373 个 |

## 热门分类 TOP 10

| 排名 | 分类 | 数量 | 占比 |
|------|------|------|------|
| 1 | Coding Agents & IDEs | 1,218 | 22.2% |
| 2 | Web & Frontend Development | 937 | 17.1% |
| 3 | DevOps & Cloud | 408 | 7.4% |
| 4 | Search & Research | 350 | 6.4% |
| 5 | Browser & Automation | 334 | 6.1% |
| 6 | Productivity & Tasks | 206 | 3.7% |
| 7 | AI & LLMs | 197 | 3.6% |
| 8 | CLI Utilities | 186 | 3.4% |
| 9 | Image & Video Generation | 171 | 3.1% |
| 10 | Git & GitHub | 169 | 3.1% |

## 筛选标准

| 过滤类型 | 数量 |
|----------|------|
| 垃圾/批量账号 | 4,065 |
| 重复/相似名称 | 1,040 |
| 非英语描述 | 604 |
| 加密货币/金融相关 | 573 |
| 恶意技能 | 373 |
| 描述不足 | 247 |
| ERC/x402/a2a 协议 | 38 |

## 安全政策

- 只收录 ClawHub 未被标记为可疑的技能
- 与 VirusTotal 合作提供安全扫描
- 接受社区举报和审查

## 安装方式

```bash
# ClawHub CLI（推荐）
npx clawhub@latest install <skill-slug>

# 手动安装
cp -r skills/<skill-name> ~/.openclaw/skills/
```

## 发展趋势

- **短期**：技能数量持续增长，预计突破 6,000
- **中期**：更多垂直领域（医疗、法律、教育）
- **长期**：Agent 互操作性协议标准化

## 相关链接

- GitHub: https://github.com/VoltAgent/awesome-openclaw-skills

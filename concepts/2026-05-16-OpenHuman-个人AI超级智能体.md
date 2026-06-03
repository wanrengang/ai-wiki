# OpenHuman：个人AI超级智能体深度研究报告

- **来源**: 微信公众号 - 量子位
- **日期**: 2026-05-16 14:00
- **链接**: https://mp.weixin.qq.com/s/YC4Xt8yKk3yxpIqJMwBZlw

## 项目信息

- **GitHub**: https://github.com/tinyhumansai/openhuman
- **官网**: https://www.tinyhumans.ai/openhuman
- **许可**: GNU GPL3
- **Star**: 9k+（持续霸榜GitHub Trending第一）
- **技术栈**: Rust + Tauri v2 + React
- **状态**: Early Beta

## 核心定位

OpenHuman 是一个**个人AI超级智能体**，核心理念：让AI在几分钟内了解你的一切，建构卡帕西式本地知识库，不需要训练期，第一天上班就能干活。

## 三大核心能力

### 1. 连接 → 118+ 第三方服务一键接入
- Gmail、GitHub、Slack、Notion、Stripe、Calendar、Drive、Linear、Jira等
- 一键OAuth授权，不需要手动生成API Key

### 2. 抓取 → 20分钟自动同步
- 每20分钟自动轮询所有已连接账户
- 新邮件、日程变更、代码提交、文档更新全部拉到本地

### 3. 记忆树（Memory Tree）→ 卡帕西式知识库
- 不是向量数据库，而是确定性、层级化的记忆管道
- 三级摘要树：Source Tree / Topic Tree / Global Tree
- 存储：SQLite + Obsidian兼容Markdown vault

## TokenJuice

智能Token压缩，三层规则叠加（内置+用户+项目），节省80% Token消耗。

## 技术架构

```
React + Tauri 桌面应用
        │ JSON-RPC
┌─────────────────────┐
│  Rust Core          │
│  • Memory Tree      │
│  • Integration适配器│
│  • TokenJuice压缩   │
│  • Provider Router  │
│  • Native Tools     │
│  • Voice (STT/TTS)  │
└─────────────────────┘
```

## 与其他Agent对比

| 特性 | Claude Cowork | OpenClaw | Hermes | **OpenHuman** |
|------|--------------|----------|--------|--------------|
| 开源 | 否 | MIT | MIT | **GPL3** |
| 记忆 | 聊天范围 | 依赖插件 | 自学习 | **Memory Tree** |
| 集成 | 少量 | 自带 | 自带 | **118+ OAuth** |
| 自动抓取 | 无 | 无 | 无 | **20分钟自动** |

## 相关项目

- AgentMemory: https://github.com/rohitg00/agentmemory
  - OpenHuman可作为其Memory Backend
  - 支持Claude Code、Codex、Cursor、OpenClaw等跨Agent记忆共享

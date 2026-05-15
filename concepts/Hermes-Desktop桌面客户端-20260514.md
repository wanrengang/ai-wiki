---
title: 告别命令行：Hermes Desktop 让你的Hermes Agent助手走进桌面
source: 微信公众号 - 小哥聊AI
date: 2026-05-14
tags: [Hermes, Hermes Desktop, Nous Research, AI Agent, 开源, Electron]
---

# 告别命令行：Hermes Desktop 让你的Hermes Agent助手走进桌面

**来源**: 小哥聊AI  
**日期**: 2026-05-14

---

## 概述

Hermes Desktop 是 Nous Research 为 Hermes Agent 打造的图形化桌面客户端，基于 Electron 构建，跨平台支持 macOS/Linux/Windows。

**定位：** 把 Hermes Agent 从命令行带进桌面，降低使用门槛的同时保留完整功能深度。

---

## 为什么需要桌面客户端？

Hermes Agent 的安装和使用完全依赖命令行，对非技术用户存在门槛。Hermes Desktop 通过 GUI 整合了安装、配置、聊天、会话管理等功能。

---

## 核心功能

| 功能 | 说明 |
|------|------|
| 引导式安装 | 首次启动自动检测 Hermes 是否已安装，全流程 GUI 引导，零命令行 |
| 多提供商支持 | OpenRouter、Anthropic、OpenAI、Google Gemini、xAI Grok、Qwen、MiniMax、Hugging Face、Groq 等；支持 LM Studio、Ollama、vLLM、llama.cpp 等本地模型 |
| 流式聊天界面 | 基于 SSE 实时流式响应，Markdown 渲染、代码高亮、工具进度指示器 |
| 会话管理 | SQLite FTS5 全文搜索，按日期分组的历史记录，随时恢复对话 |
| 档案切换 | 创建和切换多个隔离的 Hermes 环境，不同项目、不同人格互不干扰 |
| 记忆系统 | 可视化编辑记忆条目和用户画像，支持容量追踪 |
| 14 套工具集 | Web、浏览器、终端、文件操作、代码执行、视觉、图像生成、TTS、技能、记忆、会话搜索等 |
| 16 种消息网关 | Telegram、Discord、Slack、WhatsApp、微信、企业微信、飞书、钉钉、Email 等全覆盖 |

---

## 工作原理

1. 首次启动时检测 `~/.hermes` 目录是否存在
2. 不存在则调用官方安装脚本（`--skip-setup`），在 GUI 中完成提供商配置
3. 用户选择 API 提供商或本地模型端点，配置保存到 `config.yaml` 和 `.env`
4. 日常使用时，聊天请求通过本地 Hermes CLI 发出，桌面应用将响应流式回传到 UI

**本质：** Hermes Desktop 是 GUI 前端，后端完全依赖 Hermes Agent 的 CLI 能力。

---

## 界面模块（8个）

- 💬 **Chat** — 流式对话，支持 22 个斜杠命令（`/new`、`/web`、`/code`、`/image` 等）
- 📋 **Sessions** — 浏览并恢复历史会话，支持全文搜索
- 👤 **Agents** — 管理和切换活动档案
- ⚡ **Skills** — 查看内置和已安装的技能
- 🎭 **Persona** — 编辑当前档案的人格设定
- 🧠 **Memory** — 查看和编辑记忆文件
- 🔧 **Tools** — 启用或禁用工具集
- ⚙️ **Settings** — 提供商和网关配置

另有定时任务管理器（Schedules）和网关控制（Gateway）等进阶功能。

---

## 安装方式

| 平台 | 方式 |
|------|------|
| macOS | 下载 .dmg，执行 `xattr -cr` 清除隔离属性 |
| Linux | .AppImage（通用）或 .deb（Debian/Ubuntu） |
| Windows | .exe 安装包，未来支持 winget |
| Fedora | .rpm 格式，`dnf install` |

支持自动更新（electron-updater）。

---

## 技术栈

- **框架：** Electron + Vite
- **测试：** Vitest
- **数据存储：** `~/.hermes` 目录（配置文件、会话数据库、cron 任务）
- **协议：** MIT

**当前状态：** 1200+ Stars，185+ Forks

---

## 亮点特性

- 📊 **Token 用量追踪** — 实时显示 prompt 和 completion 的 token 数量及费用
- 🌍 **本地/远程双模式** — 本地运行或连接远程 Hermes API 服务器
- 📦 **备份与恢复** — 完整数据备份和恢复
- 🛠️ **系统诊断** — 内置调试工具
- 🏢 **Hermes Office (Claw3d)** — 可视化 3D 界面，支持开发服务器管理

---

## 项目地址

- GitHub: github.com/fathah/hermes-desktop
- Hermes Agent: github.com/NousResearch/hermes-agent

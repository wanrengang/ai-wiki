# OpenCut — 开源视频剪辑工具（CapCut替代品）

## 基本信息

| 项目 | 内容 |
|------|------|
| **名称** | OpenCut |
| **GitHub** | https://github.com/OpenCut-app/OpenCut |
| **当前Star** | 58,711 ⭐ |
| **Fork** | 6,397 |
| **语言** | TypeScript + Rust |
| **协议** | MIT License |
| **赞助商** | fal.ai, Vercel |

## 项目定位

> **"The open-source CapCut alternative"**

OpenCut 是一个免费、开源的跨平台视频编辑器，目标是成为剪映（CapCut）的开源替代品。

## 核心特点

### 为什么需要 OpenCut？

1. **隐私问题**：剪映是闭源软件，视频素材上传到云端处理，数据安全无法保障
2. **收费问题**：以前免费的功能（高级特效、转场、去水印导出）现在都要付费会员
3. **传输耗时**：上传下载浪费时间

### OpenCut 优势

- ✅ **完全免费**，无水印
- ✅ **MIT 开源协议**
- ✅ **隐私优先**：视频留在本地设备
- ✅ 支持 Web、桌面端、移动端
- ✅ 支持 AI 功能（MCP server for AI agents）
- ✅ 支持插件体系（Plugin-first architecture）
- ✅ 支持脚本编辑（内置 scripting tab）
- ✅ 支持自动化/批量渲染（Headless mode）

## 技术架构

### 新版本（重写中）

正在从零开始重写，特点：

- **Editor API** — 开放的编辑器 API
- **Plugin 架构** — 一级第三方插件支持
- **统一代码库** — Web + Desktop + Mobile 共用 Rust 核心
- **MCP Server** — 支持 AI Agent 集成
- **Headless 模式** — 支持自动化渲染
- **脚本标签页** — 编辑器内置脚本功能

### 旧版（Classic）

已归档，当前可用的稳定版本。

技术栈：
- `apps/web/` — Next.js Web 应用
- `apps/desktop/` — 原生桌面端（GPUI）
- `rust/` — 跨平台核心：GPU 合成器、特效、遮罩、WASM 绑定

### 技术依赖

- **运行时**：Bun
- **数据库**：PostgreSQL（Docker）
- **缓存**：Redis
- **构建**：moonrepo

## 最新版本

**v0.3.0** (2026-04-15) 更新内容：
- 新增遮罩（masks）
- 曲线关键帧编辑器
- 音量/速度控制
- 预览缩放
- 画布背景
- 贴纸功能

## 相关项目

| 项目 | 说明 |
|------|------|
| `OpenCut-app/OpenCut` | 主项目（正在重写） |
| `OpenCut-app/OpenCut-classic` | 旧版归档 |
| `SysAdminDoc/OpenCut` | Premiere Pro 扩展，AI 视频编辑/字幕/音频处理 |
| `jtydhr88/ComfyUI-OpenCut` | ComfyUI 插件 |

## 官方网站

- 当前版：https://opencut.app
- 新版（重写中）：https://new.opencut.app

## 社交

- Discord：https://discord.gg/zmR9N35cjK
- X：@opencutapp

---

*最后更新：2026-06-22*

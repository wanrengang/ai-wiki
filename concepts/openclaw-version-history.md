---
title: OpenClaw 版本更新总结
created: 2026-03-08
updated: 2026-03-08
type: concept
tags: [openclaw, version, changelog, update, release]
sources: [raw/articles/OpenClaw版本更新总结.md]
confidence: high
---

# OpenClaw 版本更新总结

> 版本：2026.3.7 → 2026.3.8，发布日期：2026-03-08

## 版本概览

| 属性 | 值 |
|------|-----|
| 当前最新版本 | 2026.3.7 |
| 预览版本 | 2026.3.8（开发中） |
| 升级命令 | `npm install -g openclaw@latest` 或 `openclaw upgrade` |

---

## 2026.3.7 主要更新（2026-03-08）

### 核心架构增强

#### Context Engine 插件接口 ⭐⭐⭐⭐⭐

**新增插件槽位：** `ContextEngine` 完整生命周期钩子

- `bootstrap` - 启动时初始化
- `ingest` - 数据摄入
- `assemble` - 上下文组装
- `compact` - 上下文压缩
- `afterTurn` - 回合后处理
- `prepareSubagentSpawn` - 子智能体生成准备
- `onSubagentEnded` - 子智能体结束处理

**技术亮点：**
- 基于槽位的注册表，配置驱动解析
- `LegacyContextEngine` 包装器保留现有压缩行为
- 子智能体运行时通过 `AsyncLocalStorage` 作用域隔离
- 新增 `sessions.get` Gateway 方法

**应用价值：** 插件（如 `lossless-claw`）可提供替代上下文管理策略，无需修改核心压缩逻辑

---

#### ACP 持久化通道绑定 ⭐⭐⭐⭐⭐

**新增功能：**
- Discord 通道持久化绑定存储
- Telegram 主题持久化绑定存储
- 路由解析
- CLI 和文档支持

**应用价值：** ACP 线程目标在重启后依然存活，一致的管理体验，支持多平台长期绑定

---

#### Telegram 主题绑定增强 ⭐⭐⭐⭐

**新增功能：**
- 支持 Telegram Mac Unicode 破折号选项前缀
- 支持 Telegram 主题线程绑定（`--thread here|auto`）
- 绑定主题的后续消息路由到 ACP 会话
- 可操作的 Telegram 批准按钮

---

#### 按主题智能体路由 ⭐⭐⭐⭐

**新增功能：** 论坛群组和 DM 主题支持按主题 `agentId` 覆盖，主题可路由到专用智能体

---

### 用户体验改进

#### Web UI 国际化 ⭐⭐⭐
- 新增西班牙语（`es`）支持
- 语言环境检测、懒加载
- 跨所有支持的语言环境的语言选择器标签

#### Web 搜索增强 ⭐⭐⭐⭐
- Perplexity 提供商迁移到 Search API（结构化结果）
- 新增语言/地区/时间过滤器
- 配置向导改进

#### Gateway SecretRef 支持 ⭐⭐⭐⭐
- `gateway.auth.token` 支持 SecretRef
- 带认证模式保护
- 配置优先于环境变量

---

### 开发者体验

#### Docker 多阶段构建 ⭐⭐⭐⭐
- 重构 Dockerfile 为多阶段构建
- 产生最小运行时镜像（无构建工具、源代码、Bun）
- 新增 `OPENCLAW_VARIANT=slim` 构建参数

#### Docker 扩展依赖预装 ⭐⭐⭐
- 新增环境变量：`OPENCLAW_EXTENSIONS`
- 容器构建可预装选定的捆绑扩展 npm 依赖
- 更快、更可重现的容器部署启动

#### 插件系统增强 ⭐⭐⭐⭐
- `prependSystemContext` 和 `appendSystemContext` 静态插件指导
- 系统提示空间中的提供商缓存
- 降低重复提示 token 成本

#### 压缩生命周期钩子 ⭐⭐⭐⭐
- 新增事件：`session:compact:before` 和 `session:compact:after`
- 插件压缩回调、会话/计数元数据
- 自动化可一致响应压缩运行

---

### 平台集成

#### TTS OpenAI 兼容端点 ⭐⭐⭐⭐
- `messages.tts.openai.baseUrl` 配置支持
- 配置优先于环境变量

#### Slack DM 打字反馈 ⭐⭐⭐
- `channels.slack.typingReaction` 配置

#### Discord 提及门控 ⭐⭐⭐
- `allowBots: "mentions"` 配置

#### Mattermost 模型选择器 ⭐⭐⭐⭐
- Telegram 风格的交互式提供商/模型浏览
- `/oc_model` 和 `/oc_models` 支持

#### Feishu 群组改进 ⭐⭐⭐⭐
- 群组提及检测修复
- 群组斜杠命令检测修复

---

### 模型支持

#### Google Gemini 3.1 Flash-Lite ⭐⭐⭐⭐
- 新增模型：`google/gemini-3.1-flash-lite-preview`
- 支持模型 ID 规范化、默认别名、媒体理解图像查找

---

## ⚠️ 破坏性变更

### Gateway 认证模式要求 🔴

**变更内容：** 当同时配置 `gateway.auth.token` 和 `gateway.auth.password` 时，现在需要显式 `gateway.auth.mode`

**升级前必须配置：**
```json
{
  "gateway": {
    "auth": {
      "mode": "token",
      "token": "your-token"
    }
  }
}
```

**影响：** 不设置 `mode` 将导致启动/配对/TUI 失败

---

## 版本对比

| 特性 | 2026.3.2 | 2026.3.7 | 提升 |
|------|---------|---------|------|
| Context Engine 插件 | ❌ | ✅ | 🚀 |
| ACP 持久化绑定 | ❌ | ✅ | 🚀 |
| Telegram 主题绑定 | 基础 | 完整 | ⬆️ |
| Web UI 语言 | 英文 | 英文+西班牙语 | ⬆️ |
| SecretRef 支持 | 部分 | 完整 | ⬆️ |
| Docker 镜像优化 | 标准 | 多阶段+slim | ⬆️ |
| 压缩控制 | 基础 | 高级 | ⬆️ |

---

## 重点推荐

### 对于架构师/开发者
1. **Context Engine 插件** - 可以实现无损上下文管理
2. **Docker 多阶段构建** - 更小的生产镜像
3. **插件系统增强** - 更灵活的扩展能力

### 对于运维/部署
1. **Gateway SecretRef** - 更安全的密钥管理
2. **Docker 扩展预装** - 更快的容器启动
3. **持久化绑定** - ACP 线程重启后保持

### 对于最终用户
1. **Telegram 主题绑定** - 更灵活的智能体路由
2. **Web 搜索增强** - 更精确的搜索结果
3. **TTS 兼容性** - 更多 TTS 提供商支持

---

## 升级建议

### 升级前检查清单
- [ ] 检查 `gateway.auth` 配置，确保设置了 `mode`
- [ ] 备份当前配置文件
- [ ] 记录自定义插件和技能
- [ ] 检查 Docker 部署配置（如果使用）

### 升级步骤
```bash
# 1. 备份配置
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup

# 2. 升级
npm install -g openclaw@latest

# 3. 检查版本
openclaw --version

# 4. 重启服务
openclaw restart

# 5. 验证运行状态
openclaw status
```

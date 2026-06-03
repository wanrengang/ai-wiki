# 飞书官方 CLI 工具 (lark-cli/feishu-cli)

## 概述

- **官方工具**：字节跳动飞书团队维护
- **GitHub**: https://github.com/larksuite/cli
- **定位**：让 AI Agent 和人类都能在终端操作飞书
- **协议**: MIT

## 安装

```bash
# npm 安装（推荐）
npx @larksuite/cli@latest install

# 源码安装
git clone https://github.com/larksuite/cli.git
cd cli && make install
```

## 核心能力

- **17 大业务域**、**200+ 命令**、**24 个 AI Agent Skills**
- 三层命令架构：快捷命令 → API 命令 → 通用调用

### 业务域覆盖

| 类别 | 能力 |
|------|------|
| 📅 日历 | 查看、创建日程，邀请参会人、查找会议室、回复邀请、查询忙闲 |
| 💬 即时通讯 | 发送/回复消息、群聊管理、消息搜索、下载媒体 |
| 📄 云文档 | 创建、读取、更新、搜索文档 |
| 📊 多维表格 | 数据表、字段、记录、视图、仪表盘、自动化 |
| 📈 电子表格 | 创建、读写、追加、查找、导出表格 |
| 📧 邮箱 | 浏览、搜索、阅读、发送、回复、转发邮件 |
| ✅ 任务 | 创建、查询、更新任务，子任务、提醒 |
| 📚 知识库 | 知识空间、节点、文档管理 |
| 🎯 OKR | 查询、创建、更新 OKR |
| ✍️ 审批 | 查询、同意/拒绝/转交审批 |

## AI Agent Skills

| Skill | 说明 |
|-------|------|
| `lark-calendar` | 日历日程 |
| `lark-im` | 即时通讯 |
| `lark-doc` | 云文档 |
| `lark-sheets` | 电子表格 |
| `lark-base` | 多维表格 |
| `lark-task` | 任务 |
| `lark-mail` | 邮箱 |
| `lark-wiki` | 知识库 |
| `lark-okr` | OKR |
| `lark-approval` | 审批 |
| `lark-skill-maker` | 自定义 skill |

## 使用流程

```bash
# 1. 配置应用凭证
lark-cli config init

# 2. 登录授权
lark-cli auth login --recommend

# 3. 验证
lark-cli auth status

# 4. 开始使用
lark-cli calendar +agenda
```

## 三层命令调用

1. **快捷命令**（人类/AI 友好）
   ```bash
   lark-cli calendar +agenda
   lark-cli im +messages-send --chat-id "oc_xxx" --text "Hello"
   ```

2. **API 命令**（平台同步）
   ```bash
   lark-cli calendar calendars list
   ```

3. **通用 API 调用**（全 API 覆盖）
   ```bash
   lark-cli api GET /open-apis/calendar/v4/calendars
   ```

## 输出格式

```bash
--format json      # JSON（默认）
--format pretty   # 人性化
--format table    # 表格
--format ndjson   # 换行分隔 JSON
--format csv      # CSV
```

## 与 OpenClaw 飞书 Skill 的区别

| | OpenClaw feishu skill | lark-cli |
|--|----------------------|----------|
| 定位 | OpenClaw 专用 | 通用 AI Agent 工具 |
| 覆盖 | 文档、知识库为主 | 全业务域 |
| 调用方 | OpenClaw | 任何 AI Agent |

---

_标签：#飞书 #CLI #AI工具 #办公自动化_

# 钉钉 CLI (dingtalk-workspace-cli)

**项目名**：dingtalk-workspace-cli（简称 dws）
**开源时间**：2026年3月27日
**协议**：Apache-2.0
**仓库**：https://github.com/DingTalk-Real-AI/dingtalk-workspace-cli
**语言**：Go，支持 macOS/Linux/Windows

## 核心定位

将钉钉全产品线能力转化为 AI Agent 可直接调用的标准化 CLI 接口。**国内首个**面向 AI Agent 开源全产品能力的办公软件。

## 开放能力：13个产品线，159条命令

| 产品 | 命令数 | 主要能力 |
|------|--------|---------|
| aitable | 41 | AI表格：Base/数据表/记录/字段/视图/图表/仪表盘/导入导出/附件/模板 全量CRUD |
| calendar | 14 | 日程CRUD、参与者管理、会议室预订、闲忙查询 |
| chat (im) | 23 | 群聊管理、消息收发（用户/机器人/webhook）、会话搜索 |
| contact | 6 | 用户搜索（姓名/手机）、部门查询、当前用户信息 |
| attendance | 4 | 打卡记录、排班查询、考勤摘要、考勤规则 |
| todo | 6 | 待办创建/列表/修改/完成/详情/删除 |
| oa | 9 | 审批同意/拒绝/撤销、待我审批/我发起的 |
| report | 7 | 日志创建/收发列表/模板/详情/统计 |
| ding | 2 | DING消息发送/撤回 |
| doc | 21 | 文档搜索/读写/文件文件夹操作/块级编辑/评论/上传下载 |
| drive | 6 | 钉盘文件列表/详情/下载/创建文件夹 |
| minutes | 19 | AI听记列表/详情/摘要/思维导图/热词/上传 |
| devdoc | 1 | 钉钉开放平台文档搜索 |

另有 `mail`（邮箱）、`sheet`（在线电子表格）、`wiki`（知识库）在 README 中提及但未列入 pre 环境命令清单。

## 亮点特性

- **Agent 原生**：结构化 JSON 输出、`dws schema` 自省、--dry-run 预览、--jq 字段筛选
- **内置 Skill 系统**：安装后 Claude Code/Cursor 可直接通过自然语言操作钉钉
- **13个现成 Python 脚本**：含日程安排（创建+订会议室）、批量创建待办、考勤统计等
- **Raw API**：直接调用任意钉钉 OpenAPI（api.dingtalk.com / oapi.dingtalk.com），Token 自动管理
- **智能纠错**：自动修正 camelCase/snake_case 混淆、粘连参数拆分、拼写模糊匹配
- **安全设计**：零信任架构、OAuth 设备流认证、Token PBKDF2+ AES-256-GCM 加密、域名白名单

## 即将推出

conference（视频会议）· aiapp（AI应用）· live（直播）

## 安装方式

```bash
# macOS/Linux
curl -fsSL https://raw.githubusercontent.com/DingTalk-Real-AI/dingtalk-workspace-cli/main/scripts/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/DingTalk-Real-AI/dingtalk-workspace-cli/main/scripts/install.ps1 | iex

# npm
npm install -g dingtalk-workspace-cli
```

## Agent Skills 安装

```bash
curl -fsSL https://raw.githubusercontent.com/DingTalk-Real-AI/dingtalk-workspace-cli/main/scripts/install-skills.sh | sh
```

## 认证方式

- `dws auth login`：浏览器自动授权（企业管理员授权模式）
- `dws auth login --device`：无浏览器环境（Docker/SSH/CI）
- 自建应用：`dws auth login --client-id <APP_KEY> --client-secret <APP_SECRET>`

## 对比飞书 CLI

飞书 CLI 更偏向个人/小团队工具；钉钉 CLI 明确面向企业场景（需企业管理员授权），能力更系统化（15+服务统一CLI），并且已经原生集成 Agent 生态。

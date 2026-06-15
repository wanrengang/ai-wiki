# ECC 2.0 Alpha：用 Rust 重写控制层，要当智能体界的 Kubernetes

> 来源：微信公众号「品味决定价值」| 2026-06-05

## 核心要点

ECC 2.0 是一个用 **Rust** 重写控制层的开源项目，目标定位为 **"cross-harness operator system"（跨平台智能体操作系统）**，自称要成为 **智能体界的 Kubernetes**。

## 核心数据

| 指标 | 数值 |
|------|------|
| 代码量 | 4,417 行 Rust |
| 技能数量 | 251 个 |
| 智能体数量 | 63 个 |
| 支持平台 | 7+ (Claude Code, Codex, Cursor, OpenCode, Gemini CLI, Zed, Terminal) |

## 技术架构亮点

### 1. DbWriter 线程：解决「SQLite from async」问题

ECC2 使用 `rusqlite 0.32` 进行本地状态持久化。但 rusqlite 是同步的，而 ECC2 的运行时是 tokio 异步环境。

ECC2 采用 **DbWriter 线程模式**：
- 一个专用的 OS 线程，通过 `mpsc::unbounded_channel` 接收异步上下文的写入请求
- 用 `oneshot channel` 返回确认
- 避免了 async/await 和同步 I/O 的混用

### 2. 会话状态机：强制转换规则

ECC2 的会话生命周期是严格的状态机：

```
Pending → {Running, Failed, Stopped}
Running → {Idle, Completed, Failed, Stopped}
```

任何非法的状态转换都会在编译期或运行期被捕获。

### 3. 代码质量指标

- **unwrap() 调用**：仅 3 个（2 个在测试中，1 个在 config default()）
- **unsafe 块**：0 个
- **TODO/FIXME 注释**：0 个
- **测试函数**：29 个，覆盖 10 个源模块

### 4. 4 轴风险评分系统

observability 模块（409 行代码）实现了 4 轴风险分析：

| 维度 | 示例 | 分数 |
|------|------|------|
| 基础工具风险 | rm -rf | 0.9, ls = 0.1 |
| 文件敏感度 | .env | 0.8, README.md = 0.1 |
| 爆炸半径 | git push --force | 0.95 |
| 不可逆性 | 数据删除 | 1.0 |

合成 **0.0-1.0 composite score**，给出建议动作：
- **Allow**：低风险操作，直接执行
- **Review**：中等风险，需要人工审查
- **RequireConfirmation**：高风险，需要明确确认
- **Block**：极高风险，直接阻止

## 跨平台复用架构

ECC 的核心理念：**"Skills are the most portable unit"**

一个设计良好的 ECC 技能应该：
- 使用 YAML frontmatter（name, description, origin）
- 描述何时使用该技能
- 声明所需工具或连接器，但不嵌入密钥
- 示例使用相对路径或通用格式
- 避免特定平台的命令假设

### 跨平台支持矩阵

| 平台 | 支持方式 |
|------|----------|
| Claude Code | ✅ 插件 + 原生钩子 |
| Codex CLI | ✅ AGENTS.md + MCP |
| Cursor | ✅ 翻译规则 + 钩子 |
| OpenCode | ✅ 插件 + 事件系统 |
| Gemini CLI | ✅ 指令驱动 |
| Zed | ✅ 项目级规划 |
| Terminal-only | ✅ CLI 工作流 |

### 选择性安装架构

```bash
./install.sh --profile minimal --target claude --with capability:machine-learning
```

## Hermes Agent

Hermes 不是 ECC 的运行时，是一个**操作员 Shell**，可以消费 ECC 的资产：
- 导入选定的 ECC 技能到 Hermes 技能目录
- 使用 ECC 的 MCP 协议进行工具访问
- 通过可复用的 ECC 模式路由聊天、CLI、Cron 和交接工作流
- 将重复的本地操作员工作流提炼为 sanitized ECC 技能

**边界清晰**：ECC 发布的是可复用模式，不发布 OAuth 令牌、API 密钥、个人工作空间记忆或私有数据集。

## 当前 Gap（Alpha 阶段）

- **Comms 模块**：有 send() 但没有 receive/poll()，消息表存在 SQLite 但无读取逻辑
- **新建会话对话框**：TUI 中 new_session() 只打印日志，不执行任何操作
- **单智能体支持**：agent_program() 只支持 "claude"
- **配置仅限文件**：Config::load() 只读取 ~/.claude/ecc2.toml，缺少环境变量覆盖

## 路线图

- [ ] 完成 Comms 模块的 receive/poll 逻辑
- [ ] 实现 TUI 的新建会话对话框
- [ ] 扩展多智能体支持（codex, opencode, custom）
- [ ] 添加环境变量和 CLI 配置支持
- [ ] 实现 Daemon 健康报告（PID 文件、日志轮转、优雅关闭）

## 项目信息

- **许可**：MIT
- **仓库**：github.com/affaan-m/ECC
- **定位**：可复用技能仓库 + 操作界面（Hermes Agent）
- **架构**：Rust 控制平面，跨平台智能体操作系统

---

*标签：#AI-Agent #Rust #Kubernetes #ECC #Claude-Code #跨平台*

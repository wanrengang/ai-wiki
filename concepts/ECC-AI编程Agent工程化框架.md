# ECC：AI 编程 Agent 工程化框架

> 来源：[公众号 MOSS进化论](https://mp.weixin.qq.com/s/ylW8ayoFq1SC5t19O7ZNvw) | 2026-05-25

## 项目概览

**ECC**（Everything Claude Code）是一个面向 AI 编程工具的 Agent 工程化框架，定位是"把 AI 编程工具当成基础设施来用的完整系统"。

- **GitHub**：https://github.com/affaan-m/ECC
- **Star**：182K+
- **作者**：Anthropic 黑客松获奖者
- **开发周期**：10 个月以上重度使用打磨
- **License**：MIT

## 核心定位

> 不只是配置文件，而是一整套完整系统：技能体系、本能行为、记忆优化、持续学习、安全扫描，以及研究优先的开发模式。

## 核心能力

### 60 个专用 Agent

覆盖开发全流程：

| Agent | 职责 |
|-------|------|
| planner | 功能实现规划 |
| architect | 系统架构设计决策 |
| tdd-guide | 测试驱动开发 |
| code-reviewer | 代码质量与安全审查 |
| security-reviewer | 漏洞分析 |
| build-error-resolver | 构建错误修复 |
| e2e-runner | Playwright 端到端测试 |
| refactor-cleaner | 无效代码清理 |
| doc-updater | 文档同步更新 |
| docs-lookup | 文档 / API 查阅 |
| chief-of-staff | 沟通梳理与文稿起草 |
| loop-operator | 自主循环执行 |
| harness-optimizer | 执行框架配置调优 |
| cpp/go/python/rust-reviewer | 各语言代码审查 |
| java/kotlin-build-resolver | 构建错误修复 |
| pytorch-build-resolver | PyTorch/CUDA 训练错误修复 |

### 232 个按需加载的 Skill

覆盖：写代码、写文章、做投资材料、跑营销方案等

### AgentShield 安全扫描

- 内置安全扫描机制，审计 `CLAUDE.md`、`MCP 配置`、`hook`、`skill` 里的安全风险
- 三个 Opus 4.6 模型跑红蓝对抗
  - 攻击方：找漏洞
  - 防守方：评估
  - 审计方：给出最终风险评级

### 连续学习层

- 自动从每次会话中提取有效模式
- 沉淀成可复用的 Skill
- 用得越多，系统越聪明

### 语言生态

支持 12 种语言：TypeScript、Python、Go、Rust、Java、Swift 等

## 架构

```
everything-claude-code/
├── .claude-plugin/       # 插件与应用商店清单
├── agents/              # 36+ 专用子智能体
│   ├── planner.md        # 功能实现规划
│   ├── architect.md      # 系统架构设计
│   ├── tdd-guide.md      # 测试驱动开发
│   ├── code-reviewer.md  # 代码审查
│   ├── security-reviewer.md
│   ├── build-error-resolver.md
│   └── ...
├── skills/              # 工作流定义与领域知识库
│   └── coding-standards/ # 各语言最佳实践
├── rules/               # 规则配置
└── hooks/               # 生命周期钩子
```

## 安装方式

### 方式一：插件安装（推荐）

```bash
# 添加市场
/plugin marketplace add https://github.com/affaan-m/everything-claude-code

# 安装插件
/plugin install ecc@ecc
```

### 方式二：手动安装

```bash
git clone https://github.com/affaan-m/everything-claude-code.git
cd everything-claude-code
npm install
mkdir -p ~/.claude/rules
cp -R rules/common ~/.claude/rules/
cp -R rules/typescript ~/.claude/rules/
```

## 相关工具

| 工具 | 说明 |
|------|------|
| `ecc-universal` (npm) | 通用包 |
| `ecc-agentshield` (npm) | 安全扫描包 |
| `ecc-tools` (GitHub App) | GitHub 市场工具 |
| `ccg-workflow` (npx) | multi-* 命令运行时 |

## 关键概念

| 概念 | 说明 |
|------|------|
| Token 优化 | 模型选择、系统提示精简、后台进程优化 |
| Memory Persistence | 自动跨会话保存/加载上下文 |
| Continuous Learning | 从会话中自动提取模式到可重用 Skill |
| Verification Loops | 检查点 vs 持续评估、pass@k 指标 |
| 并行化 | Git worktrees、级联方法 |
| 子代理编排 | 上下文问题、迭代检索模式 |

## 支持的 AI 编程工具

Claude Code、Codex、Cursor、OpenCode、Gemini、Zed、GitHub Copilot 等

## 总结

ECC 的核心价值在于：不是简单提供配置，而是提供一整套经过 10 个月真实产品开发验证的 Agent 工程化实践。它的理念值得借鉴：

1. **系统化思维**：把 AI 编程工具当成基础设施来设计
2. **持续学习**：让每次会话都产生可复用资产
3. **安全前置**：红蓝对抗 + 风险评级
4. **多语言支持**：12 种语言的代码审查和构建排错

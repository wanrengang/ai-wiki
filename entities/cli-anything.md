---
title: CLI-Anything
created: 2026-03-24
updated: 2026-03-24
type: entity
tags: [cli, agent, automation, gui-to-cli, hku, open-source]
sources: [raw/articles/CLI-Anything深度研究报告-2026-03-24.md]
confidence: high
---

# CLI-Anything

> HKUDS 实验室开源项目，将任意 GUI 软件自动转换为 AI Agent 可用的 CLI 工具。

## 概述

**核心理念：** "Today's Software Serves Humans. Tomorrow's Users will be Agents."

CLI-Anything 通过七阶段自动化方法论，将任意 GUI 应用转换为 Agent 可调用的 CLI 工具。不同于 RPA 模拟鼠标点击，它直接调用真实软件后端 API。

## 关键事实

| 属性 | 值 |
|------|-----|
| 开发团队 | 香港大学数据智能实验室（HKUDS） |
| GitHub Stars | 21k+ |
| 开源协议 | MIT |
| 已验证软件 | 16 款 |
| 测试用例 | 1,858 个 |
| 测试通过率 | 100% |

## 七阶段方法论

1. **Codebase Analysis** — 分析目标软件架构，映射 GUI 操作到 API
2. **CLI Architecture Design** — 设计 CLI 结构（REPL/子命令）
3. **Implementation** — 构建可工作的 CLI（Python + Click）
4. **Test Planning** — 先规划测试清单
5. **Test Implementation** — 四层测试金字塔（单元→E2E→后端→子进程）
6. **Test Documentation** — 记录测试结果
7. **SKILL.md Generation** — 自动生成 AI 可发现的技能文件

## 核心技术创新

### 目录结构

```
<software>/
└── agent-harness/
    ├── <SOFTWARE>.md       # 软件特定 SOP
    ├── setup.py            # PyPI 配置
    └── cli_anything/<software>/
        ├── README.md       # 安装指南
        ├── <software>_cli.py  # 主 CLI 入口
        ├── core/           # 核心模块
        ├── skills/         # AI 可发现技能
        │   └── SKILL.md
        ├── utils/          # 真实软件包装器
        └── tests/          # 测试套件
```

### 关键技术

- **真实软件调用**：调用原生软件后端，不重新实现
- **REPL 皮肤**：统一交互界面
- **会话文件锁**：原子化 JSON 写入
- **命名空间包**：PEP 420，无 `__init__.py`
- **MCP 协议支持**

## 与 Claude Dispatch 的关系

```
手机 App (Dispatch) → Claude Agent → Claude Code CLI
                                        ↓
                               CLI-Anything 生成真实软件 CLI
                                        ↓
                               GIMP / Blender / LibreOffice...
```

CLI-Anything 定位于**执行层**，与 Claude Dispatch 的**控制层**形成互补。

## 价值主张

| 特性 | 传统 GUI | CLI-Anything |
|------|----------|--------------|
| AI 可用性 | ❌ | ✅ 结构化、可组合、自描述 |
| 维护成本 | 高（模拟点击脆弱） | 低（调用真实 API） |
| 可靠性 | 低 | 高（生产级验证） |
| 跨软件一致性 | ❌ | ✅ 统一接口 |

## 相关链接

- GitHub: https://github.com/HKUDS/CLI-Anything

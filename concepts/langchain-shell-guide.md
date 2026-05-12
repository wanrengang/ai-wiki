---
title: LangChain Shell 工具使用指南
created: 2026-03-08
updated: 2026-03-08
type: concept
tags: [langchain, shell, tool, bash, terminal]
sources: [raw/articles/LangChain Shell 工具使用指南.md]
confidence: high
---

# LangChain Shell 工具使用指南

> Shell 工具允许 LLM 通过 LangChain 执行系统 shell 命令，适用于文件操作、系统管理等场景。

## 基本使用（Python）

### 安装和导入

```python
from langchain_community.tools import ShellTool

shell_tool = ShellTool()
```

### 执行命令

```python
# 执行单个命令
result = shell_tool.run({"commands": ["echo 'Hello World!'"]})
print(result)

# 执行多个命令
result = shell_tool.run({
    "commands": [
        "echo 'Hello World!'",
        "ls -la",
        "pwd"
    ]
})
```

---

## 与 Agent 集成（Python）

### 方式一：直接使用 ShellTool

```python
from langchain.agents import create_agent
from langchain_anthropic import ChatAnthropic
from langchain_community.tools import ShellTool

shell_tool = ShellTool()

agent = create_agent(
    model=ChatAnthropic(model="claude-sonnet-4-5-20250929"),
    tools=[shell_tool],
)

result = agent.invoke({
    "messages": [{
        "role": "user", 
        "content": "列出当前目录的所有 Python 文件"
    }]
})
```

### 方式二：自定义 Bash 工具

```python
import subprocess
from langchain.agents import create_agent
from langchain_anthropic import ChatAnthropic
from langchain.tools import tool

@tool
def bash(command: str, restart: bool = False):
    """执行 bash 命令
    
    Args:
        command: 要执行的 shell 命令
        restart: 是否重启会话
    """
    if restart:
        return "Bash session restarted"
    
    try:
        result = subprocess.run(
            command,
            shell=True,
            capture_output=True,
            text=True,
            timeout=30,
        )
        return result.stdout + result.stderr
    except Exception as e:
        return f"Error: {e}"

agent = create_agent(
    model=ChatAnthropic(model="claude-sonnet-4-5-20250929"),
    tools=[bash],
)
```

---

## TypeScript/JavaScript 版本

```typescript
import { ChatOpenAI, tools } from "@langchain/openai";
import { exec } from "node:child_process/promises";

const model = new ChatOpenAI({ model: "gpt-4o" });

const shellTool = tools.shell({
  execute: async (action) => {
    const outputs = await Promise.all(
      action.commands.map(async (cmd) => {
        try {
          const { stdout, stderr } = await exec(cmd, {
            timeout: action.timeout_ms ?? 30000,
          });
          return {
            stdout,
            stderr,
            outcome: { type: "exit" as const, exit_code: 0 },
          };
        } catch (error: any) {
          const timedOut = error.killed && error.signal === "SIGTERM";
          return {
            stdout: error.stdout ?? "",
            stderr: error.stderr ?? String(error),
            outcome: timedOut
              ? { type: "timeout" as const }
              : { type: "exit" as const, exit_code: error.code ?? 1 },
          };
        }
      })
    );
    return {
      output: outputs,
      maxOutputLength: action.max_output_length,
    };
  },
});

const llmWithShell = model.bindTools([shellTool]);

const response = await llmWithShell.invoke(
  "查找 ~/Documents 中最大的 PDF 文件"
);
```

---

## 安全注意事项

### ⚠️ 重要安全风险

1. **命令注入攻击** - 恶意输入可能执行危险命令
2. **权限过大** - 可能访问敏感文件或修改系统
3. **资源消耗** - 无限制命令可能导致系统过载

### 🔒 安全最佳实践

```python
import subprocess
from langchain.tools import tool

# 白名单允许的命令
ALLOWED_COMMANDS = {
    'ls', 'pwd', 'echo', 'cat', 'head', 'tail', 'grep'
}

@tool
def safe_bash(command: str):
    """安全的 bash 命令执行（带白名单限制）"""
    
    cmd_name = command.split()[0]
    if cmd_name not in ALLOWED_COMMANDS:
        return f"Error: 命令 '{cmd_name}' 不在允许列表中"
    
    dangerous_patterns = ['rm -rf', 'sudo', 'chmod', 'chown', '> /']
    if any(pattern in command for pattern in dangerous_patterns):
        return "Error: 检测到危险命令"
    
    try:
        result = subprocess.run(
            command,
            shell=True,
            capture_output=True,
            text=True,
            timeout=10,
            cwd='/safe/working/directory'
        )
        return result.stdout + result.stderr
    except subprocess.TimeoutExpired:
        return "Error: 命令执行超时"
    except Exception as e:
        return f"Error: {str(e)}"
```

---

## 实际应用场景

| 场景 | 示例命令 |
|------|---------|
| 文件管理 | `ls`, `cp`, `mv`, `find` |
| 日志分析 | `tail -f app.log`, `grep ERROR logs/` |
| 系统监控 | `ps aux`, `df -h`, `free -m` |
| 数据处理 | `awk`, `sed`, `sort` |
| Git 操作 | `git status`, `git log --oneline` |

---

## 关键配置参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `timeout` | 命令超时时间（秒） | 30 |
| `timeout_ms` | 超时时间（毫秒） | 30000 |
| `max_output_length` | 最大输出长度 | 限制输出大小 |
| `restart` | 重置会话状态 | false |

---

## 推荐做法

**✅ 应该做的：**
- 使用命令白名单
- 设置超时限制
- 限制工作目录
- 捕获并处理异常
- 记录命令执行日志

**❌ 不应该做的：**
- 允许执行任意命令
- 在生产环境使用 `rm -rf`
- 忽略超时设置
- 允许 `sudo` 权限命令
- 不做输入验证

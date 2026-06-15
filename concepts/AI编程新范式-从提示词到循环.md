# AI编程新范式：从提示词工程到循环机制

- **作者:** 褚杏娟（InfoQ）
- **日期:** 2026年6月9日
- **原始链接:** https://mp.weixin.qq.com/s/JKrDuM4jaspm2wL-vJ1qsA
- **标签:** #AI编程 #Agent #ClaudeCode #提示词工程

---

## 核心观点

### AI编程的三个阶段演进

**第一阶段（1年前）：** 在 IDE 里配合自动补全写代码

**第二阶段（去年11月）：** 卸载 IDE，同时跑 5-10 个 Claude，让 AI 互相协作写代码

**第三阶段（现在）：** 不再提示单个 Claude，而是写**循环（loops）**机制，让循环去提示 Agent 并判断下一步做什么

### 核心人物观点

**Boris Cherny**（Anthropic 工程师、Claude Code 创建者）：
> "现在，我觉得又到了下一个层级：我不再提示 Claude 了，我有一堆循环在运行，它们才是在提示 Claude 并判断接下来该做什么。我的工作变成了写循环。我认为，这是接下来几个月，甚至今年剩余时间里我们会看到的下一次转变。"

**Peter Steinberger**（OpenAI 任职、"龙虾之父"）：
> "你不该再给编程 Agent 写提示词了。你应该设计一套循环机制，让这些循环去提示你的 Agent。"

该帖子获得 **150万浏览量**，引发大量开发者讨论。

---

## 新旧范式对比

| 旧范式 | 新范式 |
|--------|--------|
| 人类写提示词给 AI | 人类写循环机制 |
| 单个 AI Agent | 多 Agent 协作循环 |
| 人判断下一步 | 循环判断下一步 |

---

## 意义

AI 编程的范式转移：**从"提示 AI 做事"到"设计 AI 做事的管理机制"**。

程序员的核心技能从"写代码"转向"设计 Agent 协作流程"。

---

## 参考

- 原文：https://mp.weixin.qq.com/s/JKrDuM4jaspm2wL-vJ1qsA
- Boris Cherny：Anthropic 工程师、Claude Code 创建者
- Peter Steinberger：OpenAI 工程师、"龙虾之父"

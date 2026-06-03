# 别再伺候 AI 了：5 个方法狠狠压榨 Codex

> 来源：X @369Serena，2026年5月29日
> 原始链接：https://x.com/369Serena/status/2060330862223515816
> 数据：11.6万查看，157转帖，557喜欢，1103书签

---

## 核心问题

你已经用 Codex 了，但每次还是要自己复制资料、补充背景、反复解释需求——最后感觉不是 AI 在帮你省时间，而是你在陪 AI 加班。

**Codex 效率提升的关键，不是"多聊几句"，而是慢慢减少那些你每次都要重复说的话。**

---

## 5 个方法

### 1. 插件：先让 Codex 长出手脚

没有插件的 Codex，只能回答你。
接上插件以后，它才开始能做事。

**核心思路：** 不要只让 AI"想"，要让它能"动手"。

Gmail、Slack、Figma、Notion、Google Drive、GitHub——这些插件接上以后，Codex 就不再只是一个聊天 AI，而是能进工具里帮你干活的助手。

**小白配置步骤：**
1. 打开 Codex app
2. 进左侧或设置里的 Plugins
3. 搜索 Gmail / Google Drive / GitHub 这类插件
4. 点安装或连接，按提示登录授权
5. 回到对话里，明确告诉 Codex 用哪个插件

---

### 2. Goal Mode：不要给任务，给目标

很多人用 Codex，是这样用的：
> "帮我看一下这个文件。" → "再改一下。" → "这里不对。" → "继续。"

这其实还是你在牵着它走。你累，它也容易跑偏。

**Goal Mode 更像是你给它一个终点，然后让它自己拆步骤、执行、检查、修正。**

2026年5月21日的 release notes 里提到，Goal mode 已经在 Codex app、IDE extension 和 CLI 里可用。

---

### 3. 自动化：让 Codex 每天自己上班

真正省时间的东西，不是"我叫它，它很快"。
是"我不叫它，它也会按时出现"。

**好的自动化任务要满足：** 具体、可重复、容易复查。

先从每天、每周重复的小流程开始。比如：
- 每天早上检查项目错误日志，整理新增问题
- 每天早上整理 AI / Codex / MCP 的重要更新

**小白配置步骤：**
1. 先在普通对话里手动跑一次
2. 确认输出格式满意
3. 对 Codex 说：把刚才这个任务创建成自动化
4. 去左侧 Automations 检查任务
5. 确认频率、提示词、状态
6. 先跑 2-3 次，再改 prompt

---

### 4. MCP + Obsidian：把 Codex 接进你的知识库

互联网给 Codex 外部信息。
Obsidian 给 Codex 你的个人上下文。

很多人说 AI 不懂自己——你的笔记、经验、素材、项目复盘都在 Obsidian 里，Codex 没看见，当然只能泛泛地写。

MCP 解决的就是这个问题。通过 MCP，可以让 Codex 连接 Obsidian、文档库、内部工具。

**配置示例：**
```ini
[mcp_servers.obsidian]
url = "https://127.0.0.1:27124/mcp/"
bearer_token_env_var = "OBSIDIAN_API_KEY"
```

---

### 5. Skills：把常用流程固化

**如果一个流程你重复做了 3 次，就应该把它变成 Skill。**

Skill 就是你写给 Codex 的 SOP。

- 插件解决"去哪里拿资料"
- Skills 解决"按什么流程做"

**更高级的做法：父子 Skill**
- 父 Skill 负责判断任务类型：这次是写长文、做表格、查资料，还是生成报告
- 子 Skill 负责具体执行：资料提炼、开头钩子、案例改写、格式检查

这样 Codex 不只是会做事，而是会按你的方式做事。

---

## 总结

先从一个最小场景开始：
- 先装一个 Gmail 插件，让它帮你整理邮件
- 或者先把每天都要看的信息做成自动化
- 再或者先把你最常写的一类文章做成 Skill

不用一下子把 5 个方法全部配齐。你只要先跑通一个，就会明显感觉到：**Codex 不再只是回答你，而是开始替你分担一小段流程。**

等插件、Goal Mode、自动化、MCP、Skills 慢慢连起来，它就会越来越像一个熟悉你工作方式的助手。

---

## 参考资料

- OpenAI Academy: Plugins and skills
- OpenAI Academy: Automations
- OpenAI Academy: What is Codex
- OpenAI Help Center: 2026-05-21 Codex updates / Goal mode release notes

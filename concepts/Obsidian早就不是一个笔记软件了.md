# Obsidian早就不是一个笔记软件了

> 来源：微信公众号  
> 日期：2026年5月13日 21:51

## 核心观点

在 Claude Code 引领的 CLI 时代，Obsidian 已从 AI 笔记软件演变为**个人知识库和自动化内容创作平台的最佳选项**。Andrej Karpathy 提出的 LLM Wiki 概念，Claude Code + Obsidian 就是最佳实践载体。

## 三层架构

### 上游：信息采集
- **Obsidian Web Clipper**（官方 Chrome 扩展）
- 功能：把网页存成本地 Markdown，给重要段落打高亮

### 中间层：内容操作和处理
Obsidian 在 Claude Code 中提供 5 个官方 Skills（Plugin），让 Agent 操作 vault：

| Skill | 用途 |
|-------|------|
| defuddle | 把网页正文剥干净再喂模型，比 WebFetch 省 token |
| obsidian-cli | 命令行操作，创建笔记、修改 frontmatter、管理 task |
| obsidian-markdown | AI 正确生成 wikilinks、callouts、properties 等 Obsidian 专用语法 |
| obsidian-bases | 操作 .base 文件给笔记建数据库视图 |
| json-canvas | 编辑 .canvas，做选题规划画布或可视化内容矩阵 |

### 下游：探索
- Obsidian 官方发布《The Future of Obsidian plugins》
- 正式上线 **Obsidian Community**：收录 3498 个插件、508 个主题
- 可按 AI、编辑、自动化、文件管理等 tag 分类筛选

## 结论

内容采集、操控、探索这三块组合，Obsidian 才真正从 markdown 笔记软件变成 **LLM Knowledge Base**。作者强烈推荐每个人把 Obsidian 深度用起来。

---

标签：#Obsidian #LLMWiki #知识管理 #ClaudeCode #AI工具

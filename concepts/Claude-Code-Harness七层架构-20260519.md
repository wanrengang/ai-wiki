# Claude Code 真正的上限，藏在 Harness 里

> 来源：木易的AI频道，机器之心转载，2026-05-19
> 原文：Anthropic 官方博客 "How Claude Code works in large codebases"

## 核心结论

**决定 Claude Code 表现上限的，不是模型，是你围绕模型搭的那套脚手架。**

Anthropic 把这套脚手架叫「Harness」。

---

## Claude Code 七层 Harness 架构

| 层级 | 名称 | 作用 |
|------|------|------|
| 1 | CLAUDE.md | 地图，按目录叠加，60-120行最佳 |
| 2 | Hooks | 强制执行，Stop Hook 可自动更新经验 |
| 3 | Skills | 按需加载专家知识，可绑定路径 |
| 4 | Plugins | Skills/Hooks/MCP 打包分发 |
| 5 | LSP | 符号级精度导航代码 |
| 6 | MCP | 连接内部工具、数据源、API |
| 7 | 子代理 | 独立 Claude 实例，探索与修改分离 |

---

## 三条实操建议

1. **在子目录启动**，不在根目录 → 上下文聚焦
2. **测试/lint 按子目录配置** → 避免全仓库扫描
3. **定期维护 CLAUDE.md** → 每3-6个月审查，模型更新后检查

---

## 商业数据

- Anthropic 商业客户占比：**34.4%** > OpenAI 32.3%
- Claude Code 贡献 GitHub 提交：**4%**（上月 2%）
- 年化收入：**25 亿美元**（6个月破10亿，3个月破25亿）

---

## 标签

#Claude-Code #Harness #AI编程 #Agent #企业部署

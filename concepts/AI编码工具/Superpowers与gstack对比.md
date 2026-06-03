# Superpowers 和 gstack 到底谁更适合给 AI 编码加护栏

> 原文：万里数字笔记，2026年5月25日

## 核心结论

两者不是竞争对手，而是同一条流水线上的两道工序：
- **Superpowers**：管"怎么写代码"（过程管理）
- **gstack**：管"写完怎么验收和交付"（验收团队）

## 基本面对比

| 工具 | Star数 | 创建时间 | 作者 | 语言 |
|------|--------|----------|------|------|
| Superpowers | 20.4万 | 2025年10月 | Jesse Vincent (Perl社区传奇) | Shell |
| gstack | 10.1万 | 2026年3月 | Garry Tan (Y Combinator CEO) | TypeScript |

## Superpowers 的职责：开发纪律

7步流程锁定编码过程：
1. 头脑风暴（硬门槛，未完成设计评审不能写代码）
2. 隔离分支
3. 任务拆解
4. 子代理执行
5. TDD 循环（AI先写实现再补测试？直接删掉重来）
6. 代码审查（独立AI实例执行，避免"自己审自己"）
7. 分支收尾

**特点**：只管编码过程，不管浏览器、不管部署。代码审查通过后工作就结束。

## gstack 的职责：验收与交付

6个核心命令：
- `/browse`：启动真实浏览器验证页面渲染（约100ms/条命令）
- `/qa`：自动跑测试、发现bug自动修复、提交原子commit
- `/ship`：同步主分支、跑测试、审计覆盖率，测试不过不推
- `/land-and-deploy`：合并PR后等CI、触发部署、在生产环境验证
- `/canary`：上线后持续监控控制台错误、性能指标、接口响应
- `/careful`：拦截危险命令（`rm -rf`、`DROP TABLE`等）

**特点**：`/codex`命令可调用OpenAI模型做交叉评审（Claude写+GPT审）。

## 选型建议

| 场景 | 建议 |
|------|------|
| 个人开发、小项目、需求不稳定 | 先上 Superpowers |
| 大量使用 Claude Code、需要浏览器验证 | 补 gstack |
| 团队协作、生产环境 | 两者组合使用 |
| 简单改 typo、小脚本 | 两者都不必用 |

## 成本与争议

**Superpowers 问题**：
- Token 消耗大（完整7步可能消耗几千Token）
- 在项目目录创建大量工作文件（脑暴记录、审查报告）
- 核心围绕 Claude Code 设计，其他工具体验有折扣

**gstack 问题**：
- README用LOC（代码行数）衡量生产力，被社区吐槽
- 默认收集遥测数据（可关闭）
- 依赖 bun 运行时，macOS 有代码签名问题，安装门槛较高

## 安装方式

**Superpowers（三行命令）**：
```bash
claude plugin marketplace add obra/superpowers-marketplace
claude plugin install superpowers@superpowers-marketplace
```

**gstack（clone仓库）**：
```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup
```

## 关键洞察

两个工具代表了 AI 辅助开发走向成熟的标志：
- 单靠"AI 聪明"不够，还需要流程（Superpowers）和验收（gstack）来兜底
- Superpowers 兜的是编码过程的底
- gstack 兜的是交付质量的底
- 两个底都兜住，AI 编码才能真正放心用在生产环境

---

标签：#AI编码 #Claude Code #Superpowers #gstack #开发者工具

# GStack: YC CEO 的开源项目与 AI Coding 争议

**来源**: 2026-05-09 微信文章  
**链接**: https://mp.weixin.qq.com/s/tPCRmEWFler5C2vInggLeg

---

## 核心数据

- GitHub Stars: 9万+
- 声称效率提升: 400x logical LOC（从100倍修正）
- 角色数量: 23个专业角色
- 工具模块: 8个
- 协议: MIT 开源

## GStack 是什么

"AI 软件工厂"系统，将 Claude Code、Codex 等模型组织成虚拟工程团队。核心角色包括：CEO、工程经理、设计师、Reviewer、QA（真实浏览器测试）、安全官、Release Engineer。

## 核心理念：Thin Harness, Fat Skills

> "Agent 的底层执行框架（Harness）应该尽量轻量化，而真正重要的，是构建大量高质量的'技能层（Skills）'。"

Skills 本质上是用 Markdown 编写的结构化工作流。Garry Tan 甚至认为：
> "Markdown 本身已经开始变成一种新的'编程语言'——它不再只是文档，而是在驱动整个 Agent 系统。"

## 争议

- 社区评价：核心理念合理但非新鲜事物；真正有用的部分：/qa 和 /browse 使用真实浏览器测试的技能
- Mo Bitar 批评："Garry 开源的是一堆提示词文件夹"
- 关键问题：AI 本质上是"自信放大器"而非能力放大器
- "每天1-2万行代码"被Reddit视为毫无意义的虚荣指标

## 代码生产力对比

| 版本 | 成本 | 团队 | 时间 |
|------|------|------|------|
| 第一次（历史） | 400万美元 | 6-7人 | 1.5年 |
| 第二次（2013年） | 10万美元 | 2人 | 3个月 |
| 第三次（2025年） | **200美元**（Claude Code Max） | 1人 | **5天** |

## 关键观点

1. **"Boil the Ocean"哲学**：AI 边际成本极低，直接砸 token 换取更完整、更真实的结果
2. **80%-90% 测试覆盖率是底线**：AI 写代码最大风险不是"不会写"而是"80%能跑但用户一碰就崩"
3. **Mini AGI 已经出现**：Browse 技能本质上是长期运行的 HTTP daemon + 70个CLI命令
4. **AI 是意志的放大器**："AI 不会替你产生'关心'，那个 agency 必须来自你自己"
5. **个人 AI vs 公司控制 AI**：个人电脑革命最大的礼物是"个人拥有计算能力"，现在进入 Personal AI 时刻
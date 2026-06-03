# AnySearch：专为AI Agent打造的搜索基础设施

> 来源：[新智元/ASI启示录](https://mp.weixin.qq.com/s/-pWdGGf4U4D5wCDnw9lawg)  |  2026-05-18

## 核心定位

AnySearch = AI 时代的"搜索基础设施"，专为 AI Agent 打造统一高质量搜索入口。一个 API Key，访问 AI 所需的优质数据源，替代过去需要集成几十个不同 API 的繁琐工作。

官网：anysearch.com | 上线：2026年5月11日

---

## 解决什么问题

传统 Agent 开发中，搜索模块的开发成本可能比 Agent 本身还高：
- 分别注册多个平台账号（企查查、Finnhub、PubMed、VirusTotal 等）
- 学习几十套不同的 API 文档
- 管理几十个 API Key 的限流和余额
- 自己写路由逻辑决定什么查询发给哪个数据源

AnySearch 把这一切打包，一个 API Key 全搞定。

---

## 四大核心能力

| 场景 | 能力 | 案例 |
|------|------|------|
| 场景1 | **聚合广度** | 查英伟达，同时返回 Reddit 讨论、GitHub 代码、股票走势，多源并行 |
| 场景2 | **专业深度** | 查 Rust 异步连接池，直接从 bb8/pgcat/deadpool 三个生产仓库拉源码，逐行级 |
| 场景3 | **信息甄别** | 查 Anthropic 尽调，数据打架时标出置信度，$1.2万亿标记"离群值不采信"，$9000亿标"条款阶段未官宣"，查不到的直接说"不编" |
| 场景4 | **诚实边界** | 查 IP 威胁情报，把 whois/BGP/DNS 三层事实摆出，明确说"没有信誉数据，不编造定性"，还纠正了搜索结果的误标 |

核心结论：**聚合广度 + 专业深度 + 对已知的甄别 + 对未知的诚实**

---

## 覆盖的信息范围

据官方说法：查企业股权穿透、A股分钟级行情、法院裁决书全文、GitHub 生产级代码、VirusTotal 恶意检测——一个 API 即可搞定。

覆盖的信息据称占互联网总量 80% 以上，却是 Google 和 Exa 架构上触及不到的盲区（因为这些数据在专业数据库深处，不是爬虫能爬到的）。

---

## 数据源支持

支持 Skill、MCP、API 三种接入方式，官方称支持 23 个垂直领域（金融、法律、学术、网络安全、能源、企业工商等）。

---

## 现状评估

**✅ 确认是真的：**
- 官网 anysearch.com 真实存在，有完整 API 文档
- GitHub：github.com/anysearch-ai（2个仓库，合计约593 stars）
- PRNewswire 5月11日发布新闻稿
- skills.sh 热榜 TOP1

**⚠️ 需要注意：**
- "吊打 Perplexity/Brave"的基准测试是**内部基准**，无第三方验证
- "23个垂直领域"具体接入哪些提供商**未公开**，无法核实
- "80%互联网"是无法证伪的营销数字
- 每天免费 1000 次 API 调用，生产环境额度很小
- GitHub 代码库很薄，是非常早期的创业项目
- 缺少真实生产环境的长期使用报告

**建议**：去 anysearch.com/console/api-keys 注册拿免费 key，亲自测试后再做判断。

---

## 相关链接

- 官网：https://www.anysearch.com
- GitHub：https://github.com/anysearch-ai
  - anysearch-skill: 303 stars
  - anysearch-mcp-server: 290 stars

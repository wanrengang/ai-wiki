---
source_url: https://mp.weixin.qq.com/s/NNCfsnhAHuqtRQrMVr0gog
ingested: 2026-05-15
sha256: 6e422a1bb9e44cbc61123dfa1b7ec6ea3478deaa617816e077baa4c740ea39cb
---
# Kimi AI基建深度解析：人手一个数据库的技术突破

量子位

## 核心问题：为什么"人手一个数据库"是奢侈配置？

Kimi K2.6 的 Agent 模式能在 5分钟内 生成完整可访问的 Web 应用，包含前端、后端、独立数据库和用户账号体系。

关键挑战：100万用户同时使用 → 后台要瞬间承载100万个独立的生产级数据库。

## 三条工程约束

1. 数据库实例粒度是"每终端用户一个"——规模不可接受
2. Schema是LLM现场生成的——改错可能数据不可恢复
3. 负载分布"零-峰两极"——闲置近乎零，爆款百倍以上

## Kimi的选择：TiDB Cloud + 三个关键决策

决策一：Serverless Cluster多租户——"虚拟数据库界面"层，长尾租户不真实分配实例，请求瞬间由DB Session Gateway维持连接

决策二：统一技术栈——vector + SQL + JSON一体化，一条SQL同时完成：按用户过滤 + 按标签筛选 + 向量相似度排序 + 时间倒序

决策三：Warm Pool + Scale-to-Zero——预维护Starter实例池，新实例从预热池直接分配，Agent可1秒内拿到fully prepared instance

## 行业信号

TiDB Cloud新建集群中，超过90%由AI Agent直接创建。

Dify案例：合并到TiDB Cloud后，基础设施成本降80%，运维负担降90%。

## Agent时代计算单位变革

| 时代 | 计算单位 | 核心需求 |
|------|---------|---------|
| Web时代 | 用户 | 扛几亿人同时来 |
| 移动时代 | 会话 | 扛几亿个并发会话 |
| Agent时代 | Agent自己 | 每个用户身边10个、100个独立Agent实例，每个都要有自己的状态、记忆、数据 |

## TiDB产品演进

- mem9：Agent持久化、跨session可检索的memory层
- drive9：Agent Sandbox的持久、共享、可挂载workspace

核心洞察：AI应用上半场比模型，下半场比地基。

原文：https://mp.weixin.qq.com/s/NNCfsnhAHuqtRQrMVr0gog
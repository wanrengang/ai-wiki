# AI Agent总失忆？Mem0+DeepSeek一小时搭好记忆系统

> 来源：增长虾（陈昀）
> 日期：2026年5月18日 20:03

## 核心问题

Agent 默认就是"金鱼脑子"。做 AI Agent 开发，被问最多的一个问题是：为什么 Agent 每次对话都跟失忆一样？

**什么时候必须加记忆系统？两个判断标准：**
1. **需要跨会话保持上下文**。比如私人助手，今天问"明天有什么安排"，明天问"这周工作重点是什么"，Agent 得记住之前聊过的内容。
2. **需要理解用户的长期偏好**。比如销售 Agent，每个客户有自己的背景、需求、跟进进度，这些信息跨会话都有效。

## 方案选型

| 方案 | 评价 |
|------|------|
| Mem0 + OpenAI | 成熟度高，但国内访问不稳，超时是常有的事 |
| Mem0 + Ollama本地部署 | 数据全在本地，隐私没问题，但本地小模型做复杂推理吃力 |
| **Mem0 + DeepSeek API** | **国内直连，速度快，价格亲民，风险可控 ✅** |
| 自建RAG | 灵活但工程量大，周期等不起 |

**结论**：想快速落地，Mem0 + DeepSeek API 是最务实的选法。

## 技术架构

### 存储流程
用户输入 → Mem0框架接收 → DeepSeek负责提取关键信息 → bge-small模型把文字转成384维向量 → 存入Qdrant向量数据库

### 检索流程
用户提问 → bge-small模型转成查询向量 → Qdrant做语义搜索 → 返回相关记忆 → 拼到Prompt里给Agent

## 踩坑记录

- **坑一**：DeepSeek没有Embedding接口 → 用 bge-small-en-v1.5（384维）替代
- **坑二**：向量维度要对齐 → 配置 `embedding_model_dims: 384`
- **坑三**：macOS的/tmp是软链接 → 用Docker跑Qdrant或清两个目录
- **坑四**：Python版本 → 用uv管理3.11.15环境

## 部署步骤

```python
from mem0 import Memory

config = {
    "llm": {"provider": "deepseek", "config": {"api_key": "your-key"}},
    "vector_store": {"provider": "qdrant", "config": {"host": "localhost", "port": 6333}},
    "embedder": {"provider": "sentence-transformers", "model": "BAAI/bge-small-en-v1.5", "embedding_dims": 384}
}
memory = Memory.from_config(config)

# 写入记忆
memory.add("用户是做销售的，最近在跟进一个大客户", user_id="user_001")

# 搜索记忆
results = memory.search("用户最近在忙什么", user_id="user_001")
```

## 局限

Agent的记忆系统不是银弹。**检索精度有上限**，语义检索毕竟不是精确匹配。

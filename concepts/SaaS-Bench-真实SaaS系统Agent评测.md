# SaaS-Bench：撕碎 Agent「全自动办公」幻想

> 来源：[新智元](https://mp.weixin.qq.com/s/L1tXbZqhXnLss9rq74kzsA) | 2026-05-25

## 背景

**SaaS-Bench** 是 UniPat AI 推出的全新评测基准，直接把真实 SaaS 系统搬进 Docker，让 Agent 在真实的前后端逻辑、数据库状态和业务约束中干活。

- **23个真实 SaaS 系统**，覆盖 6 个专业领域
- **106 个任务**，97.3% 的文本任务操作步数超过 100 步，最长轨迹达 300+ 步
- **93.4%** 的任务跨越至少两个应用

## 覆盖领域

| 领域 | 系统 |
|------|------|
| 软件研发 | OpenProject、Baserow、Code-Server、Metabase |
| 业务财务 | Twenty CRM、BigCapital、HRMS、Pretix |
| 医疗管理 | OpenEMR、OpnForm、OnlyOffice |
| 团队协作 | SiYuan、Roundcube、Mattermost、ownCloud |
| 农业供应链 | FarmOS、Grocy、Recipya、E-Label |
| 独立媒体 | PhotoPrism、MediaCMS、BookLore、Watcharr |

## 核心结论：全军覆没

| 模型 | Checkpoint Score | Resolved Score（完全通过） |
|------|-----------------|--------------------------|
| Claude Opus 4.7 | 43.9% | **3.8%**（106个任务中只完成4个） |
| Claude Sonnet 4.6 | — | — |
| Kimi K2.5 | — | **0%** |
| Gemini 3.1 Pro | — | **0%** |

**结论**：Benchmark 成绩与真实办公能力之间存在巨大鸿沟。

## 四种结构性失败模式

### 1. 越往后越做不对
- 即使每个检查点通过率高达 95%，12 个检查点的全部通过概率也只有 54%
- 所有模型都呈现通过率随任务推进呈下降趋势，没有一个模型能在后半段维持住前期表现

### 2. 一步错，步步错
- 案例：任务要求创建公司客户「Arcturus Digital」，Agent 同时填了联系人姓名和公司名，触发了个人客户逻辑，实际创建的是个人客户
- 上游 3% 的错误节点，导致了下游 30% 的分数损失

### 3. 做完不检查，自以为对了
- Agent 在意图层面认为成功，但验证器在状态层面发现失败
- 当前 CUA 框架缺少「严谨的反思闭环」—— Agent 是个不会检查自己作业的学生

### 4. 同一张考卷，成绩忽高忽低
- Claude Sonnet 4.6 在同一任务的三次独立运行中，分数范围从 0.00 到 0.68
- 路径依赖：模型在某个决策点的微小差异，会导致后续执行轨迹完全分叉

## 关键术语

| 术语 | 含义 |
|------|------|
| Computer-Use Agent | 能操作计算机（浏览器/桌面）执行任务的 AI Agent |
| GUI Agent | 基于图形界面操作的 Agent |
| Resolved Score | 端到端完全通过分数（严格） |
| Checkpoint Score | 检查点通过比例（宽松） |
| Cross-App | 跨应用协作（一个任务跨越多个 SaaS 系统） |
| Long-Horizon Task | 长程任务（操作步数100+） |
| Pass@k | 独立运行 k 次中有一次通过即算通过 |
| SaaS-Bench | 面向真实 SaaS 系统的 Agent 评测基准 |

## 启示

1. **当前 Agent 缺少对持久状态的有效推理能力**
2. **缺少操作后的闭环验证机制**
3. **缺少从错误中恢复的能力**
4. **未来方向**：不是让 Agent 学会操作人类的软件，而是软件本身要为 Agent 重新设计

## 相关链接

- Blog：https://unipat.ai/blog/SaaS-Bench
- GitHub：https://github.com/UniPat-AI/SaaS-Bench
- 论文：https://arxiv.org/abs/2605.15777

# PilotDeck：清华团队开源 Agent 操作系统

> 来源：新智元（ASI启示录）
> 日期：2026-05-28
> 链接：https://mp.weixin.qq.com/s/TdfsW6iCBmy3esAQ78LkyQ

## 核心要点

**PilotDeck** 由清华大学 THUNLP 实验室、面壁智能、OpenBMB 与 AI9stars 联合研发并开源，是一款以 **WorkSpace（工作舱）** 为核心设计的 Agent 操作系统。

## 三大核心能力

### 1. WorkSpace 级隔离 — 每个人项目独立「工作舱」

- 专属文件系统：项目文件边界清晰
- 专属记忆：Project Memory 记项目定义，Collaboration Feedback 记偏好，全部可见可改
- 专属技能：Skill 应用商店按项目安装

**与其他产品的本质区别**：Claude Cowork / Cursor 的 WorkSpace 本质是「文件夹+规则」，PilotDeck 的 WorkSpace 是 **AI 的完整生存环境**。

### 2. 智能路由 — Token 成本狂降

- **子 Agent 级别路由**，而非 request 级别（避免模型频繁切换打断 KV-cache）
- 复杂任务用强模型，简单任务用便宜模型
- 支持自定义路由规则，可用自然语言配置

**实测数据**：
- 程序员人格测试：不开路由 $10.97 → 开路由 $1.42（省 75%）
- 小红书内容生成：不开路由 $12.58 → 开路由 $2.83（省约 70%）
- 复杂任务：主 Sonnet 4.6 + 子 MiniMax-M2.7 花 $3.15、得分 70.6；单体 Sonnet 4.6 花 $18.36、得分 69.1（1/6 价格，效果略好）

### 3. 白盒记忆 — 打开 AI 大脑，逐条改

- 每条记忆标注时间戳、来源路径、类型
- 记错了可直接改，记忆冲突可直接删
- **Dream 机制**：空闲时段 AI 自动整理记忆，支持一键回滚

## 实测演示

同时跑两个 WorkSpace：
- 奶茶店模拟经营游戏（进货、定价、排队系统）
- 全球 AI 融资数据可视化大屏（交互式图表）

两者记忆完全隔离，互不干扰。

## 开源信息

- GitHub：https://github.com/OpenBMB/PilotDeck
- 官网：https://pilotdeck.openbmb.cn/
- 协议：AGPL 3.0

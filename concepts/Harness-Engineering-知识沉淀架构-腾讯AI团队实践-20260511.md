---
title: Harness Engineering 知识沉淀架构
description: 腾讯 AI 团队提出的"知识优先"工程实践：五层存储 × 五种类型 × 三级成熟度的知识分层架构，让团队知识成为真正的技术护城河
type: concept
tags:
  - Harness Engineering
  - 知识管理
  - Agent
  - 工作流
  - AI工程
  - 团队知识库
created: 2026-05-14
source: https://mp.weixin.qq.com/s/JV4-oPP0jjsBCZ4tW3Gy1g
author: stevenpxiao（腾讯程序员）
---

# Harness Engineering 知识沉淀架构

## 核心论点

> 构建 Harness 工作流不是最终目的，私域和团队知识的沉淀才是真正的技术护城河。

**核心理念**：Skill、Agent、工具链会随模型迭代更新，但领域知识是永恒的。

**出处**：腾讯 AI Team 工程交付编排系统实践

---

## 知识分层架构：五层存储 × 五种类型 × 三级成熟度

### 五层存储

| 层级 | 内容 | 共享范围 |
|---|---|---|
| Layer 0-P | 个人偏好 | 纯本地，不共享 |
| Layer 0-T | 团队约定（代码规范、Commit 规范） | 团队级，Git 共享 |
| Layer 1 | 技术知识（跨项目通用技术经验） | 团队级，跨项目 |
| Layer 2 | 业务知识（领域模型、业务规则、流程） | 团队级，按领域 |
| Layer 3 | 项目知识（仅当前项目有意义） | 项目级，随项目走 |

**关键设计**：知识可以"向上提升"——Layer 3 项目知识若判定为跨项目通用，自动提升到 Layer 1 或 Layer 2。

### 五种知识类型（MECE 原则）

| 类型 | 定义 | 示例 |
|---|---|---|
| model | 实体定义、数据结构、关系图 | "广告计划包含预算/出价/投放时段三个核心字段" |
| decision | 技术选型、架构决策及理由 | "选择事件驱动而非 RPC 同步，因为广告状态变更需要解耦" |
| guideline | 推荐做法 (recommend) 或禁止做法 (avoid) | "公共模块变更后的兼容性检查清单" |
| pitfall | 已知风险、故障模式、排查步骤 | "广告预算扣减在高并发下会超扣" |
| process | 业务流程、状态机、操作步骤 | "广告审核：提交→机审→人审→上线" |

### 三级成熟度 + 自动衰减

```
draft → verified → proven
```

| 触发条件 | 衰减动作 |
|---|---|
| proven 12个月未被引用 | 降级为 verified |
| verified 6个月未被引用 | 降级为 draft |
| draft 持续未引用 + Lint标记 | 归档，移出活跃索引 |

---

## 团队知识库架构

### 独立 Git 仓库

```
team-knowledge.git
├── knowledge-catalog.md      # 全景目录（Agent 查询入口）
├── .knowledge-config.yaml   # 团队配置
├── team-conventions/        # Layer 0-T
├── tech-wiki/              # Layer 1
├── biz-wiki/{domain}/      # Layer 2
├── project-profiles/
└── contributions/          # pending/ + conflicts/
```

### 三种团队角色

| 角色 | 权限 | 适用人群 |
|---|---|---|
| maintainer | 裁决冲突、审批 proven 提升、管理成员 | 团队负责人 |
| contributor | 通过工作流自动贡献 | 正式团队成员 |
| reader | 只消费不贡献 | 新成员试用期 |

### 区块链启发的贡献机制

- **不可篡改日志**：log.md 只追加不修改
- **贡献可溯源**：evidence.contributors[] 粒度为知识条目级
- **共识机制**：draft→verified 需1人验证；verified→proven 需≥2人+≥2项目

---

## 三级渐进式索引（按需查询）

| 层级 | 文件 | 大小 | 作用 |
|---|---|---|---|
| Layer A | knowledge-catalog.md | ~50行 | "知识库有什么？" |
| Layer B | 各目录 catalog.md | ~100-300行 | "这个分类有哪些条目？" |
| Layer C | TK-*.md / BK-*.md | ~50-200行 | "这条知识说了什么？" |

---

## 工作流 × 知识沉淀

**三个关键时刻**：
1. **INIT**：git pull 知识仓库，注入查询入口
2. **各阶段执行中**：按需查询（各阶段有独立查询预算和焦点）
3. **ARCHIVE**：自动提取知识条目 → 决策变 decision，坑变 pitfall

---

## 核心洞见

1. **工作流只是管道，知识才是流过管道的活水**
2. **没有知识闭环的工作流是"一次性"的**——上次踩的坑下次照踩
3. **知识是团队的"复利资产"**——proven 知识库让新成员"站在前人肩上"
4. **知识也会过时**——自动衰减机制防止过时知识误导 Agent

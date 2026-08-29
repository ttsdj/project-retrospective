---
name: project-retrospective
description: 常驻 IDE 的项目复盘与面试知识引擎。在开发对话、代码讨论、Debug、架构设计、技术选型等过程中持续提取项目知识，从碎片化上下文逆向重建工程完整脉络，将系统架构、模块边界、核心链路、数据 Schema、技术选型、历史 Bad Case、根因与方案、替代方案、Trade-off、性能指标、架构演进沉淀为连贯的叙事文档（多段自然语言 + ASCII 线条流程图/架构图），用于项目复盘与面试递进式盘问。每个开发任务完成、bug 修复定案、架构/技术决策落地后自动触发沉淀。
---

# Project Retrospective（项目复盘与面试知识引擎）

> 常驻 IDE。
> **Never record only what was built. Reconstruct why it was built this way, what failed before it, what alternatives existed, and what trade-offs were accepted.**
> 经典一句话：**「做了什么」只是结果，「当初怎么想、为什么这么选、放弃了什么」才是面试真正要考的东西。**

## 何时使用（常驻自动触发）

在以下场景结束后**立即沉淀**（不必等用户显式指示）：

- 完成一次开发任务 / 功能实现
- 修复一个 bug / 排查一次异常
- 做了一次架构设计 / 模块划分 / 重构
- 讨论了一次技术选型 / 方案对比 / trade-off
- 梳理了核心链路 / 数据流 / 状态机
- 产出或改动了数据 Schema / 接口契约 / Prompt

用户主动要求「沉淀 / 复盘 / 记录一下 / 整理成面试材料」时，同样触发。

## 沉淀什么（知识分类）

| 分类 | 要点 |
|------|------|
| 系统架构 | 分层 / 模块边界 / 依赖方向，配线条架构图 |
| 核心链路 | 端到端请求流、事件流、状态转移，配线条流程图 |
| 数据 Schema | 关键实体、表/字段、关系、索引、迁移决策 |
| 技术选型 | 每个关键技术决策（见「决策七要素」） |
| 历史 Bad Case | 症状、根因、修复、可复用教训 |
| Trade-off | 取舍双方、为何选 A、放弃 B 的代价 |
| 性能指标 | P50/P95/P99、QPS、TTFT 等实测数据 |
| 架构演进 | 从旧到新的迁移路径、中间态、正殿/旁路 |

## 叙述风格（几段话，方便记忆）——硬规则

**禁止**写成纯 bullet 堆砌。**必须**用 3-6 段连贯的自然语言叙事，让读者能「讲出来、复述出来」。

一段话的标准：独立成句、因果清晰、口语可复述。典型节奏：
**背景为什么存在 → 核心难点/矛盾 → 怎么解决 → 结果与代价 → 踩过的坑/遗留问题**。

可以少量配合列表补充细节，但**主体是段落**。

## 图例规范（涉及架构必配线条图）

涉及架构、链路、状态机时**必须**配图。用纯 ASCII 线条字符（`│` `↓` `→` `├` `└` `┌┴┐`），与项目既有文档风格一致。

**垂直单向链（最常用）：**
```
User Message
    │
    ↓
Load State Snapshot
    │
    ↓
Resolve Turn Intent
    │
    ↓
Decision / Semantic Router
    │
    ↓
Raw TurnPlan
```

**分支 / 并行：**
```
TurnMode
    │
    ├→ Reply: ENCOURAGE_MORE_MATCH_CHAT
    │
    └→ Actions: []
```

**模块分层：**
```
router（HTTP 编排）
  │
  ↓
service（业务编排，禁写 SQL）
  │
  ↓
repository / infrastructure（数据访问）
  │
  ↓
DB
```

图外必须跟 1-2 句文字解释（不裸图）。

## 决策七要素模板

每个关键技术决策用七要素记录（缺资料的标「待补充」，**不可编造**）：

1. **背景**：当时面临什么问题 / 业务诉求
2. **约束**：成本、性能、团队规模、迁移、兼容等硬限制
3. **候选方案**：罗列 A / B / C（哪怕只是脑内对比）
4. **选择依据**：为何选 X（尽量给出可量化的对比）
5. **实现**：落地方式、关键文件/代码、踩的坑
6. **结果**：上线后效果、性能数据、是否达成预期
7. **遗留问题**：未解决的、可改进的、未来风险

## 知识库位置与文件

- **主场：飞书知识库「面经 / 实习 / cosense」**（位于个人库「天天睡大觉」内）——用户指定，所有沉淀**直接写入此处**，不落本地
- 定位路径（**已解析 token，直接复用，无需再查**）：
  - space_id（天天睡大觉）：`7655991149382880216`
  - 面经：`Wphnwh2IAidI3rk8OD3cgxs5nnc`
  - 实习：`DKt3wXoiiilD5tkqxPpct6HznGe`
  - **cosense（知识库根节点，子文档挂其下）**：`MmYQwm7CwiyP2wklb54c1TlTnQh`
- 在 cosense 节点下创建主题文档（推荐 `docs +create --parent-token` 一步挂 wiki 节点）：
  ```powershell
  lark-cli docs +create --doc-format markdown --title "文档标题" --content "@./相对路径.md" --parent-token MmYQwm7CwiyP2wklb54c1TlTnQh
  ```
- 成功返回 `data.document.document_id` / `url`；`--parent-token` 接收 wiki 节点 token（此处=cosense）
- 详细文件结构与写入流程见 [reference.md](reference.md) §知识库文件结构

## 写入流程

1. 从当前对话/任务提取新知识点
2. 判断归属哪个主题文件；已有相似条目则**更新演进**，否则**新增**
3. 用「几段话 + 图」方式写入对应 md
4. 关键决策补全七要素
5. 涉及架构、链路随时配线条图

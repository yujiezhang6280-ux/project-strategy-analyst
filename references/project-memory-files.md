# 项目记忆文件

每个使用此 skill 的长期项目，都在当前工作区 `_project_memory/` 下维护这些本地文件。

## 必需文件

### `00-critical.md`

用于保存必须穿越长上下文窗口的高优先级事实。

建议结构：

```md
# Critical Context

## Current Goal
- 

## Current Strategy
- 

## Confirmed Definitions
- 

## High-Risk Pitfalls
- 

## Latest Decisions
- YYYY-MM-DD:
```

## 继承文件

### `parents.md`

当前项目需要继承公司级、组合级或其他父项目记忆时创建。独立项目可以省略。

建议结构：

```md
# Parent Project Memory

## company
- Path: /absolute/path/to/company/_project_memory
- Scope: 公司级策略、共享指标定义、共享数据字典、跨项目决策。
- Load when: 任务涉及公司上下文、共享定义、通用表、历史策略或跨项目对比。
- Update policy: 默认只读；只有用户明确要求修改公司级上下文，或新事实明显跨子项目适用时才更新。

## Load Order
1. 当前用户消息
2. 子项目 `_project_memory`
3. 父项目 `_project_memory`
4. 全局 Codex memories

## Override Rules
- 在当前子项目内，子项目记忆覆盖父项目记忆。
- 不要把父项目事实复制到子项目文件，除非存在子项目专属覆盖、caveat 或决策。
- 子项目覆盖要在相关子项目记忆文件中写明日期和范围。
```

### `01-background.md`

用于保存丰富项目背景和叙事上下文。

建议结构：

```md
# Project Background

## Project Summary

## Business Flow

## Stakeholders and Roles

## Historical Changes

## Notes
```

### `02-data-dictionary.md`

用于保存数据集、表和字段定义。

建议结构：

```md
# Data Dictionary

## table_name

### Table-Level Notes
- Entity grain:
- Time fields:
- Primary key or near-key:
- Common filters:
- Main join risks:

### Fields
| Field | Meaning | Type or Format | Grain | Null Meaning | Definition Notes |
|---|---|---|---|---|---|
```

### `03-metric-logic.md`

用于保存指标定义和对账规则。

建议结构：

```md
# Metric Logic

## metric_name
- Business definition:
- Formula:
- Numerator:
- Denominator:
- Time grain:
- Entity grain:
- Source tables:
- Key filters:
- Exclusions:
- Validation:
- Common misunderstandings:
```

### `04-sql-notes.md`

用于保存可复用 SQL 拆解和 query 派生业务逻辑。

建议结构：

```md
# SQL Notes

## query_topic
- Business question:
- Base table and grain:
- Key joins:
- Key filters:
- Aggregation:
- Dedup logic:
- Output metrics:
- Risks:
- Conflicts with existing definitions:
```

### `99-index.md`

大型项目的低 token 路由图。

建议结构：

```md
# Project Memory Index

## Critical First
- 分析前始终先读 `00-critical.md`。
- 如果存在 `parents.md`，在子项目 `00-critical.md` 后读取它，再判断是否需要父项目上下文。

## Topic Map
| Topic | Load File | Keywords | Why Load |
|---|---|---|---|
|  |  |  |  |

## Metric Map
| Metric | Load File | Related Tables | Notes |
|---|---|---|---|
|  |  |  |  |

## SQL Map
| SQL Pattern or Topic | Load File | Tables | Risks |
|---|---|---|---|
|  |  |  |  |

## Parent Map
| Parent | Path | Scope | Load When |
|---|---|---|---|
|  |  |  |  |
```

### `98-chat-intake.md`

用于在上下文压缩前保存开场对话中的高价值事实。

建议结构：

```md
# Chat Intake

## Source Window
- Captured from visible early chat turns:
- Capture date:

## Project Setup Facts
- 

## User-Stated Goals
- 

## Data and SQL Clues
- 

## Metric and Definition Clues
- 

## Constraints and Preferences
- 

## Items Promoted Elsewhere
| Item | Promoted To | Notes |
|---|---|---|
|  |  |  |
```

不要把此文件当完整 transcript。它只是把 durable facts 推进主项目记忆文件前的暂存区。

### `97-retrospective.md`

用于项目复盘和项目本地 lessons。

建议结构：

```md
# Project Retrospective

## Completed Work
- 

## Valuable Findings
- 

## Decisions and Strategy Changes
- 

## Data or Metric Lessons
- 

## SQL and Analysis Patterns
- 

## Failed Assumptions or Risks
- 

## Open Follow-Ups
- 

## Global Memory Candidates
| Candidate | Suggested Target | Why Reusable | Status |
|---|---|---|---|
|  |  |  | pending |
```

项目专属内容留在此文件。只有广泛可复用的 lesson 才提升到全局记忆。

## 可选文件

- `05-open-questions.md`：未解决的数据或策略问题。
- `06-meeting-notes.md`：蒸馏到必需文件前的原始会议笔记。

## 规则

如果一条笔记重要到会影响未来回答，不要只留在 raw notes。必须提升到某个必需文件。

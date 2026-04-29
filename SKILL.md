---
name: project-strategy-analyst
description: >
  用于项目策略分析、项目记忆维护、父子项目上下文、数据字典、指标口径、
  SQL 业务拆解、问题诊断、指标分析、实验与因果审查、统计推理、
  算法/模型评估、产品增长分析、结构化报告和笔记方法论蒸馏。适合在需要恢复
  长期项目背景、维护项目记忆、审计 SQL 指标逻辑、拆解业务问题、区分相关性
  与因果性、生成可读分析报告或沉淀可复用方法论时使用。
---

# Project Strategy Analyst

## 概览

把这个 skill 当作一个长期项目的操作系统。它负责维护工作区内的 `_project_memory/`，让后续对话能恢复项目策略、背景、数据定义、指标口径、父子项目继承关系和 SQL 业务逻辑。需要分析方法时，先通过精简的 methodology references 路由，不直接加载原始笔记或所有框架。

## 快速开始

1. 检查当前工作区是否已有 `_project_memory/`。
2. 如果没有，按 `references/project-memory-files.md` 创建项目记忆文件。
3. 优先读取 `_project_memory/00-critical.md`；如存在，再读 `_project_memory/parents.md` 和 `_project_memory/99-index.md`；`01-background.md`、`02-data-dictionary.md`、`03-metric-logic.md`、`04-sql-notes.md` 只在需要时读取。
4. 如果 `parents.md` 指向父项目记忆，只有当前任务需要公司级策略、共享指标定义、通用数据字典或历史上下文时，才读取父项目 `00-critical.md` 和 `99-index.md`。
5. 如果任务涉及分析方法、诊断、指标解释、实验复盘、因果、统计、算法、产品增长、报告结构或笔记蒸馏，先读 `references/methodology-index.md`，再只加载它路由到的 1-3 个方法文件。
6. 如果这是新项目或刚恢复的项目，并且早期聊天还可见，先把可复用事实写入 `_project_memory/98-chat-intake.md`，避免压缩后丢失。
7. 回答前按 `references/update-policy.md` 判断是否需要更新项目记忆。
8. 用户提供 SQL 时，先按 `references/sql-decomposition-checklist.md` 做 SQL 业务拆解；只有问题需要诊断、指标、因果、统计、建模或报告结构时，才额外加载方法文件。

## 核心工作流

### 1. 按优先级加载上下文

- `_project_memory/00-critical.md` 是最高优先级项目记忆。
- `_project_memory/parents.md` 是父项目继承地图，不存背景正文。
- `_project_memory/99-index.md` 是大型项目的低 token 路由图。
- 不要让 `01-background.md` 覆盖或稀释 `00-critical.md` 中的确认事项。
- 只读取当前任务需要的最少文件。
- 当前请求涉及策略、定义或已知陷阱时，先加载相关项目记忆再推理。
- 子项目有 `parents.md` 时，优先级为：当前用户消息、子项目记忆、父项目记忆、全局记忆。
- 父项目记忆默认只读。只有用户明确要求修改公司级上下文，或新事实明显跨子项目复用时，才更新父项目。
- 子项目与父项目冲突时，当前子项目内以子项目规则为准，并记录或提示冲突，不要静默合并。
- 长期项目要先搜索 `_project_memory/`，不要假设压缩后的聊天摘要完整。
- 如果压缩前聊天不可见，除非已写入项目记忆或外部 transcript，否则不要假装可恢复。

### 1A. 父子项目记忆

当子项目依赖共享公司背景、通用表定义、共享指标口径、公司策略或跨项目决策时，使用父项目记忆。

子项目中的 `parents.md` 形状：

```md
# Parent Project Memory

## parent_name
- Path:
- Scope:
- Load when:
- Update policy:
```

规则：

- `Path` 必须指向父项目的 `_project_memory/`。
- 先读父项目 `00-critical.md`，再读父项目 `99-index.md`；明细文件只在索引或当前任务要求时读取。
- 不要把父项目事实复制进子项目，除非子项目需要覆盖、例外、限制或决策。
- 公司级事实写父项目；实验、活动、客户、SQL 或局部 caveat 写子项目。
- 子项目覆盖父项目定义时，在子项目 `00-critical.md` 或相关指标/数据文件写清日期和适用范围。
- 更新记忆前先决定写入目标：共享持久上下文写父项目，局部上下文写子项目；只有共享规则和局部例外同时成立时才两边都写。

### 1B. 方法论层

methodology references 是共享分析手册，只指导如何推理，不存项目事实。

规则：

- 需要方法论时，必须先读 `references/methodology-index.md`。
- 只加载最小方法集合，默认 1-3 个文件。
- SQL 是独立工作流：有 SQL 时先读 `references/sql-decomposition-checklist.md`，再按问题需要补充诊断、指标、因果、统计、建模或报告方法。
- 可复用方法写 skill references；公司事实写父项目 `_project_memory/`；子项目事实写子项目 `_project_memory/`。
- 不要把原始笔记页面、截图、PDF、书摘、公司事实、项目事实或长摘录写进 skill。
- 蒸馏笔记时，只产出短规则、使用场景和来源路径；不要复制长原文。
- 方法与项目记忆或用户事实冲突时，以用户事实和项目记忆为准；方法只提供分析结构。
- 没有合适方法文件时，先用 `references/note-distillation.md` 形成候选方法摘要，再决定是否改 skill。

### 2. 回答当前任务

- 区分稳定事实、工作假设和开放问题。
- 分析业务表现时，明确区分相关性观察和因果性判断。
- 指标不清楚时，先说清分母、时间粒度、实体粒度、过滤条件和排除项。
- 请求包含 SQL 时，先反推业务意图，再下结论。
- 使用方法文件时，简短说明分析视角，并把结论落在当前项目证据上。
- 如果任务要求最终分析产物，按数据量和复杂度选择可审计形态：小数据优先 Excel 并保留公式/透视表，大数据优先 Jupyter 并保留完整过程数据和中文注释。
- 如果加载的文件没有所需上下文，先在 `_project_memory/` 搜索相关指标名、表名、活动名、SQL alias 和业务关键词，再让用户补充。
- 如果早期对话仍可见且包含项目设置，先扫描一次并沉淀持久事实，再深入回答。

### 3. 有意义的对话后更新项目记忆

- 只有新增非平凡、可复用上下文时才更新项目记忆。
- 子项目有 `parents.md` 时，先决定写父项目还是子项目。
- 优先编辑已有 bullet，避免近似重复。
- 只有最影响决策的信息才提升到 `00-critical.md`。
- 新增重大 topic、指标、表或 SQL 模式时，更新 `99-index.md`。
- 早期对话提供可复用项目设置时，更新 `98-chat-intake.md`。
- 项目有明显进展、决策、失败、分析模式或复用经验时，更新 `97-retrospective.md`。
- 背景、干系人、一次性解释写 `01-background.md`。
- 字段定义写 `02-data-dictionary.md`。
- 指标公式、维度切分、对账规则写 `03-metric-logic.md`。
- 可复用 SQL 拆解、指标陷阱、表角色写 `04-sql-notes.md`。

## 文件规则

### `_project_memory/00-critical.md`

- 保持短、硬、难以被背景噪音覆盖。
- 保存必须穿越长对话的内容：
  - 当前项目目标
  - 当前策略或决策规则
  - 不可随意改的指标定义
  - 会导致结论失效的数据陷阱
  - 最新确认决策
- 需要时用日期或置信度标注。
- 不要自动压缩成泛泛 prose summary。

### `_project_memory/parents.md`

- 只在当前项目继承一个或多个父项目时使用。
- 保持为路由地图：父项目路径、范围、加载条件、更新策略。
- 不存父项目事实；事实写父项目自己的记忆文件。
- 多个父项目时，写明加载顺序和各自负责范围。

### `_project_memory/01-background.md`

- 存有用但非最高优先级的丰富背景：
  - 项目背景
  - 干系人上下文
  - 历史演变
  - 业务流程
  - 示例案例和叙事说明
- 可以积极总结，但不要把应进入 `00-critical.md` 的内容总结掉。

### `_project_memory/02-data-dictionary.md`

- 每个表或数据集一个 section。
- 每个字段记录业务含义、粒度、常见过滤、null 语义和 join caution。
- 用户对同一字段有多个叫法时，记录 alias。

### `_project_memory/03-metric-logic.md`

- 每个指标记录：
  - 白话定义
  - 公式
  - 分母
  - 来源表和必要 join
  - 时间粒度
  - 实体粒度
  - 排除项和异常状态
  - 校验或对账检查
- 用户争论一个数字时，先更新此文件，再输出漂亮结论。

### `_project_memory/04-sql-notes.md`

- 记录 SQL 暴露出的可复用业务逻辑。
- 捕获：
  - SQL 想测什么
  - 基表和行粒度
  - join 路径
  - 过滤条件和排除项
  - 聚合层级
  - window function 或去重逻辑
  - 业务上可能对应的指标名
  - caveat 和 failure mode

### `_project_memory/99-index.md`

- 作为低 token 路由图。
- 只存指针，不写长解释：
  - topic 名
  - 相关文件
  - 关键实体、指标、表或 SQL alias
  - 为什么要加载的一句话说明
- 项目变大后，在 `00-critical.md` 之后读取。

### `_project_memory/98-chat-intake.md`

- 作为早期对话的持久 intake 摘要。
- 开场消息包含项目背景、目标、数据源、指标 caveat、SQL 或约束时创建或更新。
- 只存蒸馏事实和指针，不粘贴完整聊天记录。
- 写完 intake 后，把 critical 信息提升到 `00-critical.md`，可路由 topic 写进 `99-index.md`，字段定义写 `02-data-dictionary.md`，指标定义写 `03-metric-logic.md`。
- 早期聊天已经压缩且无 transcript 时，标记 unavailable，不要编造缺失上下文。

### `_project_memory/97-retrospective.md`

- 存项目级复盘：做了什么、什么重要、什么有效、什么失败、什么应复用。
- 项目专属细节留在这里；只有跨项目复用的经验才考虑进入全局记忆。
- 包括：
  - 完成工作和重大决策
  - 高价值发现
  - 提升质量的分析动作
  - 失败假设或数据陷阱
  - 可复用 SQL 或指标模式
  - 后续机会
- 每次复盘后，评估是否有 lesson 应复制到全局长期记忆目录。

## Token Budget

使用分阶段加载：

1. Minimal mode：只读子项目 `00-critical.md`、必要时 `parents.md`、必要时 `99-index.md`。
2. Parent mode：只有任务涉及共享公司上下文或父项目定义时，才读父项目 `00-critical.md` 和 `99-index.md`。
3. Methodology mode：读 `references/methodology-index.md`，再读路由到的方法文件。
4. Targeted mode：只加载当前任务、子项目索引、父项目索引或方法索引指向的 section。
5. Search mode：先在子项目 `_project_memory/` 搜关键词，再按 `parents.md` 搜父项目。
6. Full mode：只有用户要求完整审计、迁移或 consolidation 时，才加载全部记忆文件。

背景文件变长后，优先搜索和 section-level read，不要整文件搬进上下文。

## 压缩后恢复

如果聊天被压缩、截断或持续很久：

1. 不要只相信压缩摘要。
2. 读取 `00-critical.md`。
3. 如存在 `parents.md`，且当前任务需要父项目上下文，则读取父项目 `00-critical.md` 和 `99-index.md`。
4. 如存在 `99-index.md`，读取它。
5. 如存在且相关，读取 `98-chat-intake.md`。
6. 搜索子项目 `_project_memory/`；父项目只搜父项目拥有的 topic。
7. 只加载匹配当前任务的 section。
8. 如果关键事实只存在于压缩摘要而不在项目记忆，先向用户确认，再写入正确记忆文件。

## 复盘与全局记忆

当用户要求 review、retrospect、总结已完成工作、识别有价值内容或提炼 learnings：

1. 读取子项目 `00-critical.md`、必要时 `parents.md`、子项目 `99-index.md`、如存在则读 `97-retrospective.md`。
2. 只有涉及共享公司决策、跨项目经验或父项目定义时才读取父项目记忆。
3. 搜索 `_project_memory/` 中的决策、完成工作、指标变化、SQL notes 和已解决问题。
4. 输出简洁复盘：
   - 做了什么
   - 项目理解发生了什么变化
   - 什么有价值
   - 什么未解决
   - 什么可在项目外复用
5. 更新子项目 `_project_memory/97-retrospective.md`。
6. 只有 lesson 明显跨子项目适用时，才更新父项目 retrospective。
7. 只有非显而易见、跨项目可复用或很可能复发的 lesson，才写入全局长期记忆。

全局记忆路由：

- `$CODEX_HOME/memories/LEARNINGS.md`：可复用分析实践、纠错、业务/数据分析启发式、持久用户偏好。
- `$CODEX_HOME/memories/ERRORS.md`：可能复发的工具失败、数据陷阱、debug 记录或错误。
- `$CODEX_HOME/memories/FEATURE_REQUESTS.md`：用户希望补齐的能力或工作流优化。

不要把敏感项目细节、一次性指标或临时背景写进全局记忆，除非用户明确要求。

## SQL 处理

用户给 SQL 时：

1. 复述 SQL 似乎要回答的业务问题。
2. 先识别基表和行粒度，再看聚合。
3. 列出每个 join，并说明它会扩行、过滤行还是补属性。
4. 列出每个 `where` 条件，并翻译成业务范围。
5. 识别 `distinct`、`row_number`、`group by`、anti-join 等去重逻辑。
6. 识别派生字段，说明它是维度、标签还是指标。
7. 区分 raw count、过滤后 count 和最终报告指标。
8. 如果 SQL 逻辑可能与 `_project_memory/03-metric-logic.md` 的指标定义冲突，明确指出。
9. 如果 SQL 暴露出可复用逻辑，更新 `_project_memory/04-sql-notes.md`。
10. 不要把通用方法论写入项目记忆，除非它是项目专属应用、caveat 或决策。

不要停留在语法解释，要把 SQL 翻译成业务含义。

## 更新触发条件

以下情况发生后，更新项目记忆：

- 用户提供新的项目目标、策略、限制或决策。
- 用户澄清指标真实含义。
- 用户解释之前不清楚的表、字段或 join。
- 用户提供的 SQL 暴露可复用业务逻辑。
- 之前假设被证明错误。

## References

- 创建或修复 `_project_memory/` 时，读 `references/project-memory-files.md`。
- 决定覆盖、追加、提升或忽略时，读 `references/update-policy.md`。
- 任务包含 SQL 分析时，读 `references/sql-decomposition-checklist.md`。
- 任务需要分析方法时，读 `references/methodology-index.md`，再只加载被路由到的方法文件。

# project-strategy-analyst

一个面向 Codex 的项目策略分析师 skill。

它用于长期项目中的策略分析、项目记忆、指标口径、SQL 业务拆解、问题诊断、数据审计轨迹和可审计分析产物。它的目标不是把回答写得更像报告，而是让 Codex 在长项目里持续知道：

- 当前项目目标是什么
- 关键指标和数据口径是什么
- 哪些结论已经确认，哪些只是工作假设
- 父项目和子项目的上下文如何继承
- 分析过程里的数据和计算能不能复查
- 最终产物应该用 Excel 还是 Jupyter 承载

## 一句话理解

`project-strategy-analyst` 是一个给 Codex 用的项目分析操作系统。

它把工作拆成四层：

| 层级 | 作用 | 存放位置 |
|---|---|---|
| 项目记忆 | 保存当前项目事实、目标、指标、SQL 口径和决策 | 当前项目的 `_project_memory/` |
| 父子项目 | 让多个子项目继承同一个公司级或组合级上下文 | 子项目 `_project_memory/parents.md` 指向父项目 `_project_memory/` |
| 方法论 | 提供通用分析方法、检查清单和输出结构 | skill 的 `references/*.md` |
| 可审计产物 | 保存最终分析结果和过程证据 | 小数据用 Excel，大数据用 Jupyter |

## 仓库结构

```text
project-strategy-analyst/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── methodology-index.md
    ├── metric-analysis.md
    ├── problem-diagnosis.md
    ├── experiment-and-causality.md
    ├── statistical-methods.md
    ├── algorithm-and-modeling.md
    ├── product-growth-methods.md
    ├── structured-thinking-and-reporting.md
    ├── sql-decomposition-checklist.md
    ├── project-memory-files.md
    ├── update-policy.md
    └── note-distillation.md
```

核心文件：

| 文件 | 用途 |
|---|---|
| `SKILL.md` | skill 入口，定义何时触发、如何加载项目记忆和方法论 |
| `agents/openai.yaml` | Codex UI 展示名、简短说明和默认 prompt |
| `references/methodology-index.md` | 方法论路由入口，决定当前任务该加载哪些 reference |
| `references/project-memory-files.md` | 说明 `_project_memory/` 应该有哪些文件 |
| `references/update-policy.md` | 说明对话后哪些信息该写入哪类记忆 |
| `references/sql-decomposition-checklist.md` | SQL 业务拆解和审计清单 |
| `references/structured-thinking-and-reporting.md` | 报告结构、表格规则、Excel/Jupyter 产物规则 |

## 项目记忆结构

使用这个 skill 的项目，建议在项目根目录维护一个 `_project_memory/`：

```text
your-project/
└── _project_memory/
    ├── 00-critical.md
    ├── parents.md
    ├── 01-background.md
    ├── 02-data-dictionary.md
    ├── 03-metric-logic.md
    ├── 04-sql-notes.md
    ├── 97-retrospective.md
    ├── 98-chat-intake.md
    └── 99-index.md
```

每个文件的作用：

| 文件 | 作用 | 什么时候读 |
|---|---|---|
| `00-critical.md` | 保存最重要、最不能丢的项目目标、指标定义、风险和决策 | 每次进入项目优先读 |
| `parents.md` | 指向父项目记忆，说明继承哪些共享上下文 | 子项目依赖公司级或组合级背景时读 |
| `01-background.md` | 保存较长背景、历史、业务流程和叙事说明 | 当前问题需要背景时读 |
| `02-data-dictionary.md` | 保存表、字段、含义、粒度、join 风险 | 涉及数据字段或表结构时读 |
| `03-metric-logic.md` | 保存指标公式、分母、过滤、排除项和对账规则 | 涉及指标解释或争论数字时读 |
| `04-sql-notes.md` | 保存可复用 SQL 逻辑、join 路径和查询陷阱 | 用户给 SQL 或复用查询逻辑时读 |
| `97-retrospective.md` | 保存项目复盘、经验、失败假设和可复用 lesson | review、复盘、总结时读 |
| `98-chat-intake.md` | 保存早期对话里的项目设置，防止上下文压缩后丢失 | 新项目或长对话恢复时读 |
| `99-index.md` | 保存低 token 路由图，告诉 Codex 哪些文件值得读 | 项目变大后读 |

## 父项目和子项目怎么用

父项目保存共享上下文，子项目保存局部上下文。

适合放父项目的内容：

- 公司级策略
- 多个项目共用的指标定义
- 通用数据字典
- 跨项目通用 SQL 口径
- 长期有效的分析规则

适合放子项目的内容：

- 当前实验、活动或专题的事实
- 当前项目自己的 SQL、指标 caveat 和分析结论
- 只在当前项目成立的例外规则
- 当前项目的时间窗口、业务范围和决策记录

子项目通过 `parents.md` 指向父项目：

```md
# Parent Project Memory

## company
- Path: /absolute/path/to/company/_project_memory
- Scope: 公司级策略、共享指标定义、共享数据字典、跨项目决策。
- Load when: 任务涉及公司上下文、共享定义、通用表、历史策略或跨项目对比。
- Update policy: 默认只读；只有明确要求修改共享上下文时才更新。
```

读取优先级：

1. 当前用户消息
2. 子项目 `_project_memory/`
3. 父项目 `_project_memory/`
4. 全局长期记忆
5. skill 的 `references/*.md`

写入原则：

- 公司级事实写父项目。
- 当前项目事实写子项目。
- 可复用方法写 skill reference。
- 个人偏好或反复纠错写全局长期记忆。
- 原始笔记、长摘录、私有案例和一次性材料不写进 skill。

## 方法论怎么调用

Codex 不应该一次性加载所有方法文件。

正确流程是：

1. 先读项目记忆，确认当前项目事实和口径。
2. 再读 `references/methodology-index.md`。
3. 由 index 路由到 1-3 个最相关的 reference。
4. 用 reference 的结构组织分析，但结论必须落在当前项目证据上。

常见路由：

| 任务 | 默认加载 |
|---|---|
| 指标下跌、异常、业务诊断 | `problem-diagnosis.md` + `metric-analysis.md` |
| SQL 审计 | `sql-decomposition-checklist.md` |
| 实验复盘、因果判断 | `experiment-and-causality.md` |
| 样本量、显著性、区间 | `statistical-methods.md` |
| 模型、预测、推荐、分类 | `algorithm-and-modeling.md` |
| 增长、产品策略、竞品 | `product-growth-methods.md` |
| 报告、老板摘要、图表叙事 | `structured-thinking-and-reporting.md` |
| 笔记提炼成方法论 | `note-distillation.md` |

## 怎么调用

在 Codex 里直接说：

```text
使用 project-strategy-analyst。
```

也可以把任务说清楚：

```text
使用 project-strategy-analyst。
请先读取当前项目的 _project_memory，再分析这个指标下跌问题。
输出数据正确性、数据审计轨迹、Driver Bridge、5Why、尚未证明和下一步检查。
```

创建项目记忆：

```text
使用 project-strategy-analyst。
请为当前项目创建 _project_memory，并初始化 00-critical.md、99-index.md、03-metric-logic.md 和 04-sql-notes.md。
```

连接父项目：

```text
使用 project-strategy-analyst。
当前项目是一个子项目，请创建 parents.md，指向父项目的 _project_memory，并说明什么时候读取父项目。
```

SQL 审计：

```text
使用 project-strategy-analyst。
请审计这段 SQL：先解释业务问题，再说明基表、行粒度、join、过滤、去重、指标口径、数据审计轨迹和风险。
```

可审计分析产物：

```text
使用 project-strategy-analyst。
这是一个数据分析任务。请根据数据量选择 Excel 或 Jupyter。
小数据保留公式/透视表，大数据用 Jupyter 留完整过程和中文注释。
```

## 记忆怎么维护

每次有意义的对话结束后，Codex 应判断是否需要更新 `_project_memory/`。

需要更新的情况：

- 用户澄清了项目目标、策略、限制或决策。
- 用户解释了指标真实含义。
- 用户解释了表、字段、join 或 SQL 口径。
- 某个假设被证明错误。
- 一段 SQL 暴露出可复用业务逻辑。
- 当前分析产生了未来会复用的 caveat、风险或检查规则。

不要更新的情况：

- 只是一次性闲聊。
- 只是临时背景，未来不会复用。
- 信息还没有确认。
- 内容属于原始笔记、长摘录或私有案例。
- 内容是通用方法论，应写入 skill reference，而不是项目记忆。

写入目标：

| 信息类型 | 写入位置 |
|---|---|
| 最关键目标、策略、定义、风险 | `00-critical.md` |
| 背景、业务流程、历史说明 | `01-background.md` |
| 表、字段、实体粒度、join 风险 | `02-data-dictionary.md` |
| 指标公式、分母、过滤、对账 | `03-metric-logic.md` |
| SQL 拆解、查询陷阱、可复用逻辑 | `04-sql-notes.md` |
| 早期对话项目设置 | `98-chat-intake.md` |
| 大项目路由索引 | `99-index.md` |
| 项目复盘和局部 lesson | `97-retrospective.md` |
| 公司级共享事实 | 父项目 `_project_memory/` |
| 跨项目方法论 | skill `references/*.md` |

维护方式：

1. 优先编辑已有条目，避免重复堆叠。
2. 重要信息先提升到 `00-critical.md`。
3. 项目变大后同步更新 `99-index.md`。
4. 子项目和父项目冲突时，以子项目当前范围为准，并记录 override。
5. 未确认信息标成假设，不写成事实。

## 输出应该长什么样

业务分析默认结构：

```md
## 结论
## Data Correctness（数据正确性）
## Data Audit Trail（数据审计轨迹）
## Driver Bridge（驱动项贡献拆解）或证据摘要
## 诊断
## 尚未证明
## 下一步检查
```

数据正确性检查输入和口径：

```md
| 检查项 | 当前状态 | 如果错误的风险 | 对结论的影响 | 下一步验证 |
|---|---|---|---|---|
```

数据审计轨迹检查计算过程：

```md
| 步骤 | 输入数字 | 计算/处理 | 输出数字 | 复算状态 | 备注 |
|---|---:|---|---:|---|---|
```

Driver Bridge 检查变化贡献：

```md
| 驱动项 | 上期 | 本期 | 变化 | 贡献 | 解读 |
|---|---:|---:|---:|---:|---|
```

## Excel 和 Jupyter 规则

小数据优先 Excel：

- 保留公式，不只写静态结果。
- 尽量使用表格、筛选、透视表、条件格式或可复算汇总表。
- 关键指标旁边保留分子、分母、单位、时间范围和过滤条件。
- 建议包含 `README/说明`、`Source/原始数据或输入`、`Calc/计算过程`、`Check/校验`、`Output/结论`。

大数据优先 Jupyter：

- 所有过程数据必须留痕。
- Markdown 说明、代码注释和检查说明使用中文。
- 每一步写清目的、输入、处理、输出、检查点和风险。
- 保留清洗、过滤、join、去重、聚合、中间行数、分子分母和残差检查。
- 最终 notebook 包含 `数据审计轨迹`、`口径说明`、`中间结果索引`、`结论` 和 `复算/残差检查`。

## 安装

把仓库克隆到 Codex skills 目录：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/yujiezhang6280-ux/project-strategy-analyst.git ~/.codex/skills/project-strategy-analyst
```

如果你已经下载了本仓库，也可以复制本地目录：

```bash
mkdir -p ~/.codex/skills/project-strategy-analyst
cp -R . ~/.codex/skills/project-strategy-analyst
```

安装后重启 Codex 或新开会话，让 skill 元信息重新加载。

## 边界

- 不包含原始笔记、日记、长摘录或私有案例。
- 不内置数据库连接。
- 不保存公司事实或项目事实；这些应进入项目自己的 `_project_memory/`。
- 不把相关性直接写成因果；证据只能支撑到哪一层，就明确停在哪一层。

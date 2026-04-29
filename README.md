# project-strategy-analyst

一个面向 Codex 的项目策略分析师 skill。

它把“项目记忆、指标口径、SQL 业务拆解、问题诊断、数据审计轨迹、可审计分析产物”放到同一套工作流里，适合长期项目、业务复盘、数据分析、增长诊断和策略判断场景。

核心目标不是让回答更花哨，而是让分析过程更可追溯：结论从哪里来、数据能不能信、计算能不能复算、哪些判断还没有被证明。

## 适合做什么

- 维护 `_project_memory/` 项目记忆。
- 记录和恢复项目目标、背景、关键决策、指标定义、数据字典和 SQL 口径。
- 区分父项目、子项目、全局记忆和 skill reference 的归属边界。
- 拆解业务指标下跌、漏斗转化、收入变化、留存、增长和实验结果。
- 审计 SQL 的业务目的、基表、行粒度、join、过滤、去重、聚合和指标口径。
- 输出 `Data Correctness（数据正确性）`，检查输入数据和指标定义是否可信。
- 输出 `Data Audit Trail（数据审计轨迹）`，检查分析过程和中间结果是否可复算。
- 生成结构化分析报告、老板摘要、5Why（五问法）和 Driver Bridge（驱动项贡献拆解）。
- 根据数据量选择可审计产物：小数据优先 Excel，大数据优先 Jupyter。

## 典型工作流

1. 先读取当前项目的 `_project_memory/`，恢复背景、指标口径和历史决策。
2. 如果涉及分析方法，先读 `references/methodology-index.md`，再只加载必要方法文件。
3. 如果有 SQL，先做 SQL 业务拆解，再进入指标诊断或报告输出。
4. 输出结论前，先检查数据正确性和计算审计轨迹。
5. 任务结束后，按规则更新项目记忆：项目事实进项目记忆，可复用方法进 skill reference。

## 产物规范

业务分析默认包含：

```md
## 结论
## Data Correctness（数据正确性）
## Data Audit Trail（数据审计轨迹）
## Driver Bridge（驱动项贡献拆解）或证据摘要
## 诊断
## 尚未证明
## 下一步检查
```

小数据分析：

- 优先用 Excel。
- 尽量保留公式，不把派生指标只写成静态值。
- 能用透视表、筛选、条件格式或汇总表表达的，保留为可检查结构。
- 建议包含 `README/说明`、`Source/原始数据或输入`、`Calc/计算过程`、`Check/校验`、`Output/结论`。

大数据分析：

- 优先用 Jupyter。
- 所有过程数据必须在 notebook 留痕。
- Markdown 说明、代码注释和检查说明使用中文。
- 每一步写清目的、输入、处理、输出、检查点和风险。
- 保留清洗、过滤、join、去重、聚合、中间行数、分子分母和残差检查。

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

## 使用示例

业务诊断：

```text
使用 project-strategy-analyst。
请分析这份业务指标下滑 case，并输出数据正确性、数据审计轨迹、Driver Bridge、5Why、尚未证明和下一步检查。
```

可审计数据产物：

```text
使用 project-strategy-analyst。
这是一个数据分析任务。请根据数据量选择 Excel 或 Jupyter，并保证所有过程数据可审计。
小数据保留公式/透视表，大数据用 Jupyter 留完整过程和中文注释。
```

SQL 审计：

```text
使用 project-strategy-analyst。
请审计这段 SQL：先解释它想回答的业务问题，再说明基表、行粒度、join、过滤、去重、指标口径、数据审计轨迹和风险。
```

项目记忆：

```text
使用 project-strategy-analyst。
请为这个项目创建或更新 _project_memory，并区分哪些内容应该写入子项目、父项目、全局记忆或 skill reference。
```

## 目录结构

```text
project-strategy-analyst/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── methodology-index.md
    ├── metric-analysis.md
    ├── problem-diagnosis.md
    ├── sql-decomposition-checklist.md
    └── ...
```

## 边界

- 不包含原始笔记、日记、长摘录或私有案例。
- 不内置数据库连接。
- 不保存公司事实或项目事实；这些应进入项目自己的 `_project_memory/`。
- 不把相关性直接写成因果；证据只能支撑到哪一层，就明确停在哪一层。

# project-strategy-analyst

项目策略分析师 skill，用于项目记忆、指标口径、SQL 业务拆解、问题诊断、数据审计轨迹和可审计分析产物。

## 能力范围

- 维护 `_project_memory/` 项目记忆。
- 区分父项目、子项目、全局记忆和 skill reference 的写入边界。
- 拆解指标定义、分母、粒度、过滤、排除项和数据风险。
- 审计 SQL 的业务目的、基表、行粒度、join、过滤、去重、聚合和指标口径。
- 输出 `Data Correctness（数据正确性）` 和 `Data Audit Trail（数据审计轨迹）`。
- 小数据分析优先产出 Excel，并尽量保留公式或透视表。
- 大数据分析优先产出 Jupyter，并保留完整过程数据和中文注释。
- 生成结构化分析报告、老板摘要、5Why 和 Driver Bridge。

## 安装

把本仓库复制或克隆到 Codex skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R project-strategy-analyst ~/.codex/skills/project-strategy-analyst
```

如果仓库根目录本身就是 skill 根目录，可以这样安装：

```bash
mkdir -p ~/.codex/skills/project-strategy-analyst
cp -R . ~/.codex/skills/project-strategy-analyst
```

安装后重启 Codex 或新开会话，让 skill 元信息重新加载。

## 使用示例

```text
使用 project-strategy-analyst。
请分析这份支付收入下滑 case，并输出数据正确性、数据审计轨迹、Driver Bridge、5Why、尚未证明和下一步检查。
```

```text
使用 project-strategy-analyst。
这是一个数据分析任务。请根据数据量选择 Excel 或 Jupyter，并保证所有过程数据可审计。
小数据保留公式/透视表，大数据用 Jupyter 留完整过程和中文注释。
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

- 不包含原始 Logseq 笔记、日记、长摘录或私有案例。
- 不内置数据库连接。
- 不保存公司事实或项目事实；这些应进入项目自己的 `_project_memory/`。

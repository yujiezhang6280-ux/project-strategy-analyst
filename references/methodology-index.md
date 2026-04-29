# 方法论索引

这是分析方法论的唯一默认入口。先读本文件，再按任务只加载匹配的详细方法文件。

## 加载规则

1. 先读本索引。
2. 选择能支撑当前回答的最小方法集合。
3. 默认只加载 1-3 个方法文件。
4. 项目事实、定义、SQL 历史和决策优先从项目记忆读取，不从方法文件推断。
5. 除非用户明确要求笔记蒸馏，不加载原始 Logseq 笔记、图片、PDF 或 highlights。

## 按任务路由

| 用户任务 | 加载文件 | 说明 |
|---|---|---|
| 指标下跌、异常、表现变化、为什么发生 | `problem-diagnosis.md`, `metric-analysis.md` | 先做 5Why（五问法）和指标拆解，再谈因果。 |
| KPI 定义、分母、漏斗、cohort、虚荣指标、领先/滞后指标 | `metric-analysis.md` | 若项目有 `03-metric-logic.md`，先对齐项目口径。 |
| A/B test、实验复盘、因果判断、前后对比 | `experiment-and-causality.md` | 涉及样本、不确定性或实验设计时再加 `statistical-methods.md`。 |
| 显著性、样本量、置信区间、分布、方差、趋势斜率 | `statistical-methods.md` | 统计语言必须绑定业务粒度和数据限制。 |
| 算法、模型、预测、分类、回归、推荐、评估 | `algorithm-and-modeling.md` | 先解释业务用途、验证、泄漏和 failure modes。 |
| 产品策略、增长循环、竞品分析、功能拆解 | `product-growth-methods.md` | 与项目背景和指标文件配合使用。 |
| 报告、PPT、老板汇报、图表叙事、结构化结论 | `structured-thinking-and-reporting.md` | 证据清楚后再用于组织输出。 |
| SQL query、表 join、过滤、行粒度、指标查询审计 | `sql-decomposition-checklist.md` | SQL checklist 优先；需要诊断或报告时再加方法文件。 |
| 监察分析过程、复算中间数、检查 bridge 或 SQL 聚合是否对得上 | `structured-thinking-and-reporting.md`, `metric-analysis.md`, `sql-decomposition-checklist.md` | 区分数据正确性和数据审计轨迹；列输入、中间值、公式、输出和 residual。 |
| 最终分析产物、Excel、Jupyter、过程留痕、公式、透视表、notebook 中文注释 | `structured-thinking-and-reporting.md` | 小数据优先 Excel 留公式/透视表；大数据优先 Jupyter 留完整过程和中文注释。 |
| 阅读笔记、提炼框架、把 Logseq/pages/highlights/images/PDF 转成 skill 内容 | `note-distillation.md` | 只蒸馏规则，不复制原文。 |

## 常见场景默认组合

- 业务诊断：`problem-diagnosis.md` + `metric-analysis.md`
- 实验结论：`experiment-and-causality.md` + 必要时 `statistical-methods.md`
- 定量分析：`statistical-methods.md` + 必要时 `algorithm-and-modeling.md`
- 增长/产品复盘：`product-growth-methods.md` + `metric-analysis.md`
- 最终汇报：`structured-thinking-and-reporting.md` + 分析阶段用到的方法文件
- 笔记到 skill：`note-distillation.md` + 被修改的目标方法文件

## 边界

- 方法文件存可复用分析动作，不存公司事实。
- 公司事实写父项目 `_project_memory/`。
- 子项目事实写子项目 `_project_memory/`。
- 原始笔记必须留在 skill 外，除非被压缩成短规则。
- 证据只能支持相关性时，就说相关性；不要升级成因果。

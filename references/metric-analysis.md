# 指标分析

用于 KPI 定义、漏斗问题、cohort、指标下跌、北极星指标、领先/滞后指标和指标质量检查。

## 先定义指标

解释任何数字前，先明确：

- 分子
- 分母
- 实体粒度：用户、订单、session、event、商户、设备
- 时间粒度和时区
- eligible 条件
- 过滤条件
- 排除项和异常状态
- 来源表或 dashboard
- 指标类型：均值、比率、计数、比例、分位数或分布

如果项目有 `_project_memory/03-metric-logic.md`，先与其对齐再下结论。

## 计算审计

任何依赖数字推导的结论，都要让用户能复算。

默认列出：

- 上期值、本期值、绝对变化、相对变化。
- 比率类指标的分子和分母，而不只给百分比。
- 转化率或成功率变化用百分点 `pp`，不要只写相对百分比。
- AOV、ARPU、收入等乘法关系要列出组成项。
- driver bridge 的每一步输入、公式、输出和贡献。
- residual 检查：用 bridge 重建的本期值是否等于实际本期值。

如果缺少分子、分母、原始 count、单位、周期或公式，标为 `无法复算`，不要把该数字当作已审计证据。

建议输出：

```md
| 指标 | 上期 | 本期 | 绝对变化 | 相对变化/pp | 分子/分母 | 复算状态 |
|---|---:|---:|---:|---:|---|---|
```

```md
| Bridge 步骤 | 输入 | 公式 | 输出 | 对总变化贡献 | residual/备注 |
|---|---|---|---:|---:|---|
```

## 指标质量

一个有用指标应当：

- 绑定具体决策
- 业务变化时足够敏感
- 难以被刷高而不改善真实系统
- 指标 owner 能理解
- 足够稳定，能重复比较

警惕虚荣指标：数字上涨，但不对应留存、收入、转化质量或决策质量。

## 常用视角

- 北极星指标：产品长期优化的价值信号。
- 第一关键指标：最能代表当前阶段目标的短期瓶颈指标。
- Leading metric（领先指标）：业务结果出现前先变化，可触发动作。
- Lagging metric（滞后指标）：事后确认结果。
- Guardrail metric（护栏指标）：防止为了优化一个指标而损害另一个指标。
- Funnel metric（漏斗指标）：拆解步骤转化和流失。
- Cohort metric（同群指标）：比较起点一致的用户。
- Distribution metric（分布指标）：揭示均值掩盖的集中度、大户、尾部风险或异常值。

## 按阶段选指标

按产品阶段和商业模式选指标，不要只选最容易查的 dashboard 指标。

常见阶段：

- 激活：用户到达 first meaningful value moment。
- 参与：用户重复关键行为或加深使用。
- 留存：用户在合理周期内回来获得重复价值。
- 变现：用户支付、支付成功或扩大价值。
- 扩张：存量用户提升用量、席位、消费或下游价值。
- 效率：用更低成本、风险或运营负担创造同样价值。

每个阶段选择：

- 1 个匹配当前瓶颈的主指标
- 1 个防止局部优化的护栏指标
- 1 个指标变化后可执行的动作

行为信号不清楚时，不要过早下变现或扩张结论。

## 拆解顺序

收入或 ARPU：

```text
eligible users（符合资格用户）
-> funnel entry（漏斗入口）
-> conversion rate（转化率）
-> paid user count（付费用户数）
-> payment success rate（支付成功率）
-> order count per payer（人均订单数）
-> average order value / AOV（客单价）
-> refunds/chargebacks（退款/拒付）
-> final revenue（最终收入）
```

结果发生变化时，在链路拆解后加 Driver Bridge（驱动项贡献拆解）。先量化每个 driver 对总变化的贡献，再排序原因。

默认使用 sequential bridge（顺序桥接），除非用户要求其他分解方法：

```text
baseline outcome（基准结果）
-> denominator or eligible population effect（分母/资格人群影响）
-> funnel entry or exposure effect（入口/曝光影响）
-> attempt or conversion effect（尝试/转化影响）
-> success rate effect（成功率影响）
-> order frequency effect（频次影响）
-> average value effect（客单价影响）
-> refund or chargeback effect（退款/拒付影响）
-> current outcome（当前结果）
```

能算时，同时报告每个 driver 的绝对贡献和占总变化比例。Driver contribution 只说明变化发生在哪里，不证明为什么发生。

bridge 输出必须可审计：写清 baseline 公式、每一步替换了哪个变量、替换后的结果和最后 residual。若只能根据用户给的汇总数字估算，明确标为 `基于汇总数估算`。

转化：

```text
eligible population（符合资格人群）
-> exposure（曝光）
-> intent action（意图动作）
-> step-by-step funnel（分步漏斗）
-> success state（成功状态）
-> repeat behavior（重复行为）
```

留存：

```text
cohort entry（同群入口）
-> activation（激活）
-> first value moment（首次价值时刻）
-> repeat use（重复使用）
-> retained period（留存周期）
-> monetization or downstream value（变现或下游价值）
```

## 框架

框架是提示，不是必填章节：

- AARRR：acquisition、activation、retention、referral、revenue。
- HEART：happiness、engagement、adoption、retention、task success。
- OSM：objective、strategy、measure。用于把目标、动作和指标连起来。
- NPS：可用于情绪反馈，但不是完整产品指标体系。

## 常见失败模式

- 用均值代表偏态分布。
- 比较不同渠道或不同成熟度的 cohort。
- 混用事件时间和支付/结算时间。
- 忽略分母变化。
- 把 proxy metric 当成真实业务结果。
- 没核对官方定义就接受 SQL 派生指标。
- 只展示最终百分比，不展示分子、分母和中间值。
- bridge 没有 residual 检查，导致贡献加总和实际变化对不上。

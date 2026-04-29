# 算法与建模

用于预测模型、分类、回归、推荐、排序、聚类、特征设计、评估和算法业务解释。

## 业务框定

先明确：

- 模型支持什么决策
- 预测目标或优化目标
- 预测单位
- 模型输出会触发什么动作
- false positive 和 false negative 的成本
- 需要多高解释性
- 刷新频率和数据可得性

目标和动作不清楚前，不讨论模型选择。

## 数据检查

- label 定义和观察窗口
- 特征在预测时是否可用
- 数据泄漏风险
- train/validation/test split
- 行为随时间变化时使用 temporal split
- segment coverage
- 缺失值
- 类别不平衡
- 异常值影响

## 方法路由

- Regression：估计连续结果，如价值、需求或概率型 score。
- Classification：预测类别，如流失、欺诈、转化或风险。
- Ranking/recommendation：排序 item、用户、内容、offer 或动作。
- Clustering：用于探索分组；未经验证不要把 cluster 当真实用户分层。
- Forecasting：时间结构和季节性是核心时使用；不确定性问题可加 `statistical-methods.md`。

## 评估

评估指标要匹配业务成本：

- accuracy：类别均衡且成本对称时才安全。
- precision：正例预测质量。
- recall：实际正例覆盖。
- F1：precision 和 recall 的平衡。
- AUC/ROC：排序分离能力。
- calibration：预测概率是否可信。
- lift：top-ranked group 的业务增益。
- offline vs online gap：离线验证能否预测线上业务表现。

## 解释

把模型输出翻译成业务语言：

```text
什么决策会改变？
哪些用户/物品会受影响？
什么证据说明模型可泛化？
哪里可能失败？
什么护栏指标防止伤害？
需要什么人工复核或 fallback？
```

## Failure Modes

- 使用未来数据造成泄漏。
- 用 post-treatment behavior 训练。
- 优化 proxy metric 损害真实目标。
- 产品变化后特征过期。
- aggregate performance 掩盖 segment bias。
- 推荐系统强化狭窄历史模式。
- 离线提升没有带来线上业务影响。

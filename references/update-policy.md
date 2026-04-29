# 更新策略

对话新增或改变项目上下文时，使用本策略。

## 优先顺序

1. 保护 `00-critical.md`。
2. 使用 `parents.md` 判断继承上下文，再决定事实属于子项目还是父项目。
3. 用 `99-index.md` 路由上下文加载，再读长文件。
4. 先处理矛盾，再补更多背景。
5. 先更新定义，再写 polished analysis。
6. 只有暂时无法做干净决策时，才追加 raw uncertainty。

## 父子项目写入归属

当前项目有 `_project_memory/parents.md` 时，编辑前先决定归属：

- 写子项目：实验、活动、客户、SQL、时间盒或只影响当前项目分析的事实。
- 写父项目：公司级、跨项目、稳定、应影响多个子项目的事实。
- 两边都写：已有父项目规则，同时子项目有范围化例外或局部实现细节。
- 父项目记忆默认只读；只有用户明确要求更新公司级上下文，或信息明显跨项目共享时才改。
- 子项目定义与父项目定义冲突时，在子项目记录带日期和范围的 override，不要直接改父项目。

## 方法论归属

可复用分析方法写 skill references，不写项目记忆。

- 只有用户明确在改进 skill 或把笔记蒸馏成可复用方法时，才写 `references/*.md`。
- 方法应用到具体项目并形成局部结论、caveat 或决策时，写子项目记忆。
- 结果是公司级定义、共享规则或稳定跨项目决策时，写父项目记忆。
- 不要把原始笔记、截图、PDF、书摘或复制来源文本写进项目记忆或 skill references。
- 不确定是方法还是事实时，方法保留为短规则；事实写入正确项目记忆文件。

## Promote vs Append

提升到 `00-critical.md`，当信息：

- 改变未来分析要优化什么
- 改变重要指标含义
- 推翻常见但错误的假设
- 记录未来对话必须遵守的决策

追加到 `01-background.md`，当信息：

- 增加故事或历史上下文
- 有助于解释但不控制未来决策
- 对定向有用，但不够关键

写入 `02-data-dictionary.md`，当信息：

- 定义表、字段、code value 或 join path
- 澄清 null、重复或数据新鲜度

写入 `03-metric-logic.md`，当信息：

- 改变公式或分母
- 澄清哪些行计入
- 解决 dashboard、SQL 或 stakeholder 语言之间的不一致

写入 `04-sql-notes.md`，当信息：

- 暴露可复用 SQL 结构
- 揭示 query 中隐藏的业务逻辑
- 展示 join、去重或过滤中的重复陷阱

写入 `99-index.md`，当信息：

- 创建值得未来查找的新重大 topic
- 增加无需加载全文件也应发现的指标、表或 SQL 模式
- 改变某类常见问题的最佳加载文件

写入 `parents.md`，当信息：

- 新增、删除或修改继承父项目
- 澄清父项目拥有哪个上下文
- 改变何时加载父项目记忆
- 改变是否可从子项目更新父项目记忆

写入 `98-chat-intake.md`，当信息：

- 来自开场可见对话，可能在压缩后丢失
- 是项目设置但还没干净归入 critical、background、dictionary、metric 或 SQL 文件
- 需要先写短暂存摘要再提升

写入 `97-retrospective.md`，当信息：

- 记录项目做了什么
- 识别有价值发现、决策或失败
- 捕获项目专属 lesson，未来可能提升到全局记忆
- 总结未解决 follow-up

## 压缩规则

不要把所有内容过度压缩进一个 summary 来解决上下文膨胀。

使用非对称压缩：

- `00-critical.md` 保持短而明确。
- `99-index.md` 只做 pointer routing map。
- `01-background.md` 可以积极总结。
- `02-data-dictionary.md` 和 `03-metric-logic.md` 保持结构化，不写叙事。
- `04-sql-notes.md` 保持可复用、query-centric。
- `97-retrospective.md` 保持项目专属，并周期性蒸馏。

## Token Budget 规则

默认分阶段加载：

1. 读 `00-critical.md`
2. 如存在，读 `parents.md`
3. 如存在，读 `99-index.md`
4. 只有父项目上下文相关时，读父项目 `00-critical.md` 和 `99-index.md`
5. 只有恢复项目设置或验证早期假设时，读 `98-chat-intake.md`
6. 只有 review、retrospective、handoff 或全局记忆提取时，读 `97-retrospective.md`
7. 先搜子项目 `_project_memory/`，父项目只搜父项目拥有的 topic
8. 只读取匹配 section 或最小相关文件
9. 只有完整审计或 consolidation 才加载全部记忆文件

除非用户要求完整内容，不要在回答里粘贴或复述大段记忆文件。

## 全局记忆提升规则

项目记忆默认是本地的。只有 lesson 非显而易见、跨项目可复用或可能复发时，才提升到全局长期记忆目录。

- `$CODEX_HOME/memories/LEARNINGS.md`：可复用分析实践、纠正后的假设、持久工作流改进、跨项目业务/数据启发式。
- `$CODEX_HOME/memories/ERRORS.md`：可能复发的意外失败、debug notes、数据陷阱或流程错误。
- `$CODEX_HOME/memories/FEATURE_REQUESTS.md`：用户想要的缺失能力。

不要提升一次性项目事实、敏感细节、raw SQL、临时指标或客户专属背景，除非用户明确要求。

## 冲突规则

新聊天内容与现有项目记忆冲突时：

1. 标记旧说法为 superseded 或 uncertain。
2. 记录新说法及日期或来源上下文。
3. 除非新证据明确权威，否则不要静默删除矛盾。

子项目记忆与父项目记忆冲突时：

1. 在该子项目内使用子项目记忆。
2. 如果影响回答，提示冲突。
3. 更新子项目 override；修改父项目记忆前先确认。

## 输出规则

回答用户时：

- 明确暴露关键假设。
- 不要把 tentative background 说成 confirmed fact。
- 未对账前，不要把 SQL 派生逻辑说成官方指标逻辑。

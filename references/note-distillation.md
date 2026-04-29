# 笔记蒸馏

用于把笔记页面、日记、highlights、图片、PDF 或其他材料转成此 skill 可复用的方法论。

## 目标

提取可复用分析方法。不要总结整套笔记库，不要把来源材料复制进 skill。

## 来源优先级

1. 标题或链接明显是方法论的笔记页面。
2. 指向分析、产品、增长、统计、算法或报告材料的 highlights。
3. 日记或复盘中明确的分析规则、反复纠错或稳定方法。
4. 图片或 PDF：只有相关 markdown 页面说明它为什么重要时才处理。
5. Office 文件和视频：只有用户明确要求，或高价值页面指向具体 section 时才处理。

默认跳过备份目录、回收站、临时上传和未引用大附件，除非用户明确要求。

## 候选方法卡

每个候选方法输出：

```text
method_name:
source_refs:
use_when:
core_steps:
input_requirements:
output_shape:
common_misuse:
project_fact_or_general_method:
recommended_target:
confidence:
```

## 归口判断

- Skill reference：可复用方法、框架、检查清单、分析输出结构。
- Parent project memory：公司级事实、共享指标定义、通用数据字典、稳定战略规则。
- Child project memory：项目事实、实验结论、SQL note、局部 caveat。
- Global memory：持久用户偏好、反复纠错、跨项目工作规则。
- Skip：原文摘录、一次性日记细节、无证据主张、低上下文碎片。

## 蒸馏规则

- 来源路径只保留在项目 workbench 或 review outputs 中用于追溯。
- 除非用户明确要求 skill 内 provenance，不要把来源路径、页面标题、书名、时间戳或案例标识写入最终 skill reference。
- 提取短规则和短例子，不复制长 prose。
- 重复内容合并到已有方法文件，不新增文件。
- taxonomy 不清楚时，先写短候选并询问，不直接改 skill。
- 不要让笔记分类覆盖 `methodology-index.md` 的任务路由，除非重复案例证明路由错误。

## 防污染过滤

只有当笔记能变成可复用方法、框架、检查清单或输出模式时，才提升到 skill。

跳过或路由到其他地方：

- 公司事实、共享指标、表定义、战略上下文：父项目记忆。
- 项目事实、实验结论、SQL 输出、局部 caveat：子项目记忆。
- 个人偏好或反复纠错：全局记忆。
- 原文摘录、来源案例、截图、PDF 文本、日记正文、长引用：跳过。

技术或算法笔记进入 skill 前，必须包含：

- 业务使用场景
- 假设和 failure modes
- 评估标准
- 决策后果

立项、PPT、case 材料只能贡献可复用决策结构。移除名称、截图、日期、案例事实、来源指标和资产细节。

## 更新此 skill 时

1. 读 `methodology-index.md`。
2. 读目标方法文件。
3. 只加或改最小有用规则。
4. 保持方法文件简洁。
5. 不在最终 skill patch 中包含来源路径或来源措辞。
6. 修改后做 skill validation。

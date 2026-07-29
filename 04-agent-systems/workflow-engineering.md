# Workflow Engineering

## 通俗解释

Prompt 像给员工一句要求，Workflow 像给员工一套 SOP。

```text
Prompt：帮我做市场分析

Workflow：
1. 搜集竞品
2. 提取功能
3. 形成对比
4. 输出结论
```

Workflow Engineering 是把复杂任务拆成可观察、可验证、可替换的固定步骤。

## 为什么出现

单次模型调用适合一次性任务，但无法稳定完成多步骤、需要检查中间结果的任务。

```text
用户需求
→ 生成方案
→ 执行
→ 检查
→ 输出
```

当流程已知时，把它显式固化可以提高稳定性、可诊断性和可评估性。

## 系统模型

从系统视角看，Workflow 是预定义的状态流转：

```text
Input
→ State A
→ Step 1
→ State B
→ Step 2
→ State C
→ Output
```

例如典型 RAG：

```text
问题
→ Query Rewrite
→ 检索
→ 重排
→ 生成
→ 答案
```

## 生产设计原则

### 先定义输入输出

先明确最终交付物，再倒推步骤。输入不清晰、输出不可验证的 Workflow 很难稳定。

### 每一步只承担一个可诊断职责

```text
retrieve()
rerank()
generate()
```

比一个 `do_everything()` 更容易定位问题、替换能力和单独评测。

### 明确定义失败路径

常见策略：

- Retry：临时失败后重试。
- Fallback：切换模型、数据源或降级方案。
- Human in the Loop：高风险或不确定场景转人工。
- Abort：权限不足或关键条件不满足时终止。

## 核心共识

- Prompt 不足以支撑复杂生产任务。
- Workflow 是许多 AI 应用的最小稳定生产单元。
- Workflow 的价值不只是自动化，更重要的是可观察和可诊断。
- 确定性部分优先固化为 Workflow。

## 核心 Trade-off

### 确定性 vs 灵活性

Workflow 更稳定、可预测；Agent 更能处理未知情况。

### 开发成本 vs 运行成本

Workflow 需要提前设计和维护流程，但运行更便宜；Agent 前期编排较少，但运行时推理成本更高。

### 可控性 vs 泛化能力

流程越固定，越容易评测和运营，但覆盖未知场景的能力越弱。

## 适用判断

如果一个任务同时满足：

- 下一步基本明确；
- 失败后的处理方式明确；
- 执行中产生的新信息较少；
- 分支数量可控；

通常 Workflow 是优先选择。

## 典型误区

- Agent 一定比 Workflow 高级。
- Workflow 只是把多个 Prompt 串起来。
- 复杂产品应该尽量 Agent 化。
- 成功流程设计好就够了，失败逻辑可以后补。

## 关键问题

> RAG 是 Agent 吗？

不能只回答“是”或“不是”。需要判断它是否有动态决策、循环、状态和自主规划。大多数固定的“检索 → 重排 → 生成”系统，本质上是 Workflow。

## 工程与产品启发

成熟系统通常不是纯 Workflow 或纯 Agent，而是：

```text
Workflow 管确定性部分
Agent 管不确定性部分
```

对于 Agent，Workflow 可以被封装成可复用的“高阶 Tool”。

# Agent Orchestration

## 通俗解释

Agent 编排不是让多个 Agent 自由聊天，而是让一个编排层根据任务、上下文、工具和风险，选择最小且可控的执行路径。

## 系统角色

### Orchestrator / 主 Agent

负责：

- 理解任务；
- 选择 Agent、Workflow 或 Tool；
- 组装必要 Context；
- 维护任务 State；
- 整合结果；
- 处理失败和风险。

### 业务 Agent

负责特定领域判断，例如月付、保险、信贷等。

### Workflow

负责已经标准化、可预测的确定性流程。

### Tool

负责原子能力，例如查询额度、查询保单、调用业务接口。

### Memory

负责跨任务长期信息，不替代实时业务状态。

## Router 应输出什么

最简单的 Router 只输出目标 Agent：

```text
route = 月付 Agent
```

更成熟的路由结果可以是任务描述：

```text
意图：月付开通失败原因
目标 Agent：月付 Agent
所需 Context：用户实时状态、准入规则、风控标签
所需 Tool：额度查询、失败原因查询
风险等级：高
输出限制：不能编造或承诺开通结果
```

这时 Router 已经接近轻量 Orchestrator。

## 三种执行模式

### 单 Agent

适合明确、单领域问题。默认优先选择，成本最低、归因最简单。

### Agent + Workflow

适合流程明确但需要模型解释或判断的任务。

```text
查用户状态
→ 查业务规则
→ 匹配失败原因
→ 生成合规解释
```

### 多 Agent

只在问题真正跨领域、单 Agent 无法独立完成时使用。各 Agent 应有清晰职责和 Context 边界，由主 Agent 汇总。

## 生产设计步骤

1. 定义每个 Agent / Workflow 的输入、输出和不能处理的边界。
2. 定义 Router 的意图体系、置信度和追问策略。
3. 定义不同路径需要的 Context 与 Tool。
4. 选择最小执行模式：单 Agent、Workflow 或多 Agent。
5. 定义失败、冲突、低置信度和人工介入路径。
6. 建立可观测日志和 Bad Case 归因体系。

## 金融场景兜底

- Router 低置信度：追问用户，不强行路由。
- Tool 查询失败：明确无法查询，不编造实时结果。
- 业务 Agent 越权：主 Agent 或规则层拦截。
- 多 Agent 结论冲突：保守输出、重新核验或转人工。
- 高风险规则解释：引用当前有效规则，避免承诺。

## 核心 Trade-off

### 能力覆盖 vs 系统复杂度

Agent 越多、路径越灵活，覆盖面越广，但错误归因和维护成本越高。

### 中央编排 vs 业务自治

中央层统一管理风险和体验，但可能成为瓶颈；业务 Agent 自治更灵活，但容易产生标准不一致。

### 一次路由 vs 动态重路由

一次路由便宜、稳定；动态重路由能处理任务变化，但增加循环和状态复杂度。

## 典型误区

- 多 Agent 一定比单 Agent 更强。
- Agent 编排就是把多个 Agent 串联起来。
- Router 只需返回 Agent 名称。
- Workflow 是低级 Agent。
- Memory 可以代替实时 Tool 查询。

## 关键原则

```text
确定性部分 → Workflow
不确定性决策 → Agent
原子外部动作 → Tool
跨任务长期信息 → Memory
完整执行生命周期 → Runtime
```

## 工程与产品启发

两个 Agent PM 共同负责 Router、Context、Memory 和整体质量时，角色更接近 **Agent System PM**，需要按“需要设计 / 需要理解 / 暂时知道存在”划分学习深度，而不是简单区分产品与技术。

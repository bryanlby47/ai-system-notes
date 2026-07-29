# Agent Runtime

## 通俗解释

如果 LLM 是一个聪明的员工，Agent Runtime 就是它的工作台和项目秘书。

工作台上放着：

- 当前目标与进度；
- 任务清单；
- 可用工具；
- 工作流程；
- 历史记录；
- 权限与停止条件；
- 失败后的恢复信息。

模型每次工作前读取工作台，工作后更新工作台，才能连续完成长任务。

## 实体是什么

Agent Runtime 不是一个抽象名词，而是正在运行的软件系统。

它可能是：

- 本地 Python / Node.js 进程；
- IDE 内的后台进程；
- 云端服务或 Worker；
- 多个服务、数据库和队列共同组成的系统。

最小循环可以抽象为：

```text
加载 State
→ 组装 Context
→ 调用模型
→ 解析 Action
→ 执行 Tool / Workflow
→ 保存新 State
→ 继续或停止
```

## Runtime 管什么

- Router：把任务交给谁。
- Context Builder：模型本轮看到什么。
- State Manager：当前任务做到哪里。
- Memory：读取或写入长期信息。
- Tool Manager：调用哪些外部能力。
- Workflow Executor：执行确定性流程。
- Loop：根据反馈持续推进。
- Permission：哪些动作允许自动执行。
- Checkpoint：中断后从哪里恢复。
- Logs / Evaluation：发生了什么、效果如何。

## Runtime、Router 与 Orchestrator

Router 只是 Runtime 的一个模块：

```text
Router：选择执行路径
Runtime：管理完整执行生命周期
```

Orchestrator 更强调跨 Agent、Workflow 和 Tool 的协调决策。不同公司会把 Orchestrator 算作 Runtime 的一部分，也可能单独分层；判断时应看实际职责，不应只看名字。

## Runtime 与 Harness

```text
Runtime = 决策、调度与生命周期管理
Harness = 受控执行环境
```

在 Coding Agent 中，Harness 可能提供：

- 文件系统；
- 终端；
- Git；
- 测试环境；
- 浏览器；
- 沙箱。

Runtime 决定“下一步需要运行测试”，Harness 负责真正执行测试并返回结果。

金融问答 Agent 未必需要独立 Harness；只有当 Agent 需要实际操作后台、填写工单或执行复杂外部动作时，执行环境才会成为核心。

## 核心共识

- 长任务需要独立于模型的运行管理层。
- State、Context 和 Tool 生命周期是 Runtime 的核心。
- 模型升级不会自动消除权限、恢复、调度和状态管理问题。
- Runtime 的价值在复杂、长周期、有外部动作的任务中最明显。

## 核心 Trade-off

### 自由度 vs 可控性

开放更多自主决策可以覆盖未知场景，但会增加风险和调试难度。

### 自动化 vs 可解释性

越多决策由模型动态完成，越难准确复现和归因。

### 状态完整性 vs 上下文成本

Runtime 可以保存大量状态，但模型每轮只应看到当前推理真正依赖的部分。

## Agent PM 需要掌握到什么程度

需要设计：

- Router、Context、Memory、Workflow、Tool 的行为；
- State 的业务 Schema 和更新规则；
- 风险边界、失败路径和人工介入点；
- 质量指标与 Bad Case 归因。

暂时只需理解、不必深入实现：

- Redis、消息队列和分布式一致性；
- Scheduler 和执行引擎细节；
- Checkpoint 的底层存储机制。

## 典型误区

- Runtime 等于 Router。
- Runtime 等于某个 Agent Framework。
- Agent 只是模型加几个 Tool Call。
- 模型变强后 Runtime 会消失。

## 工程与产品启发

判断一个问题是否属于 Runtime，可以问：

> 它是在定义业务上“做什么”，还是在管理这件事“如何持续、安全地运行”？

Agent System PM 通常既要定义业务决策，也需要设计 Runtime 能力如何被使用，但不一定负责底层技术实现。

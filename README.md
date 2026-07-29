# AI System Notes

面向 AI Product Builder / Agent System PM 的结构化知识库。

目标不是收藏术语，而是建立从概念到产品决策的完整认知链路：

```text
概念 → 系统 → 工程 → 产品
```

## 学习方式

每个主题尽量回答：

1. 通俗解释
2. 为什么出现
3. 系统模型
4. 核心共识
5. 核心 Trade-off
6. 典型误区
7. 关键问题
8. 技术演进位置
9. 工程与产品启发

对于工程主题，优先讨论约束与 Trade-off；只有存在真实范式冲突时，才单独整理“核心争议”。

## 学习地图

### 01 Foundations

- 数据结构
- 状态与状态变化
- 存储、索引、缓存

### 02 Software Engineering

- [Git Object Model](02-software-engineering/git-object-model.md)

### 03 AI Systems

- Transformer
- Embedding
- RAG
- MCP / Function Calling

### 04 Agent Systems

- [Workflow Engineering](04-agent-systems/workflow-engineering.md)
- [Loop Engineering](04-agent-systems/loop-engineering.md)
- [State](04-agent-systems/state.md)
- [Agent Runtime](04-agent-systems/agent-runtime.md)
- [Context Engineering](04-agent-systems/context-engineering.md)
- [Agent Orchestration](04-agent-systems/agent-orchestration.md)
- Memory
- Router
- Evaluation / Quality System

### 05 AI Products

- AI Coding
- AI Search
- Agent Product Design

### 06 Projects

- 实际 Agent 产品方案与复盘

## 当前工作主线

默认优先学习与 Agent 系统产品工作直接相关的内容：

```text
Context Engineering
→ Router
→ Memory
→ Evaluation
→ Quality System
→ Workflow
→ Multi-Agent
→ Tool / Skill
```

Runtime、State、Checkpoint 等基础设施内容作为背景知识；除非实际工作需要，否则暂不深入到底层实现。

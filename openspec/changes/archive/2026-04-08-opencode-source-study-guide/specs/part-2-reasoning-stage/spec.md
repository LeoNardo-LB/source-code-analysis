## ADDED Requirements

### Requirement: Agent 系统与 Prompt 构建文档
系统 SHALL 提供 `docs/part-2-推理阶段/03-Agent系统与Prompt构建.md`，包含：
- Agent 类型对比表（build / plan / general 三种 Agent 的职责与差异）
- Agent 执行循环的 Mermaid 流程图（接收消息 → 构建 Prompt → 调用 LLM → 解析响应 → Tool 调用 → 循环/终止）
- Prompt 构建过程的伪代码（系统提示 + 上下文消息 + 用户消息的组装逻辑）
- Sub-agent 委派机制的 Mermaid 时序图
- 源码锚点表：`packages/opencode/src/agent/`

#### Scenario: 理解 Agent 如何思考
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过流程图理解 Agent 的完整推理循环，通过伪代码理解 Prompt 是如何组装的

### Requirement: Provider 适配与 API 调用文档
系统 SHALL 提供 `docs/part-2-推理阶段/04-Provider适配与API调用.md`，包含：
- Provider 抽象层的 Mermaid 类图（统一接口 → 20+ 具体实现）
- Provider 注册与选择机制的伪代码
- API 调用流程的 Mermaid 时序图（Provider 选择 → 参数转换 → HTTP 请求 → 响应解析 → 统一格式）
- 流式响应（SSE）处理逻辑的伪代码
- Vercel AI SDK 集成方式说明
- 源码锚点表：`packages/opencode/src/provider/`

#### Scenario: 理解多 Provider 如何统一适配
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过类图理解 Provider 抽象设计，通过时序图理解从 Agent 到 LLM API 的完整调用链路

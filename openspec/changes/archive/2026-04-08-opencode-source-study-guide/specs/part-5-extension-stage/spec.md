## ADDED Requirements

### Requirement: 插件系统与二次开发文档
系统 SHALL 提供 `docs/part-5-扩展阶段/10-插件系统与二次开发.md`，包含：
- 插件架构的 Mermaid 组件图（Plugin SDK → manifest → Hook/Tool/Provider/Channel 注册）
- 插件生命周期流程的 Mermaid 时序图（加载 → 解析 manifest → 注册能力 → 运行时调用）
- 插件 manifest（openclaw.plugin.json）结构的字段说明表
- 插件开发最小示例的伪代码
- 源码锚点表：`packages/opencode/src/plugin/`、`packages/plugin/`

#### Scenario: 理解插件系统如何工作
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过组件图理解插件架构，通过时序图理解插件生命周期，并能通过伪代码示例快速上手插件开发

### Requirement: MCP 协议集成文档
系统 SHALL 提供 `docs/part-5-扩展阶段/11-MCP协议集成.md`，包含：
- MCP 协议概述（Model Context Protocol 的定位与价值）
- MCP 集成架构的 Mermaid 架构图（OpenCode ↔ MCP Client ↔ MCP Server）
- MCP Tool 发现与调用的 Mermaid 时序图
- MCP Server 配置方式的伪代码
- 源码锚点表：`packages/opencode/src/mcp/`

#### Scenario: 理解 MCP 如何扩展 Agent 能力
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过架构图理解 MCP 集成方式，通过时序图理解 Tool 发现和调用流程

### Requirement: 自定义 Provider 开发文档
系统 SHALL 提供 `docs/part-5-扩展阶段/12-自定义Provider开发.md`，包含：
- Provider 接口设计的 Mermaid 类图（核心接口与默认实现）
- 添加新 Provider 的步骤流程图
- 自定义 Provider 实现的伪代码（实现统一接口 → 注册 → 配置）
- 源码锚点表：`packages/opencode/src/provider/`

#### Scenario: 理解如何添加新的 AI Provider
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过类图理解 Provider 接口设计，通过伪代码和步骤图理解添加新 Provider 的完整过程

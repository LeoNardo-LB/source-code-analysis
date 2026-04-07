## ADDED Requirements

### Requirement: 响应渲染与 TUI 界面文档
系统 SHALL 提供 `docs/part-4-输出阶段/08-响应渲染与TUI界面.md`，包含：
- TUI 架构的 Mermaid 组件图（opentui 框架 → SolidJS 组件 → 终端渲染）
- 消息渲染流程的 Mermaid 时序图（Markdown 解析 → 代码高亮 → 终端绘制）
- Web UI 与 TUI 的共享/差异对比表
- 流式输出（Token 级别实时渲染）的伪代码
- 源码锚点表：`packages/opencode/src/cli/cmd/tui/`、`packages/app/`

#### Scenario: 理解 TUI 如何渲染 AI 响应
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过组件图理解 TUI 架构，通过时序图理解从 LLM 流式响应到终端绘制的完整流程

### Requirement: 权限控制与安全模型文档
系统 SHALL 提供 `docs/part-4-输出阶段/09-权限控制与安全模型.md`，包含：
- 权限检查流程的 Mermaid 流程图（Tool 调用 → 权限判断 → 用户确认/自动批准/拒绝）
- 权限规则的配置与匹配伪代码
- 敏感操作分级表（读文件 vs 写文件 vs 执行 Shell 的权限级别）
- 源码锚点表：`packages/opencode/src/permission/`

#### Scenario: 理解权限如何保护系统安全
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过流程图理解权限检查的完整过程，通过分级表理解不同操作的权限差异

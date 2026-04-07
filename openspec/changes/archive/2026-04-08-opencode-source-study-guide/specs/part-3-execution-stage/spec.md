## ADDED Requirements

### Requirement: Tool 系统详解文档
系统 SHALL 提供 `docs/part-3-执行阶段/05-Tool系统详解.md`，包含：
- Tool 注册与发现机制的 Mermaid 流程图
- 内置 Tool 清单表（Bash、Read、Write、Edit、Grep、Glob 等，含每个 Tool 的输入/输出 schema）
- Tool 调用流程的 Mermaid 时序图（Agent 决策 → 参数校验 → 执行 → 结果返回）
- 自定义 Tool 的伪代码示例
- Zod schema 校验在 Tool 参数中的作用说明
- 源码锚点表：`packages/opencode/src/tool/`

#### Scenario: 理解 Tool 系统如何工作
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过流程图理解 Tool 注册机制，通过时序图理解完整调用链路，并能通过伪代码示例理解如何开发自定义 Tool

### Requirement: 文件操作与 Shell 执行文档
系统 SHALL 提供 `docs/part-3-执行阶段/06-文件操作与Shell执行.md`，包含：
- 文件读写操作的 Mermaid 流程图（虚拟文件系统 → 权限检查 → 实际 IO）
- Shell 命令执行的 Mermaid 时序图（命令解析 → PTY 创建 → 执行 → 输出捕获 → 超时处理）
- node-pty / bun-pty 的集成方式说明
- 沙箱与安全隔离机制的伪代码
- 源码锚点表：`packages/opencode/src/file/`、`packages/opencode/src/shell/`、`packages/opencode/src/pty/`

#### Scenario: 理解文件和 Shell 操作的安全机制
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过流程图和时序图理解文件操作和 Shell 执行的完整流程，包括权限检查和安全隔离

### Requirement: LSP 集成文档
系统 SHALL 提供 `docs/part-3-执行阶段/07-LSP集成.md`，包含：
- LSP 协议概述（对 Java 开发者友好，类比 Java Language Server）
- LSP 集成架构的 Mermaid 架构图（OpenCode ↔ LSP Client ↔ Language Server）
- 代码补全、诊断、跳转定义等功能的 Mermaid 时序图
- Tree-sitter 作为轻量级替代的说明
- 源码锚点表：`packages/opencode/src/lsp/`

#### Scenario: 理解 LSP 如何为 Agent 提供代码智能
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过架构图理解 LSP 集成方式，通过时序图理解具体功能（补全、诊断等）的交互过程

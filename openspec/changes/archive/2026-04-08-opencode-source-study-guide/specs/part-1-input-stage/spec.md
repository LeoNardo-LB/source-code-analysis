## ADDED Requirements

### Requirement: CLI 入口与启动流程文档
系统 SHALL 提供 `docs/part-1-输入阶段/01-CLI入口与启动流程.md`，包含：
- CLI 入口文件的 Mermaid 调用链图（从 bin 文件到 main 函数）
- 启动模式说明（TUI 模式、Headless Server 模式、Web 模式）
- 参数解析流程的伪代码（yargs 配置 → 命令路由 → 模式选择）
- Server 启动流程的 Mermaid 时序图（Hono 初始化 → 端口监听 → WebSocket）
- 源码锚点表：`packages/opencode/src/cli/`、`packages/opencode/src/server/`

#### Scenario: 理解 CLI 启动流程
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过 Mermaid 调用链图和时序图理解从命令行输入到 Server 就绪的完整流程，无需查看源码

### Requirement: Session 与上下文构建文档
系统 SHALL 提供 `docs/part-1-输入阶段/02-Session与上下文构建.md`，包含：
- Session 数据模型的 Mermaid ER 图（Session → Message → ToolCall 等关系）
- 上下文构建流程的 Mermaid 流程图（项目扫描 → 文件索引 → 系统提示组装）
- 消息历史管理与截断策略的伪代码
- Drizzle ORM + SQLite 存储方案说明
- 源码锚点表：`packages/opencode/src/session/`、`packages/opencode/src/storage/`

#### Scenario: 理解 Session 如何构建上下文
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过 ER 图理解数据模型，通过流程图理解上下文构建过程

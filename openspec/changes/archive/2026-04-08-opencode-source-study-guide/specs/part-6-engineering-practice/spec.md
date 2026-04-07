## ADDED Requirements

### Requirement: Effect 框架实战指南文档
系统 SHALL 提供 `docs/part-6-工程实践/13-Effect框架实战指南.md`，包含：
- Effect 核心概念速查表（Effect/T/Service/Layer/Program，每个概念一句话解释）
- Effect vs RxJava 对比表（关键概念映射）
- Effect 在 OpenCode 中的使用模式的 Mermaid 流程图（Service 定义 → Layer 组装 → Program 执行）
- 错误处理、依赖注入、并发控制的伪代码示例
- 源码锚点表：`packages/opencode/src/effect/`、项目中 Effect 的典型使用位置

#### Scenario: Java 开发者理解 Effect 框架
- **WHEN** Java 开发者阅读该文档
- **THEN** SHALL 通过对比表快速建立 Effect 与 RxJava 的认知映射，通过伪代码理解 Effect 的核心使用模式

### Requirement: 构建系统与 Monorepo 文档
系统 SHALL 提供 `docs/part-6-工程实践/14-构建系统与Monorepo.md`，包含：
- Monorepo 结构的 Mermaid 依赖图（18 个子包的依赖关系）
- Turborepo 任务编排流程的 Mermaid 流程图
- Bun workspace 配置与脚本管理说明
- 构建缓存与增量构建策略
- 源码锚点表：`turbo.json`、`package.json`、`packages/*/package.json`

#### Scenario: 理解 Monorepo 构建系统
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过依赖图理解子包关系，通过流程图理解 Turborepo 的任务编排方式

### Requirement: 存储与数据模型文档
系统 SHALL 提供 `docs/part-6-工程实践/15-存储与数据模型.md`，包含：
- 存储层架构的 Mermaid 架构图（Drizzle ORM → SQLite → 文件系统缓存）
- 核心数据模型的 Mermaid ER 图（Session、Message、ToolCall、File 等）
- 数据迁移策略说明
- 源码锚点表：`packages/opencode/src/storage/`、`packages/opencode/src/db/`

#### Scenario: 理解数据如何持久化
- **WHEN** 读者阅读该文档
- **THEN** SHALL 通过架构图理解存储层设计，通过 ER 图理解核心数据模型

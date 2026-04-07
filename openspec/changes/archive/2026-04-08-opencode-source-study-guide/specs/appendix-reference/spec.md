## ADDED Requirements

### Requirement: Java 开发者 TS 速查附录
系统 SHALL 提供 `docs/appendix/A-Java开发者TS速查.md`，包含：
- TypeScript 基础语法与 Java 的对比表（类型系统、接口、泛型、装饰器等）
- 现代 TS 特性速查（可选链、空值合并、模板字面量类型等）
- Bun 运行时 vs Node.js vs JVM 的对比表
- 常见 TS 项目配置说明（tsconfig.json 关键字段）

#### Scenario: Java 开发者快速查阅 TS 语法
- **WHEN** Java 开发者在阅读源码文档时遇到不熟悉的 TS 语法
- **THEN** SHALL 能通过该附录快速找到对应 Java 概念的类比和解释

### Requirement: 设计模式总结附录
系统 SHALL 提供 `docs/appendix/B-设计模式总结.md`，包含：
- OpenCode 中使用的设计模式清单表（模式名称 → 使用位置 → 简要说明）
- 每个模式的 Mermaid 类图/流程图
- 与 Java/Gang of Four 模式的对比

#### Scenario: 查阅 OpenCode 使用的设计模式
- **WHEN** 读者想了解 OpenCode 的架构设计思想
- **THEN** SHALL 能通过模式清单表快速定位感兴趣的模式，并通过图表理解其实现

### Requirement: 核心 API 参考索引附录
系统 SHALL 提供 `docs/appendix/C-核心API参考索引.md`，包含：
- 核心模块的 API 速查表（模块名 → 主要导出 → 用途）
- HTTP API 端点列表（Server 暴露的 REST/WebSocket 端点）
- 配置文件字段索引（opencode.json 所有可配置项）

#### Scenario: 快速查找 API 或配置项
- **WHEN** 读者在阅读文档时需要查阅某个 API 的签名或配置项的含义
- **THEN** SHALL 能通过该附录快速定位到相关信息

## Why

OpenCode 是一个 139k Star 的开源 AI 编码代理项目，采用 TypeScript + Bun 运行时构建，其架构设计（C/S 架构、多 Provider 适配、Agent 系统、插件体系）对学习现代 AI 工程化实践具有极高的参考价值。当前缺少一份面向 Java 开发者的、由浅入深、自上而下的源码学习指南，需要创建一套移动端友好的 Markdown + Mermaid 图表教学文档体系，使读者在不看源码的前提下也能理解每个模块的运转机制。

## What Changes

- 创建 `opencode/` 目录，拉取 opencode 最新 tag 版本源码作为研究对象
- 创建 `docs/` 目录，存放全部教学文档，与源码目录分离
- 编写 15 篇核心教学文档 + 3 篇附录，按"消息请求生命周期"为主线组织（输入 → 推理 → 执行 → 输出 → 扩展 → 工程实践）
- 每篇文档采用统一模板：架构位置图（Mermaid）→ 问题定义 → 运转流程图（Mermaid）→ 核心伪代码 → 设计决策 → 源码锚点（可选深入）
- 所有文档为纯 Markdown + Mermaid 图表，确保 GitHub 直接渲染、手机可读
- 创建 `docs/README.md` 作为全局路线图与导航索引

## Capabilities

### New Capabilities

- `study-route-map`: 全局源码学习路线图与架构总览文档（docs/README.md + 00 篇），包含整体架构图、学习路径建议、文档索引
- `part-1-input-stage`: 输入阶段文档（01-02 篇），覆盖 CLI 入口启动流程、Session 与上下文构建机制
- `part-2-reasoning-stage`: 推理阶段文档（03-04 篇），覆盖 Agent 系统（build/plan/general）与 Prompt 构建、Provider 适配层与 LLM API 调用
- `part-3-execution-stage`: 执行阶段文档（05-07 篇），覆盖 Tool 系统详解、文件操作与 Shell 执行、LSP 集成
- `part-4-output-stage`: 输出阶段文档（08-09 篇），覆盖 TUI 响应渲染、权限控制与安全模型
- `part-5-extension-stage`: 扩展阶段文档（10-12 篇），覆盖插件系统与二次开发、MCP 协议集成、自定义 Provider 开发
- `part-6-engineering-practice`: 工程实践文档（13-15 篇），覆盖 Effect 框架实战指南、构建系统与 Monorepo、存储与数据模型
- `appendix-reference`: 附录文档（A/B/C 篇），覆盖 Java 开发者 TS 速查、设计模式总结、核心 API 参考索引

### Modified Capabilities

（无已有 capabilities 需要修改）

## Impact

- **目录结构**: 项目新增 `opencode/`（源码）和 `docs/`（文档）两个顶级目录
- **依赖**: 需要网络访问拉取 GitHub 仓库（sst/opencode）
- **目标读者**: 有 Java 后端经验、学过 JS/TS 的开发者
- **阅读平台**: GitHub 仓库直接浏览，手机端友好
- **维护策略**: 文档基于最新 tag 版本，后续可按需更新

## 1. 项目初始化

- [x] 1.1 确认 test-driven-development、dispatching-parallel-agents 技能已加载（未加载则加载，已加载则跳过）
- [x] 1.2 拉取 opencode 最新 tag 源码到 `opencode/` 目录（`git clone --depth 1 --branch $(git ls-remote --tags https://github.com/sst/opencode | grep -oP 'refs/tags/v\K[0-9]+\.[0-9]+\.[0-9]+$' | sort -V | tail -1) https://github.com/sst/opencode opencode`）
- [x] 1.3 创建 `docs/` 目录结构：`part-1-输入阶段/`、`part-2-推理阶段/`、`part-3-执行阶段/`、`part-4-输出阶段/`、`part-5-扩展阶段/`、`part-6-工程实践/`、`appendix/`

## 2. 全局路线图与架构总览（Chunk 1 — 骨架）

- [x] 2.1 编写 `docs/README.md`：项目描述、技术栈概览表、Mermaid 整体架构图、学习路径建议、15+3 文档索引
- [x] 2.2 编写 `docs/00-源码学习路线图与架构总览.md`：C/S 架构详解、Monorepo 包依赖 Mermaid 图、40+ 模块职责总表、技术栈 vs Java 生态类比速查表、消息完整旅程 Mermaid 时序图
- [x] 2.3 验证：在 GitHub 预览 README.md 和 00 篇，确认所有 Mermaid 图表渲染正常、链接跳转正确、手机端可读

## 3. 输入阶段文档（Chunk 2）

- [x] 3.1 编写 `docs/part-1-输入阶段/01-CLI入口与启动流程.md`：CLI 入口 Mermaid 调用链图、三种启动模式说明、参数解析伪代码、Server 启动 Mermaid 时序图、源码锚点表
- [x] 3.2 编写 `docs/part-1-输入阶段/02-Session与上下文构建.md`：Session 数据模型 Mermaid ER 图、上下文构建 Mermaid 流程图、消息历史管理伪代码、Drizzle ORM 存储说明、源码锚点表
- [x] 3.3 验证：确认两篇文档 Mermaid 图渲染正常，伪代码逻辑与 `opencode/` 源码对应

## 4. 推理阶段文档（Chunk 3）

- [x] 4.1 编写 `docs/part-2-推理阶段/03-Agent系统与Prompt构建.md`：Agent 类型对比表、Agent 执行循环 Mermaid 流程图、Prompt 构建伪代码、Sub-agent 委派 Mermaid 时序图、源码锚点表
- [x] 4.2 编写 `docs/part-2-推理阶段/04-Provider适配与API调用.md`：Provider 抽象层 Mermaid 类图、注册与选择伪代码、API 调用 Mermaid 时序图、SSE 流式响应伪代码、Vercel AI SDK 集成说明、源码锚点表
- [x] 4.3 验证：确认两篇文档 Mermaid 图渲染正常，Agent 循环和 Provider 调用链的伪代码与源码逻辑一致

## 5. 执行阶段文档（Chunk 4）

- [x] 5.1 编写 `docs/part-3-执行阶段/05-Tool系统详解.md`：Tool 注册 Mermaid 流程图、内置 Tool 清单表、Tool 调用 Mermaid 时序图、自定义 Tool 伪代码示例、Zod schema 说明、源码锚点表
- [x] 5.2 编写 `docs/part-3-执行阶段/06-文件操作与Shell执行.md`：文件读写 Mermaid 流程图、Shell 执行 Mermaid 时序图、PTY 集成说明、沙箱安全伪代码、源码锚点表
- [x] 5.3 编写 `docs/part-3-执行阶段/07-LSP集成.md`：LSP 协议概述、LSP 集成 Mermaid 架构图、代码智能功能 Mermaid 时序图、Tree-sitter 说明、源码锚点表
- [x] 5.4 验证：确认三篇文档 Mermaid 图渲染正常，Tool/Shell/LSP 的伪代码流程与源码实现对应

## 6. 输出阶段文档（Chunk 5）

- [x] 6.1 编写 `docs/part-4-输出阶段/08-响应渲染与TUI界面.md`：TUI 架构 Mermaid 组件图、消息渲染 Mermaid 时序图、Web UI vs TUI 对比表、流式输出伪代码、源码锚点表
- [x] 6.2 编写 `docs/part-4-输出阶段/09-权限控制与安全模型.md`：权限检查 Mermaid 流程图、权限规则配置伪代码、敏感操作分级表、源码锚点表
- [x] 6.3 验证：确认两篇文档 Mermaid 图渲染正常

## 7. 扩展阶段文档（Chunk 6）

- [x] 7.1 编写 `docs/part-5-扩展阶段/10-插件系统与二次开发.md`：插件架构 Mermaid 组件图、插件生命周期 Mermaid 时序图、manifest 字段说明表、插件开发伪代码示例、源码锚点表
- [x] 7.2 编写 `docs/part-5-扩展阶段/11-MCP协议集成.md`：MCP 协议概述、MCP 集成 Mermaid 架构图、Tool 发现与调用 Mermaid 时序图、MCP Server 配置伪代码、源码锚点表
- [x] 7.3 编写 `docs/part-5-扩展阶段/12-自定义Provider开发.md`：Provider 接口 Mermaid 类图、添加新 Provider 步骤流程图、自定义 Provider 伪代码、源码锚点表
- [x] 7.4 验证：确认三篇文档 Mermaid 图渲染正常

## 8. 工程实践文档（Chunk 7）

- [x] 8.1 编写 `docs/part-6-工程实践/13-Effect框架实战指南.md`：Effect 核心概念速查表、Effect vs RxJava 对比表、Effect 使用模式 Mermaid 流程图、错误处理/依赖注入/并发控制伪代码、源码锚点表
- [x] 8.2 编写 `docs/part-6-工程实践/14-构建系统与Monorepo.md`：Monorepo 结构 Mermaid 依赖图、Turborepo 编排 Mermaid 流程图、Bun workspace 说明、构建缓存策略、源码锚点表
- [x] 8.3 编写 `docs/part-6-工程实践/15-存储与数据模型.md`：存储层 Mermaid 架构图、核心数据模型 Mermaid ER 图、数据迁移策略、源码锚点表
- [x] 8.4 验证：确认三篇文档 Mermaid 图渲染正常

## 9. 附录文档（Chunk 8）

- [x] 9.1 编写 `docs/appendix/A-Java开发者TS速查.md`：TS vs Java 语法对比表、现代 TS 特性速查、Bun vs Node.js vs JVM 对比表、tsconfig.json 说明
- [x] 9.2 编写 `docs/appendix/B-设计模式总结.md`：OpenCode 设计模式清单表、每个模式的 Mermaid 图、与 GoF 模式对比
- [x] 9.3 编写 `docs/appendix/C-核心API参考索引.md`：核心模块 API 速查表、HTTP API 端点列表、opencode.json 配置字段索引
- [x] 9.4 验证：确认三篇附录文档表格格式正确、链接有效

## 10. 全局验证与收尾

- [x] 10.1 全链路验证：从 `docs/README.md` 出发，逐一点击所有文档链接，确认跳转正确、无死链
- [x] 10.2 Mermaid 图表全覆盖验证：确认所有文档中的 Mermaid 图在 GitHub 渲染正常（无语法错误）
- [x] 10.3 手机端可读性验证：确认文档在小屏设备上的排版、图表缩放、表格横向滚动均正常
- [x] 10.4 源码锚点一致性验证：抽查每个文档的源码锚点路径是否在 `opencode/` 目录中真实存在
- [x] 进行 artifact 文档，讨论结果的一致性检查

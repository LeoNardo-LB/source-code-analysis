## ADDED Requirements

### Requirement: 全局路线图文档
系统 SHALL 提供 `docs/README.md` 作为整个源码学习指南的总入口，包含以下内容：
- OpenCode 项目一句话描述与技术栈概览表
- 以 Mermaid 架构图展示整体系统分层（Client → Server → Engine → Provider → Tool）
- 按请求生命周期的学习路径建议（输入 → 推理 → 执行 → 输出 → 扩展 → 工程实践）
- 15 篇核心文档 + 3 篇附录的完整索引，含每篇的一句话简介和阅读顺序建议
- 面向 Java 开发者的阅读提示

#### Scenario: 读者查看全局路线图
- **WHEN** 读者在 GitHub 打开 `docs/README.md`
- **THEN** 页面 SHALL 展示完整的架构 Mermaid 图、学习路径和文档索引，GitHub 原生渲染无错误

#### Scenario: 读者按路径导航到具体文档
- **WHEN** 读者点击路线图中的文档链接
- **THEN** SHALL 正确跳转到 `docs/part-X-xxx/` 目录下对应的 .md 文件

### Requirement: 架构总览文档
系统 SHALL 提供 `docs/00-源码学习路线图与架构总览.md`，包含：
- C/S 架构详解（Server 端口 4096、多种 Client 类型）
- Monorepo 包结构图（Mermaid，18 个子包的依赖关系）
- 核心模块 40+ 子系统的职责总表
- 技术栈与 Java 生态类比速查表
- "一条消息的完整旅程"时序图（Mermaid，从用户输入到最终输出的完整链路）

#### Scenario: Java 开发者阅读架构总览
- **WHEN** Java 开发者打开架构总览文档
- **THEN** SHALL 看到技术栈与 Java 生态的类比速查（如 Effect vs RxJava、Zod vs Bean Validation），并能通过 Mermaid 时序图理解消息完整旅程

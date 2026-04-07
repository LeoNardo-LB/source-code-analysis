## Why

上一轮文档审计（commit ceff5f8）修复了 20 个高优先级问题，但仍遗留 12 个事实性错误/遗漏和 6 处跨文档重复内容。这些问题会误导开发者对核心机制的理解，且重复内容增加维护负担——一处修改需同步多处。现在趁审计上下文热，一次性完成全部修复和去重。

## What Changes

### 遗留事实性错误修复（12 项）
- **ER 图补全**：02-Session 的 Session 实体补充 `summary_files`、`revert` 字段；Part 实体补充 `id PK`
- **源码行号修正**：02-Session 中 Part 类型锚点 L87-401 → L87-350
- **环境信息注入补全**：03-Agent 中补充完整的 `Here is some useful information...` 前缀和 `<directories>` 部分
- **函数签名补全**：03-Agent 中 `resolveTools` 补充 `bypassAgentCheck` 和 `messages` 参数
- **架构图清理**：00-总览 移除不存在的 VSCode 扩展包节点
- **配置文件说明补充**：00-总览 补充全局 `config.json` 与项目 `opencode.json` 的区分
- **Agent Prompt 数量明确化**：03-Agent 中明确 "4 个 txt 文件：explore/compaction/title/summary"
- **子包数量统一**：00-总览 与 14-构建 统一子包数量描述
- **存储路径全面修正**：15-存储 中剩余的 `~/.opencode/` 引用改为 XDG 路径

### 跨文档去重 + 交叉引用化（6 处）
- **runLoop 执行循环**：02-Session 简化为摘要 + 指向 03-Agent 详细说明
- **数据库 PRAGMA 配置**：02-Session 删除 PRAGMA 详情，改为引用 15-存储
- **Provider 接口与注册**：12-自定义Provider 删除重复接口定义，改为引用 04-Provider
- **权限检查流程**：06-文件操作 简化安全机制部分，引用 09-权限控制
- **ER 关系图**：02-Session 简化为仅 Session 层实体，完整 ER 图只在 15-存储 维护
- **架构图/技术栈**：README 简化为简介 + 索引，详细架构只在 00-总览 维护

### 内容增强
- 00-总览 补充遗漏的重要源码模块描述（auth/、control-plane/、project/、env/ 等）

## Capabilities

### New Capabilities
- `doc-factual-fixes`: 修复 12 处文档与源码不一致的事实性错误和遗漏
- `doc-dedup-references`: 6 处跨文档重复内容去重，改为交叉引用
- `doc-content-enhancement`: 补充遗漏模块描述、统一数量引用

### Modified Capabilities

## Impact

- **受影响文件**（预估 12-15 个 .md 文件）：
  - `00-源码学习路线图与架构总览.md`
  - `README.md`
  - `part-1-输入阶段/02-Session与上下文构建.md`
  - `part-2-推理阶段/03-Agent系统与Prompt构建.md`
  - `part-3-执行阶段/06-文件操作与Shell执行.md`
  - `part-5-扩展阶段/12-自定义Provider开发.md`
  - `part-6-工程实践/14-构建系统与Monorepo.md`
  - `part-6-工程实践/15-存储与数据模型.md`
- **无代码变更**：纯文档修改，不影响任何源码
- **无破坏性变更**：仅删除重复内容并替换为交叉引用

## ADDED Requirements

### Requirement: Session ER 图补全 summary_files 和 revert 字段
02-Session 文档中的 Session ER 图实体 SHALL 包含 `summary_files: integer` 和 `revert: json` 字段，与源码 `session.sql.ts` 一致。

#### Scenario: Session ER 图字段完整性
- **WHEN** 检查 02-Session 文档中 Session 实体的 Mermaid ER 图
- **THEN** 实体包含 `int summary_files` 和 `json revert` 字段

### Requirement: Part ER 图补全 id 主键
02-Session 文档中的 Part ER 图实体 SHALL 包含 `id: text PK "PartID"` 主键字段。

#### Scenario: Part ER 图主键完整性
- **WHEN** 检查 02-Session 文档中 Part 实体的 Mermaid ER 图
- **THEN** 实体包含 `text id PK` 字段

### Requirement: 源码锚点行号修正
02-Session 文档中 message-v2.ts 的 Part 类型行号 SHALL 为 `L87-350`（非 L87-401）。

#### Scenario: 行号与源码一致
- **WHEN** 检查 02-Session 文档中 message-v2.ts 的锚点引用
- **THEN** Part 类型行号范围标记为 L87-350

### Requirement: 环境信息注入描述补全
03-Agent 文档中的环境信息注入伪代码 SHALL 包含完整的模板内容：`Here is some useful information about the environment you are running in:` 前缀、`<env>` 块和 `<directories>` 块。

#### Scenario: 环境信息模板完整性
- **WHEN** 检查 03-Agent 文档中环境信息注入的代码示例
- **THEN** 包含 `Here is some useful information` 前缀行和 `<directories>` 部分

### Requirement: resolveTools 函数签名补全
03-Agent 文档中 `resolveTools` 的调用示例 SHALL 包含 `bypassAgentCheck: boolean` 和 `messages: MessageV2.WithParts[]` 参数。

#### Scenario: 函数签名与源码一致
- **WHEN** 检查 03-Agent 文档中 resolveTools 的伪代码
- **THEN** 调用参数包含 bypassAgentCheck 和 messages

### Requirement: 架构图移除不存在的 VSCode 包
00-总览 的 Monorepo 包结构图中 SHALL NOT 包含 VSCode 扩展包节点（packages/ 下不存在该目录）。

#### Scenario: 架构图与实际包结构一致
- **WHEN** 检查 00-总览的 Mermaid 包结构图
- **THEN** 图中不存在 VSCode 相关节点

### Requirement: 配置文件双类型说明
00-总览的技术栈表中 SHALL 区分全局配置 `config.json`（`~/.config/opencode/`）和项目配置 `opencode.json`（`.opencode/`）。

#### Scenario: 配置文件类型完整
- **WHEN** 检查 00-总览中"配置"行
- **THEN** 同时提及 config.json（全局）和 opencode.json（项目）

### Requirement: Agent Prompt 数量明确化
03-Agent 文档中 Agent Prompt 描述 SHALL 明确为 "4 个 txt 文件：explore、compaction、title、summary"。

#### Scenario: Agent Prompt 数量准确
- **WHEN** 检查 03-Agent 文档中 Agent Prompt 相关描述
- **THEN** 明确列出 4 个文件名

### Requirement: 子包数量统一
00-总览和 14-构建文档中关于子包数量的描述 SHALL 使用相同数字。核实后统一为实际数量。

#### Scenario: 子包数量一致
- **WHEN** 对比 00-总览和 14-构建中的子包数量描述
- **THEN** 两处使用相同数字

### Requirement: 存储路径 XDG 化
15-存储文档中所有 `~/.opencode/` 路径引用 SHALL 替换为 XDG 标准路径 `~/.local/share/opencode/`。

#### Scenario: 存储路径与源码一致
- **WHEN** 搜索 15-存储文档中所有路径引用
- **THEN** 不存在 `~/.opencode/` 的引用（均为 XDG 路径）

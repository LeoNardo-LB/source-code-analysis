## 1. 遗留事实性错误修复

- [x] 1.1 02-Session ER 图：Session 实体补充 `summary_files: integer` 和 `revert: json` 字段
- [x] 1.2 02-Session ER 图：Part 实体补充 `id: text PK "PartID"` 主键字段
- [x] 1.3 02-Session 源码锚点：Part 类型行号 L87-401 修正为 L87-350
- [x] 1.4 03-Agent 环境信息注入：补充完整模板（`Here is some useful information...` 前缀 + `<directories>` 块）
- [x] 1.5 03-Agent resolveTools 签名：补充 `bypassAgentCheck: boolean` 和 `messages: MessageV2.WithParts[]` 参数
- [x] 1.6 00-总览 架构图：移除不存在的 VSCode 扩展包节点及连线
- [x] 1.7 00-总览 技术栈表：配置行补充全局 `config.json`（~/.config/opencode/）与项目 `opencode.json`（.opencode/）区分
- [x] 1.8 03-Agent Agent Prompt 描述：明确为 "4 个 txt 文件：explore、compaction、title、summary"
- [x] 1.9 00-总览与14-构建子包数量：核实实际包数量后统一描述
- [x] 1.10 15-存储 剩余路径：搜索并替换所有残留的 `~/.opencode/` 为 XDG 路径

## 2. 跨文档去重 — 交叉引用化

- [x] 2.1 02-Session runLoop 去重：删除详细伪代码/流程图，替换为摘要段落 + 引用链接
- [x] 2.2 02-Session PRAGMA 去重：删除完整 PRAGMA 代码块，替换为摘要 + 引用链接
- [x] 2.3 12-自定义Provider 接口去重：删除 ProviderService 完整类图和注册流程图，替换为接口摘要 + 引用链接
- [x] 2.4 06-文件操作 权限流程去重：删除完整权限检查流程图，替换为安全机制摘要 + 引用链接
- [x] 2.5 02-Session ER 图精简：移除 Project 和 Permission 实体，仅保留 Session/Message/Part/Todo
- [x] 2.6 README 架构去重：简化技术栈表和架构图为简介级别，添加引用链接指向 00-总览

## 3. 内容增强

- [x] 3.1 00-总览 核心模块表：补充 auth/、control-plane/、project/、env/、command/ 五个模块行
- [x] 3.2 00-总览 核心模块表：添加注释 "仅列出核心模块，完整模块列表见源码 src/ 目录（44 个）"

## 4. 验证与收尾

- [x] 4.1 进行 artifact 文档一致性检查（proposal ↔ specs ↔ tasks 三维度交叉比对）— 全部 PASS
- [x] 4.2 全文搜索验证：确认所有文档中不存在残留的 `~/.opencode/` 路径 — 附录 C 已修复
- [x] 4.3 交叉引用验证：确认所有新增的相对路径链接指向的文件存在 — 全部有效
- [x] 4.4 Mermaid 图表验证：确认所有修改后的图表无语法错误 — ER 图精简后语法正确

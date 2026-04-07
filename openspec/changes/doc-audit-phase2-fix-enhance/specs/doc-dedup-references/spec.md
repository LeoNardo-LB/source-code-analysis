## ADDED Requirements

### Requirement: runLoop 去重 — 02 简化引用 03
02-Session 文档中的 runLoop 执行循环详细伪代码和流程图 SHALL 替换为简短摘要段落，并添加指向 03-Agent 文档的交叉引用链接。

#### Scenario: 02-Session 无重复 runLoop 详情
- **WHEN** 检查 02-Session 文档
- **THEN** 不包含完整的 runLoop 伪代码（超过 10 行），而是包含摘要和引用链接

### Requirement: 数据库 PRAGMA 去重 — 02 简化引用 15
02-Session 文档中的数据库 PRAGMA 配置详情 SHALL 替换为简短提及，并添加指向 15-存储文档的交叉引用链接。

#### Scenario: 02-Session 无重复 PRAGMA 配置
- **WHEN** 检查 02-Session 文档
- **THEN** 不包含完整的 PRAGMA 代码块（WAL/NORMAL/5000/64000/ON），而是引用 15-存储

### Requirement: Provider 接口去重 — 12 简化引用 04
12-自定义Provider 文档中的 ProviderService 接口类图和注册流程图 SHALL 替换为简短接口摘要，并添加指向 04-Provider 文档的交叉引用链接。

#### Scenario: 12-自定义Provider 无重复接口定义
- **WHEN** 检查 12-自定义Provider 文档
- **THEN** 不包含完整的 ProviderService 类图（超过 7 个方法），而是引用 04-Provider

### Requirement: 权限检查去重 — 06 简化引用 09
06-文件操作 文档中的权限检查流程图 SHALL 替换为简短安全机制说明，并添加指向 09-权限控制 文档的交叉引用链接。

#### Scenario: 06-文件操作 无重复权限流程
- **WHEN** 检查 06-文件操作 文档
- **THEN** 不包含完整的权限检查流程图，而是引用 09-权限控制

### Requirement: ER 图去重 — 02 精简版
02-Session 文档中的 ER 关系图 SHALL 精简为仅包含 Session 层实体（Session、Message、Part、Todo），移除 Project 和 Permission 实体。

#### Scenario: 02-Session ER 图精简
- **WHEN** 检查 02-Session 文档中的 ER 图
- **THEN** 仅包含 Session、Message、Part、Todo 四个实体，并注明完整 ER 图见 15-存储

### Requirement: 架构图去重 — README 简化
README 中的技术栈表和架构图 SHALL 简化为简介级别，详细内容通过链接指向 00-总览。

#### Scenario: README 无详细重复架构
- **WHEN** 检查 README
- **THEN** 技术栈和架构部分为简要概述（不超过 10 行表格），包含指向 00-总览的链接

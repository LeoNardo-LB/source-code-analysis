## ADDED Requirements

### Requirement: 补充遗漏源码模块描述
00-总览的核心模块一览表 SHALL 补充 5 个重要遗漏模块：auth/、control-plane/、project/、env/、command/，每个模块包含路径、职责和 Java 类比。

#### Scenario: 遗漏模块已补充
- **WHEN** 检查 00-总览的核心模块一览表
- **THEN** 表格包含 auth、control-plane、project、env、command 五个模块行

### Requirement: 模块表添加注释说明
00-总览的核心模块一览表 SHALL 在表头或表尾添加注释，说明仅列出核心模块，完整模块列表参见源码 `src/` 目录。

#### Scenario: 模块表有完整性说明
- **WHEN** 检查 00-总览的核心模块一览表
- **THEN** 表格附近有"仅列出核心模块，完整列表见 src/ 目录"的注释

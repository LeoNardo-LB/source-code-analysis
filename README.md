# Source Code Analysis

> 面向 Java 开发者的开源项目源码深度学习仓库
> 以"图表优先、伪代码驱动、不看源码也能理解"为理念

---

## 项目索引

| 项目 | 版本 | 语言 | 文档数 | 状态 | 入口 |
|------|------|------|--------|------|------|
| [OpenCode](opencode/) | v1.3.17 | TypeScript | 18 篇 + 3 附录 | ✅ 完成 | [阅读指南 →](opencode/docs/README.md) |

### 即将添加

| 项目 | 语言 | 计划方向 |
|------|------|----------|
| LangChain4j | Java | AI Agent 框架源码分析 |
| Spring AI | Java | Spring 生态 AI 集成源码分析 |
| ... | ... | ... |

---

## 每个项目的目录结构

```
<project-name>/
├── README.md          # 项目学习指南入口（= docs/README.md 的副本）
├── docs/              # 全部教学文档
│   ├── 00-架构总览.md
│   ├── part-1-xxx/    # 按阶段组织
│   └── appendix/      # 附录
├── <source>/          # 源码（.gitignore，仅供本地参考）
└── openspec/          # 变更提案归档（.gitignore）
```

---

## 阅读方式

1. **电脑端**：直接在 GitHub 浏览，Mermaid 图表原生渲染
2. **手机端**：同上，GitHub 移动端支持 Mermaid
3. **本地**：clone 到本地用任意 Markdown 阅读器

---

## 关于

- **作者**: [LeoNardo-LB](https://github.com/LeoNardo-LB)
- **目标读者**: 有 Java 后端经验、学过 JS/TS 的开发者
- **文档风格**: 图表优先 + 伪代码驱动 + 轻量 Java 类比

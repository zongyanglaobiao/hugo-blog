---
title: CodeGraph
description: CodeGraph 是什么，以及如何使用 CodeGraph
slug: use-codeGraph
date: 2026-06-02 18:06:15+0000
# 是否生成目录
toc: true
categories:
  - ai-category
  - open-source-project-category
tags:
  - codegraph
keywords:
  - AI
  - codegraph
id: 1ac13ad9-30ff-4e1d-aec4-951179735cbb
# 是否可以添加评论
comments: true
---

# CodeGraph

[CodeGraph](https://github.com/colbymchenry/codegraph) 是一个面向 AI 编程 Agent 的本地代码知识图谱工具。它会提前为项目建立索引，让 Claude Code、Codex、Gemini、Cursor、opencode、Antigravity、Kiro、Hermes Agent 等工具通过 MCP 查询代码结构、符号关系和调用链，从而减少反复 `grep`、`find`、`Read` 带来的令牌消耗和工具调用次数。

## 安装 CodeGraph 并配置 Agent

我使用的是 macOS：

1. 安装 CLI：`npm i -g @colbymchenry/codegraph`
2. 配置 Agent：`codegraph install`，交互式检测并写入需要配置的 Agent。也可以使用 `npx @colbymchenry/codegraph` 一次性下载并运行安装器。
3. 初始化项目并建立索引：在项目根目录执行 `codegraph init -i`

完成以上步骤后，Agent 在分析项目结构、调用链或影响范围时，就可以通过 CodeGraph MCP 服务直接查询本地索引。

## CodeGraph 常用命令

### 安装与项目初始化

| 命令 | 说明 |
| --- | --- |
| `codegraph` | 启动交互式安装器。 |
| `codegraph install` | 显式运行安装器，自动检测并配置支持的 Agent。 |
| `codegraph install --yes` | 非交互式安装，适合脚本或 CI 环境。 |
| `codegraph install --target=cursor,claude --yes` | 只为指定 Agent 配置 CodeGraph。 |
| `codegraph install --print-config codex` | 输出指定 Agent 的 MCP 配置片段，不直接写入配置文件。 |
| `codegraph uninstall` | 从已配置的 Agent 中移除 CodeGraph MCP 配置。 |
| `codegraph init [path]` | 在项目中创建 `.codegraph/` 索引目录。 |
| `codegraph init -i [path]` | 初始化项目并立即建立索引。 |
| `codegraph uninit [path]` | 移除项目中的 CodeGraph 初始化信息；如需跳过确认可加 `--force`。 |

### 索引与状态检查

| 命令 | 说明 |
| --- | --- |
| `codegraph index [path]` | 对项目执行完整索引；常用参数有 `--force`、`--quiet`。 |
| `codegraph sync [path]` | 执行增量同步。MCP 服务启动后通常会自动监听文件变化，手动同步主要用于排查缺失符号等问题。 |
| `codegraph status [path]` | 查看索引统计、健康状态和数据库信息。 |
| `codegraph files [path]` | 查看已索引的文件结构；常用参数有 `--format`、`--filter`、`--max-depth`、`--json`。 |
| `codegraph serve --mcp` | 启动 MCP 服务。安装器通常会把这个命令写入 Agent 配置。 |

### 代码查询与影响分析

| 命令 | 说明 |
| --- | --- |
| `codegraph query <search>` | 按名称搜索符号；常用参数有 `--kind`、`--limit`、`--json`。 |
| `codegraph callers <symbol>` | 查询某个函数或方法被哪些位置调用。 |
| `codegraph callees <symbol>` | 查询某个函数或方法内部调用了哪些函数或方法。 |
| `codegraph impact <symbol>` | 分析修改某个符号可能影响的代码范围；常用参数有 `--depth`、`--json`。 |
| `codegraph affected [files...]` | 根据变更文件推导可能受影响的测试文件。 |

`affected` 也适合配合 Git 使用，例如：

```bash
git diff --name-only | codegraph affected --stdin --quiet
```

这条命令会将 Git 变更文件通过标准输入传给 `affected`，并只输出受影响的测试文件路径。

除了 CLI 命令，Agent 通过 MCP 使用 CodeGraph 时还会调用 `codegraph_explore`、`codegraph_search`、`codegraph_callers`、`codegraph_callees`、`codegraph_impact`、`codegraph_node`、`codegraph_files`、`codegraph_status` 等工具。其中 `codegraph_explore` 通常是最常用的入口，适合回答“某个功能如何工作”“某个流程如何到达另一个模块”等结构化问题。

## 总结

从实际体验看，CodeGraph 对“理解项目结构、追踪调用链、分析影响范围”这类问题比较有帮助。同样的问题相比直接让 Agent 在文件系统中反复搜索，响应会更快，工具调用也更少。至于长期能节省多少 Token，还需要结合具体项目规模和使用场景继续观察。



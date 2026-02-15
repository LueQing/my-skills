---
name: siyuan-note
description: 在思源笔记（SiYuan）中进行笔记创建、整理、检索、结构调整与属性维护。用于“写入思源”“整理文档”“查找笔记”“移动块或文档”“读取块属性或反链”“执行 SQL 检索”“写入每日笔记并自动拟题”等场景。
---

# Siyuan Note

## 概览

使用 SiYuan MCP 工具完成文档与块级操作。优先保证结构正确、位置准确、结果可回读验证。

## 最小工作流

1. 确认目标对象（文档/块/笔记本）和目标位置（`parentID`、`previousID`、`nextID`、文档 ID）。
2. 选择最小必要工具执行。
3. 回读验证并报告写入位置（文档 ID/块 ID）。

## 必须遵守

- 更新块前先读取 Kramdown：`siyuan_get_block_kramdown` -> `siyuan_update_block`。
- SQL 查询前先读取 schema：`siyuan_database_schema` -> `siyuan_query_sql`。
- 写入每日笔记前先确认笔记本为打开状态：`siyuan_list_notebook`。
- 复杂任务小步执行，逐步验证。

## 参考文件（按需加载）

- 工具与任务映射：`references/tool-recipes.md`

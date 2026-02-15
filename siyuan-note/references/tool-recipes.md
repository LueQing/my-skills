# Tool Recipes

## 目录

1. 常用工具映射
2. 每日笔记写入模板
3. 回读验证清单

## 1. 常用工具映射

- 新建文档：`siyuan_create_note_with_md`
- 读取内容：`siyuan_read_doc_content`、`siyuan_get_children_blocks`
- 追加内容：`siyuan_append_markdown_to_doc`、`siyuan_append_block`、`siyuan_prepend_block`
- 精确插入：`siyuan_insert_block`
- 更新块（保留格式）：`siyuan_get_block_kramdown` + `siyuan_update_block`
- 属性读写：`siyuan_get_block_attributes`、`siyuan_set_block_attributes`
- 结构调整：`siyuan_move_block_by_id`、`siyuan_move_docs_by_ids`
- 检索分析：`siyuan_database_schema` + `siyuan_query_sql`
- 反链：`siyuan_get_doc_backlinks`
- 每日笔记：`siyuan_list_notebook` + `siyuan_append_to_dailynote`

## 2. 每日笔记写入模板

适用指令：
- “根据 XXX 内容为我在每日笔记中创建笔记（标题根据 XXX 拟定）并写入内容”

执行步骤：
1. 调用 `siyuan_list_notebook` 找到目标笔记本并确认 `closed=false`。
2. 根据输入内容拟定标题。
3. 调用 `siyuan_append_to_dailynote` 写入：

```markdown
## <拟定标题>

<正文内容>
```

## 3. 回读验证清单

- 是否写入到正确笔记本/文档。
- 是否写入到预期位置（父块或前后块关系）。
- 是否保留原有属性和样式（涉及更新块时）。

# LaTeX 论文工作流

当用户要求基于数学建模模板输出论文时，按此流程执行。

## 模板位置

- `assets/latex-template/example.tex`
- `assets/latex-template/cumcmthesis.cls`

## 使用原则

- 优先在 `example.tex` 基础上改写，不要从零手搓整篇模板。
- 不要随意修改 `cumcmthesis.cls`，除非用户明确要求修改模板行为。
- 论文内容必须和 Python 求解结果一致，图表、参数、结论要能互相对上。

## 推荐流程

1. 先完成 Python 求解，拿到稳定结果。
2. 确定论文章节结构与需要插入的图表、表格、公式。
3. 基于 `example.tex` 填充摘要、假设、符号说明、模型建立、求解、结果分析。
4. 若结果来自 `.ipynb`，提取关键图和关键指标，不要整段粘贴 Notebook 原始输出。
5. 给出 Windows 下的编译说明与依赖提醒。

## Windows 说明

- 路径、图片引用、数据文件路径优先写成 Windows 用户易理解的形式。
- 若需要编译指令，优先给出适用于 Windows TeX 发行版或编辑器的说明。
- 不要默认用户有 Bash、Makefile 或 Unix 工具链。

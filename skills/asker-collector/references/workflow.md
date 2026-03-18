# asker-collector workflow

## 核心架构

推荐把能力拆成 3 个模块：

- `Prompt Generator`
  - 生成首轮主 prompt、二轮追问 prompt、附件识别失败时的备用前缀
- `Browser Reader`
  - 识别浏览器现有标签页，读取 Claude / Grok / ChatGPT 等站点回答正文
- `Synthesizer`
  - 归纳共识、分析分歧、判断风险并输出最终 Markdown

## 标准工作流

### 阶段 1：生成投喂包

输入通常包括：

- 用户任务目标
- 题目类型
- 是否偏向代码、论文、方案或对比分析

建议输出：

- 首轮 prompt
- 二轮 prompt
- 备用前缀
- `prompts.md`

### 阶段 2：用户手动投喂

用户自行完成：

- 打开或切换到目标网页 AI
- 上传附件
- 粘贴 prompt
- 发送消息
- 如有需要发送追问

此阶段默认不接管发送动作。

### 阶段 3：等待用户确认回答已完成

典型触发语：

- “三个网页已经回答完毕，请整合”
- “Claude、Grok、ChatGPT 都出结果了”

### 阶段 4：读取浏览器内容

典型顺序：

1. 列出当前页面
2. 识别属于 Claude / Grok / ChatGPT / 其他站点的标签页
3. 切换到目标页面
4. 读取页面结构快照
5. 提取问答正文
6. 如仍在生成，则等待稳定后再读

读取目标是“现成对话内容”，不是默认继续操控网页。

### 阶段 5：综合整理

至少完成三件事：

1. 提取多家共识
2. 标出主要分歧与错误风险
3. 给出最终推荐路线

建议输出到：

- `result.md`

## 推荐参数

- `sites`
  - 默认：`claude`, `grok`, `chatgpt`
- `prompt_mode`
  - 可选：`idea_only`, `paper_outline`, `code_plan`, `solution_compare`
- `output_mode`
  - 可选：`synthesis`, `comparison`, `raw_by_site`, `paper_ready_outline`
- `save_path`
  - 默认：`result.md`
- `manual_send`
  - 默认：`true`

## 站点适配原则

- 先抽象：
  - 页面识别
  - 用户消息识别
  - 模型消息识别
  - 回答正文提取
- 再做站点适配，不要把流程写成针对单一站点的硬编码分支。

## 推荐适用场景

- 数学建模题分析
- 论文思路对比
- 多模型方案收集
- 多站点答案交叉验证

## 默认边界

- 不默认接管登录、验证码、上传
- 不默认全自动输入与发送
- 优先读取已有会话结果并整理

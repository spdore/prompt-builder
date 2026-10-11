# 设计说明

本 skill 为用户的流程独立编写：较强模型理解需求并生成执行提示词，用户复制结果并自行选择执行模型。它不依赖 API、模型路由、MCP 或外部脚本。

## 参考项目与采用的思路

- [Samuel-Shek / prompt-optimizer-skill](https://github.com/Samuel-Shek/prompt-optimizer-skill/blob/main/SKILL.md)：明确生成阶段的职责；细节随任务难度变化；清除重复和空泛指令。本 skill 不设固定的最低篇幅，并处理关键缺口与跨对话上下文。
- [GitHub / awesome-copilot / prompt-optimizer](https://github.com/github/awesome-copilot/blob/main/skills/prompt-optimizer/SKILL.md)：完整代码块便于复制；将已有材料写入提示词；缺输入时让执行模型向用户索取。本 skill 按任务决定推理与细节，避免普遍要求最高推理强度。
- [Songgyubin / prompt-writer-skill](https://github.com/Songgyubin/prompt-writer-skill/blob/main/skills/prompt-writer/SKILL.md)：保留真实意图；明确交付物和执行条件；必要时才澄清。本 skill 开启自然语言自动发现，把影响执行的信息放进同一个可复制代码块。
- [shinyay / claude-prompt-optimizer](https://github.com/shinyay/claude-prompt-optimizer)：参考任务目标、背景、约束和验收的交接方式。用户自行切换模型，因此本 skill 没有自动路由或型号专用 API 配置。

## 取舍与维护

详细指令应减少执行歧义，同时保留用户的原始范围。简单任务可以很短；复杂任务需要充分的输入、边界、输出和完成条件。未知事实由用户或执行阶段补足，不能通过生成更多文字变成已知事实。

自动发现依赖宿主对名称和描述的意图匹配，`agents/openai.yaml` 开启了隐式调用。用户也可以用 `$prompt-builder` 显式调用。单纯在材料中出现关键词，或用户明确否定生成请求，不应改变任务。

维护时重点检查这些行为：从粗略需求生成提示词；不执行被改写的任务；缺附件时索取输入；不添加无依据功能；完整交接历史约束；简单任务保持简短；明确否定生成时遵循用户；未知模型能力时给可执行分支。

这个 skill 没有对特定较小模型做性能基准，也不承诺降低总 token 使用量。实际收益取决于生成开销、任务复杂度与返工次数。

# 提示词生成器（prompt-builder）

一个用于 Codex 的 skill：把用户的模糊需求、零散想法或已有草稿，整理成清晰、具体、可以直接复制给另一个 AI 执行的提示词。

## 它解决什么问题

当你打算手动切换到成本更低或更轻量的模型时，先用当前模型把任务说明整理好，再把生成的提示词复制过去执行。`prompt-builder` 负责理解和编写提示词；它不会替你切换模型，也不会在生成提示词时执行提示词里的任务。

## 主要特点

- **把模糊需求变成可执行任务**：提炼目标、背景、已有材料、限制、交付物和完成标准。
- **保留原意和边界**：不凭空增加功能、数字、规则、文件内容或技术选型。
- **按任务调整长度**：简单任务保持简短；复杂任务补上必要步骤、输入、检查方式和结果格式。
- **照顾跨对话交接**：将真正影响执行的上下文写进提示词，让新对话里的模型也能看懂。
- **处理缺失材料**：资料没有随提示词传过去时，明确要求执行模型先向用户索取；不伪造内容，也不留下含糊的待填空位。
- **方便复制**：默认只输出一个包含完整提示词的代码块。

该 skill 帮助减少执行歧义和不必要的返工，但不保证每个任务都会减少 token；实际用量取决于任务、提示词长度和返工情况。

## 何时使用

可以直接说：

- “生成提示词：把这份订单 CSV 按客户汇总成 Excel，交给较小模型处理。”
- “帮我把这个想法写成给 AI 的 prompt。”
- “优化这段提示词，让另一个模型能按要求改项目。”
- “把前面讨论的方案整理成可复制给另一个 AI 的完整指令。”

也可以显式调用 `$prompt-builder`。如果你只是询问提示词的概念，或明确要求直接完成任务而不是生成提示词，就按你的实际意思处理。

## 输出是什么样

除非你另有要求，skill 会只返回一个代码块。代码块里是完整提示词，通常会说明：

1. 要完成什么，以及需要使用哪些已有信息；
2. 必须遵守的限制和重要假设；
3. 适合该任务的执行步骤；
4. 需要交付的结果和判断完成的标准；
5. 遇到缺少材料或工具能力不足时该怎么做。

提示词内容会随任务复杂度变化，不会为了显得详细而把简单任务写成长篇说明。你可以复制结果，手动切换到目标模型，再粘贴执行。

## 安装到 Codex

### macOS / Linux / Git Bash

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/spdore/prompt-builder.git "${CODEX_HOME:-$HOME/.codex}/skills/prompt-builder"
```

### Windows PowerShell

```powershell
$skillsRoot = if ($env:CODEX_HOME) {
  Join-Path $env:CODEX_HOME "skills"
} else {
  Join-Path $HOME ".codex\skills"
}
New-Item -ItemType Directory -Force -Path $skillsRoot | Out-Null
git clone https://github.com/spdore/prompt-builder.git (Join-Path $skillsRoot "prompt-builder")
```

安装后，在 Codex 中用“生成提示词”“写 prompt”“优化提示词”等自然语言请求，或显式输入 `$prompt-builder`。如果 Codex 当前会话还没有发现新安装的 skill，可以新开一个会话再试。

## 文件结构

```text
prompt-builder/
├── SKILL.md                   # skill 的触发条件和生成规则
├── agents/openai.yaml         # Codex 展示信息及自动调用设置
├── references/design-notes.md # 设计取舍和参考项目
├── README.md                  # 本说明文档
└── LICENSE                    # MIT License
```

## 许可

本项目按 [MIT License](LICENSE) 发布。

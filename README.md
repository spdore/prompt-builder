# Prompt Builder

**Prompt Builder** is a Codex skill that turns a vague request, rough notes, or a draft into a clear, actionable prompt that can be copied into another AI.

It is useful when you want one model to clarify a task before you manually hand it to another model, including a smaller or lower-cost model. Prompt Builder writes the handoff; it does not switch models, route requests, or carry out the task inside the prompt.

## What it does

- Clarifies the goal, relevant context, available inputs, constraints, deliverables, and completion criteria.
- Preserves the user's intent without inventing facts, requirements, or features.
- Adjusts the prompt's length and structure to the task instead of making every prompt long.
- Makes copied prompts self-contained by including the context that matters across conversations.
- Handles missing files or information by telling the executing AI what to request before starting.
- Returns one complete, easy-to-copy prompt by default.
- Supports multilingual use: it writes the generated prompt in the user's language unless the user chooses another language. The skill instructions and this repository's documentation are written in English.

The skill can reduce ambiguity and avoid some rework, but it does not guarantee lower total token usage. Results depend on the task, prompt length, model, and number of follow-up corrections.

## When to use it

Ask naturally, for example:

- “Generate a prompt that tells an AI to summarize this customer survey.”
- “Turn this rough idea into clear instructions for another AI.”
- “Improve this prompt so another model can make the requested change in my project.”
- “Write a copy-ready prompt using the requirements we discussed.”

Requests in other languages work too. You can also invoke the skill explicitly with `$prompt-builder`. It should not activate for a question about prompt-writing concepts alone, or when you ask the AI to perform the task directly.

## Output

Unless you ask for another format, the skill returns one code block containing the complete prompt. Depending on the task, it may include:

1. The goal and relevant input materials.
2. Constraints and important assumptions.
3. A concise execution plan when steps matter.
4. Deliverables and completion criteria.
5. Instructions for handling missing materials or unavailable capabilities.

The level of detail follows the task. Copy the prompt, switch to your chosen model if needed, and paste it into that model.

## Install in Codex

### macOS, Linux, or Git Bash

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

If the skill is already installed, update it from its installation directory with `git pull` instead of cloning it again. If Codex does not discover the skill in the current conversation, start a new conversation and try again.

## Repository files

```text
prompt-builder/
├── SKILL.md                   # Invocation rules and prompt-writing instructions
├── agents/openai.yaml         # Codex display metadata and implicit invocation
├── references/design-notes.md # Design choices and related projects
├── README.md                  # This guide
└── LICENSE                    # MIT License
```

## License

This project is distributed under the [MIT License](LICENSE).


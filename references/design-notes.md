# Design Notes

Prompt Builder is designed around a manual handoff: one model understands the request and prepares a prompt; the user copies it and chooses the execution model. The skill does not depend on an API, model router, MCP integration, or external script.

## Related projects and ideas adopted

- [Samuel-Shek / prompt-optimizer-skill](https://github.com/Samuel-Shek/prompt-optimizer-skill/blob/main/SKILL.md): separates prompt writing from task execution, scales detail to task complexity, and removes repetition and vague directions. Prompt Builder also avoids a fixed minimum length and accounts for critical missing inputs and cross-conversation context.
- [GitHub / awesome-copilot / prompt-optimizer](https://github.com/github/awesome-copilot/blob/main/skills/prompt-optimizer/SKILL.md): uses a complete copyable block, incorporates supplied material, and requests missing inputs before execution. Prompt Builder chooses detail and reasoning instructions based on the task instead of always demanding maximum reasoning.
- [Songgyubin / prompt-writer-skill](https://github.com/Songgyubin/prompt-writer-skill/blob/main/skills/prompt-writer/SKILL.md): preserves intent, clarifies deliverables and execution conditions, and asks questions only when necessary. Prompt Builder supports natural-language discovery and keeps relevant context in the copyable prompt.
- [shinyay / claude-prompt-optimizer](https://github.com/shinyay/claude-prompt-optimizer): informed the handoff of task goals, context, constraints, and acceptance criteria. Since users select their own execution model, Prompt Builder does not configure automatic routing or model-specific APIs.

## Design choices and maintenance

Detailed instructions should reduce ambiguity while preserving the user's original scope. A simple task can have a short prompt; a complex task may need clear inputs, boundaries, outputs, and completion criteria. Unknown facts remain unknown and must be supplied by the user or resolved during execution.

Automatic discovery depends on the host matching the skill name and description to the user's intent. `agents/openai.yaml` enables implicit invocation, and users can also invoke `$prompt-builder` explicitly. A trigger phrase appearing only inside source material should not activate the skill, and an explicit request not to generate a prompt should be respected. The skill supports multilingual requests and defaults the generated prompt to the user's language, independently of the English authoring language used for this repository.

Maintenance should preserve these behaviors: generate a prompt from a rough request; do not perform the task being rewritten; request missing attachments; avoid unsupported additions; carry forward relevant context; keep simple prompts concise; respect an explicit rejection of prompt generation; and give actionable alternatives for unknown model capabilities.

This skill has not been benchmarked against specific smaller models and does not guarantee reduced total token usage. Actual savings depend on the cost of prompt generation, task complexity, and the amount of rework avoided.


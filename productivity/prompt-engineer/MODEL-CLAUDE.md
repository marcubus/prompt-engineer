# Claude deltas

- XML tags: `<instructions>` core task, `<context>` background, `<document>` or `<content>` source material, `<example>` inside `<examples>`, `<output_format>` response shape, `<audience>` audience profile. Tag only when tags remove ambiguity. Skip tags on simple prompts.
- Long context: role and context first, source material next, task and output format near the end. Claude follows instructions best when the task sits near the end.
- Style mirroring: Claude matches the prompt register. Write the prompt in the register you want back. Bullets produce bullets. Prose produces prose.
- Thinking vocabulary: avoid "think" as an instruction verb when extended thinking is off. Use "reason through", "work through", "analyse", "evaluate step by step".
- Tool use: `name`, `description`, JSON Schema `input_schema`. `tool_choice` forces selection. State when tools are available.

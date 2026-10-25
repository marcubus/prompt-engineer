# Claude deltas

- XML tags: `<instructions>` core task, `<context>` background, `<document>` or `<content>` source material, `<example>` inside `<examples>`, `<output_format>` response shape, `<audience>` audience profile. Use tags when they remove ambiguity.
- Long context: role and context first, source material next, task and output format near the end. Claude follows instructions best when the task sits near the end.
- Style mirroring: Claude matches the prompt register. Write the prompt in the register you want back. Bullets produce bullets. Prose produces prose.
- Thinking vocabulary: when extended thinking is off, use "reason through", "work through", "analyse", or "evaluate step by step" as instruction verbs.
- Tool use: `name`, `description`, JSON Schema `input_schema`. `tool_choice` forces selection. State when tools are available.

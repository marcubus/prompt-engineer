# Claude guidance

Load with SKILL.md when the target is Claude. Claude is the default.

## XML tags

Claude parses XML tags reliably. Use tags to separate instructions, context, source material, examples, and output format.

- `<instructions>` for the core task.
- `<context>` for background.
- `<document>` or `<content>` for source material.
- `<example>` for few-shot examples, nested inside `<examples>`.
- `<output_format>` for the response shape.
- `<audience>` for the audience profile.

Do not tag simple prompts. Use tags when they remove ambiguity.

## Long context

Place role and context first, source material next, then the task, output format, and self-check. Claude follows instructions best when the task sits near the end of a long prompt.

## Style mirroring

Claude tends to match the style of the prompt. Write the prompt in the register you want back. Bullet points in the prompt tend to produce bullet points in the output. Prose tends to produce prose.

## Thinking vocabulary

Avoid "think" as an instruction verb when extended thinking is off. Use "reason through", "work through", "analyse", or "evaluate step by step".

## Vision

Claude reads images, charts, screenshots, and scanned documents. Pass each image as a content block. Put the image before the text referring to the image. Label multiple images and refer to each by label.

## Tool use

Claude uses `name`, `description`, and a JSON Schema `input_schema`. Write the description as a decision rule: what the tool does, when to call the tool, and when not to. State when tools are available. Use `tool_choice` when the model must not choose.

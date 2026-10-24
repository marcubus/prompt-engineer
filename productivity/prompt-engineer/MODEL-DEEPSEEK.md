# DeepSeek guidance

Load with SKILL.md when the target is DeepSeek.

DeepSeek has two families. The V3 and V4 chat models answer directly. The R1 reasoning models emit a reasoning block before the answer. Identify the family before writing.

## Reasoning models

- Do not add chain-of-thought. R1 reasons internally. "Think step by step" duplicates the work and lowers quality.
- Skip the system prompt. Put all instructions in one self-contained user message.
- Use zero-shot. Few-shot examples degrade R1. Leave them out.
- State the final-answer format: sections, table, or word count.
- If the deployment echoes the trace, add "Return only the final answer, without the reasoning."

## Chat models

- Chain-of-thought helps on analytical and decisioning tasks.
- The system role works normally.
- Few-shot examples work for format and style.
- Use labelled sections: `TASK`, `CONTEXT`, `SOURCE`, `OUTPUT FORMAT`.
- DeepSeek handles markdown, tables, and code fences well.

## Sampling

Set these at the API, not in the prompt.

- V3 and V4 general chat: temperature 1.3.
- V3 and V4 coding and maths: temperature 0.0 to 0.6.
- R1 reasoning: temperature 0.6.

## Structured output and code

V3 and V4 support JSON output and function calling. State the schema and add "Return only valid JSON, with no surrounding text." For code, name the language, version, framework, and style.

## Vision

V3, V4, and R1 are text-only. For images, target DeepSeek-VL and apply the skill's multimodal framework. A text DeepSeek model will invent the contents of an attachment.

## Function calling

V3 and V4 expose an OpenAI-compatible `tools` interface. Define each tool with a name, a description stating when to call the tool, and a parameter schema. Validate the returned arguments before use. R1 is less reliable at tool calling. Prefer V3 or V4 for agent loops.

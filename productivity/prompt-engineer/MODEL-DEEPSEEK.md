# DeepSeek deltas

DeepSeek has two families. V3 and V4 chat models answer directly. R1 reasoning models emit a reasoning block before the answer. Identify the family before writing.

- R1 reasoning: R1 reasons internally. Give one self-contained user message with the final-answer format. Use zero-shot. Extra CoT duplicates the work and lowers quality. If the deployment echoes the trace, add "Return only the final answer, without the reasoning."
- V3 and V4 chat: chain-of-thought helps on analytical and decisioning tasks. The system role works normally. Few-shot examples work for format and style. Use labelled sections `TASK`, `CONTEXT`, `SOURCE`, `OUTPUT FORMAT`.
- Sampling: set at the API, not in the prompt. General chat 1.3. Coding and maths 0.0 to 0.6. R1 0.6.
- JSON and code: V3 and V4 support JSON output and function calling. State the schema and add "Return only valid JSON, with no surrounding text." For code, name the language, version, framework, and style.
- Vision: V3, V4, and R1 are text-only. Route images to DeepSeek-VL and apply the multimodal framework in `SKILL.md`.
- Function calling: V3 and V4 expose an OpenAI-compatible `tools` interface. Validate returned arguments before use. R1 is less reliable at tool calling. Prefer V3 or V4 for agent loops.

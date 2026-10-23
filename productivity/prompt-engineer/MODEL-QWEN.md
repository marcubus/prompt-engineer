# Qwen guidance

Load with SHARED.md when the target is Qwen.

## Thinking switch

Qwen3 has two modes. Thinking reasons before answering. Non-thinking answers fast. Match the mode to the task: thinking for maths, code, logic, and multi-step analysis. Non-thinking for chat, extraction, formatting, and simple instruction.

- In the message: append `/think` or `/no_think`.
- In the API or chat template: set `enable_thinking` to true or false.
- Qwen3-Thinking cannot disable thinking. Qwen2.5 has no thinking mode.

## Sampling

Set these at the API, not in the prompt.

- Thinking: temperature 0.6, top_p 0.95, top_k 20, min_p 0.
- Non-thinking: temperature 0.7, top_p 0.8, top_k 20, presence_penalty 1.5.
- Do not use greedy decoding with Qwen3. Greedy decoding risks repetition.

## Chat template

Let the runtime apply the Qwen chat template. Do not hand-insert special tokens. Keep the system role for standing role and format. Keep the user role for the task.

## Few-shot

Qwen responds well to three to five diverse input-output pairs for format, classification, and style.

## Structured output and coding

Give input tables as markdown and state the output schema exactly: columns, JSON keys, enum values. For strict JSON, add "Return only valid JSON, with no surrounding text." Qwen3-Coder is the code specialist. Name the language, version, framework, and style.

## Vision

Images and video need a vision variant, Qwen2.5-VL or Qwen3-VL. These read images, documents, and video, and return bounding boxes. State the unit of output for video: transcript, timestamped summary, or per-scene analysis. A text Qwen model will invent the contents of an attachment.

## Function calling

Qwen2.5 and Qwen3 expose an OpenAI-compatible `tools` interface and the Qwen-Agent framework. Define each tool with a name, a description stating when to call the tool, and typed parameters. Qwen supports tool use in both modes. Constrain parameters with enums where the domain is fixed.

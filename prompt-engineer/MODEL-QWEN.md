# Prompt Engineering Knowledge Base: Qwen-Specific Techniques

## Purpose

This document covers techniques specific to Alibaba's Qwen models: the Qwen2.5 series and the Qwen3 series (including Qwen3-Instruct, Qwen3-Thinking, and Qwen3-Coder). Load this alongside the shared base KB when the target is Qwen. Qwen3's defining feature is a hybrid thinking mode you control from the prompt itself.

---

## 1. Thinking is a switch (Qwen3)

Qwen3 runs in two modes:

- **Thinking**: reasons before answering. Use this mode for maths, code, logic, and multi-step analysis.
- **Non-thinking**: answers fast. Use this mode for chat, extraction, formatting, and simple instruction.

Control the mode:

- **Soft switch in the message:** append `/think` to force reasoning, or `/no_think` to force the fast path.
- **API / chat template:** set `enable_thinking` (true or false). With `enable_thinking=False` the model answers directly.

Qwen3-Thinking models cannot disable thinking. Qwen2.5 has no thinking mode, so treat Qwen2.5 as non-thinking only. Match the mode to the task: pair thinking with hard problems, `/no_think` with simple or latency-sensitive ones.

---

## 2. Recommended sampling parameters

Set at the API, not in the prompt, but they change results. Qwen3's documented best practices:

- **Thinking mode:** temperature 0.6, top_p 0.95, top_k 20, min_p 0.
- **Non-thinking mode:** temperature 0.7, top_p 0.8, top_k 20, presence_penalty 1.5.

Do not use greedy decoding with Qwen3. Greedy decoding risks repetition and degraded output.

---

## 3. Use the chat template, keep roles clean

Qwen-Instruct models are trained with a specific chat template. When prompting through an API or a runtime that applies the template (vLLM, llama.cpp, LM Studio, Ollama), let the runtime apply the template. Do not hand-insert the special tokens. System, user, and assistant roles all work: keep system for standing role and format, user for the task. Breaking the template manually degrades output.

---

## 4. Few-shot examples work

Qwen responds well to few-shot examples for format, classification, and style. Give three to five diverse input-output pairs wrapped in clear delimiters, then end with the real input. This is one of Qwen's strengths relative to reasoning-first models.

---

## 5. Name the output language

Qwen is trained across 100+ languages and is strong in Chinese. With mixed-language input or an unstated preference, output can drift toward Chinese. State the target language explicitly on every multilingual task ("Answer in English.").

---

## 6. Long context: question after the material

Qwen2.5 and Qwen3 carry long context (32K native, extensible toward 128K). Put large source material before the question and place the task instruction at the end, so the request is freshest at generation.

---

## 7. Structured output and tables

Qwen handles structured data well. Give input tables as markdown, and specify the output schema exactly: columns, JSON keys, enum values. For strict JSON, say "Return only valid JSON, with no surrounding text."

---

## 8. Coding

Qwen3-Coder is the code specialist. Qwen3-Instruct also codes well. Specify language, version, framework, and expected style. For debugging, give the failing input, the expected output, and the actual output.

---

## 9. Anti-preamble

Qwen2.5 commonly opens with "Sure, here is..." or restates the task. Add: "Begin your response directly with the output. Do not restate the task." In thinking mode the reasoning trace is separate. Keep the instruction aimed at the final answer.

---

## 10. Multimodal and Tool Use for Qwen

### Images and video

Images require a vision variant (Qwen2.5-VL or Qwen3-VL), not the text Qwen models. These read images, documents, and video, and can return bounding boxes and point coordinates for elements they locate.

- Target the VL variant explicitly when an attachment is involved. A text Qwen model will invent the contents.
- Apply the shared multimodal framework: extract first, reason second.
- For video, state the unit of output: transcript, timestamped summary, or per-scene analysis.

### Function calling

Qwen2.5 and Qwen3 support tool use and JSON output, exposed through an OpenAI-compatible `tools` interface and the Qwen-Agent framework. Qwen3-Coder is tuned for agentic coding loops.

- Define each tool with a name, a description stating when to call it, and typed parameters.
- Qwen can use tools in both thinking and non-thinking modes. In thinking mode the model may reason before emitting the call.
- Constrain parameters with enums where the domain is fixed, and keep the tool set non-overlapping.

---

## 11. One-shot prompt template for Qwen

**Thinking target (append the soft switch):**

```
/think
You are [specific role].

[Source material, if any.]

Task: [Direct instruction: the problem to solve and the standard it must meet.]

Output format: [Structure, length, format.]

Answer in [language]. Begin the final answer directly, with no preamble.
```

**Fast target (append the soft switch):**

```
/no_think
You are [specific role]. [One sentence on purpose and audience.]

[Source material, if any.]

Task: [Direct instruction.]

Output format: [Structure, length, format.]

Answer in [language]. Begin directly with the output.
```

# Prompt Engineering Knowledge Base: DeepSeek-Specific Techniques

## Purpose

This document covers techniques specific to DeepSeek models: the DeepSeek-V3/V4 chat series (fast general instruction models) and the DeepSeek-R1 reasoning series (models that reason internally before answering). Load this alongside the shared base KB when the target is DeepSeek. The two families need opposite handling: prompt the reasoning models sparingly, the chat models explicitly.

---

## 1. Reasoning vs non-reasoning: establish which one you have

DeepSeek ships two behaviours under one name.

- **Reasoning models** (the R1 series and thinking modes) emit a reasoning block before the answer and decide for themselves how much to reason.
- **Chat models** (V3/V4 non-thinking) answer directly.

Identify the target before writing. The sections below state which techniques apply to which family.

---

## 2. Do not add chain-of-thought to a reasoning model

Reasoning models already reason step by step internally. Adding "think step by step", "show your reasoning", or an explicit reasoning scaffold duplicates the work, lengthens the trace, and can lower quality.

- **Reasoning target:** state the problem and the required output. Let the model choose its reasoning.
- **Chat target:** chain-of-thought ("reason through this before answering") helps on analytical and decisioning tasks.

---

## 3. No system prompt for R1: keep instructions in the user turn

DeepSeek's guidance for the R1 reasoning series is to skip the system prompt and place all instructions in the user message. A system prompt adds no reliability and can distract the reasoning. Write one self-contained user message.

For V3/V4 chat models, the system role works normally.

---

## 4. Zero-shot first for reasoning models, few-shot degrades R1

DeepSeek-R1 performs best given a clear zero-shot problem statement. Few-shot examples consistently degrade R1's performance. Leave them out of reasoning prompts. On V3/V4 chat models, few-shot examples are effective for format and style.

---

## 5. Name the output language

DeepSeek is trained bilingually (English and Chinese). Under an ambiguous prompt the output can drift toward Chinese, especially when source material is Chinese. State the output language explicitly on every multilingual task, for example "Respond in English."

---

## 6. Specify the final-answer shape for reasoning models

A reasoning model's visible output is the final answer. The reasoning trace is separate. Still, specify the exact final-answer format (sections, table, word count) so the model exits reasoning into the right shape. If a deployment echoes the trace into the response, instruct: "Return only the final answer, without the reasoning."

---

## 7. Structure with clear sections

Chat models respond well to labelled sections. Use a lightweight convention (`TASK`, `CONTEXT`, `SOURCE`, `OUTPUT FORMAT`) separated by blank lines or `---`. DeepSeek handles markdown well, including tables and code fences.

---

## 8. Long context: material first, question last

DeepSeek handles long inputs, but attention is strongest near the end. For prompts carrying large source material:

1. Role and context (brief)
2. Source material (as long as needed)
3. Task instruction and output format (at the end)
4. Key constraint restated (optional)

---

## 9. Structured output and code

V3/V4 support JSON output and function calling. When a strict shape is needed, state the schema and say "Return only valid JSON, with no surrounding text." For code, name the language and version, the framework, and the style expected. DeepSeek is strong on code and maths but will choose its own conventions if you leave them open.

---

## 10. Sampling parameters

Set at the API, not in the prompt, but they change results:

- V3/V4 chat, general conversation: temperature 1.3.
- V3/V4 chat, coding and maths: lower (around 0.0-0.6) for exactness.
- R1 reasoning: temperature 0.6.

---

## 11. Anti-preamble

V3/V4 can open with a restatement ("Certainly, here is..."). Add: "Begin your response directly with the output." Reasoning models already separate reasoning from answer. Keep the instruction aimed at the answer.

---

## 12. Multimodal and Tool Use for DeepSeek

### Images

The DeepSeek-V3/V4 chat models and the R1 series are text-only. For image or document tasks, target a vision model in the DeepSeek-VL family instead, and apply the shared multimodal framework. Do not ask a text DeepSeek model to read an attachment. The model will invent the contents.

### Function calling

V3/V4 support function calling through an OpenAI-compatible `tools` interface, plus JSON output mode.

- Define each tool with a name, a description that states when to call it, and a parameter schema.
- Validate the returned arguments before executing. Treat DeepSeek tool calls as best-effort, not guaranteed well-formed.
- The R1 reasoning series is less reliable at tool calling than the chat models. Prefer a V3/V4 model for agentic loops and keep the reasoning model for self-contained analysis.

---

## 13. One-shot prompt template for DeepSeek

**Chat model (V3/V4):**

```
TASK
[Direct instruction: what to produce, for whom, and to what standard.]

CONTEXT
[Background and motivation, 1-3 sentences.]

SOURCE
[Source material, if any.]

OUTPUT FORMAT
[Structure, length, format, tone and register.]

Respond in [language]. Begin your response directly with the output.
```

**Reasoning model (R1 series), no system prompt, no few-shot:**

```
[State the problem and everything needed to solve it, in one clear message.]

[Source material, if any.]

Produce [exact final-answer format]. Return only the final answer.
```

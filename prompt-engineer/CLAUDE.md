# Prompt Engineering Knowledge Base: Claude-Specific Techniques

## Purpose

This document covers techniques specific to Claude models (Claude 3, Claude 3.5, Claude 4 series). Load this alongside the shared base KB when the target model is Claude or when no model has been specified (Claude is the default). These techniques reflect Claude's instruction-following patterns, training, and known behaviours.

---

## 1. XML Tag Structuring

Claude is trained on large volumes of structured web content and parses XML tags cleanly and reliably. Use XML tags to separate distinct sections of a prompt. This reduces ambiguity, improves instruction-following, and makes complex prompts easier to reason about.

**When to use XML tags:**
- When the prompt contains both instructions and source content (e.g. a document to analyse)
- When there are multiple distinct inputs (e.g. a current state and a target state)
- When few-shot examples need to be clearly delimited from instructions
- When the output should follow a specific structure

**Recommended tag conventions:**
- `<instructions>`: the core task directive
- `<context>`: background information or motivation
- `<document>` or `<content>`: source material the model should work from
- `<example>`: few-shot examples
- `<output_format>`: specification of the desired response structure
- `<audience>`: audience profile information

**Nesting:** Tags can be nested where logical. For example, wrap multiple examples in `<examples>` with individual `<example>` tags inside.

**Self-closing tags:** Not required. Open and close tags are sufficient.

**Do not over-tag:** Simple prompts do not need XML structure. Use tags when they reduce ambiguity, not as a default formatting habit.

---

## 2. Role Priming in One-Shot Prompts

In a one-shot prompt (no system prompt), the role must be established inline at the opening of the prompt. Claude responds well to specific, grounded role framing.

**Effective role framing:**
- Be specific about expertise, not the title alone. "You are an experienced executive communications writer who specialises in translating complex technical decisions into clear business narratives for senior leadership" is more effective than "You are a writer."
- Include the purpose of the role in context. "Your job in this task is to..." anchors the role to the specific output.
- Do not assign fictional or implausible roles. Claude responds better to grounded professional roles than to exaggerated personas.

**One-shot vs system prompt role placement:**
- One-shot: Role goes at the very top of the prompt, before any context or instructions.
- System prompt: Role is the foundation of the entire system prompt and shapes all subsequent instructions.

---

## 3. Long Context Positioning

When the prompt includes substantial source material (documents, transcripts, data), position matters significantly. Research indicates up to 30% quality improvement when positioning is correct.

**Rule:** Place source material and context near the top of the prompt. Place the task instruction and query at the bottom.

**Structure for long-context prompts:**
1. Role block (brief)
2. Context / motivation (brief)
3. Source material (as long as needed)
4. Task instruction (at the bottom, after the material)
5. Output format specification
6. Self-check instruction

This mirrors how a human expert would work: read the material first, then answer the question. Placing the question last keeps it fresh in context when Claude generates its response.

---

## 4. Positive Instruction Framing

Claude responds better to instructions that specify what to do rather than what not to do. Negative instructions are often less reliable and can produce unexpected interpretations.

**Prefer positive framing:**
- Instead of "don't use jargon" use "write in plain English accessible to a non-technical reader"
- Instead of "don't be verbose" use "write concisely, target 150 words or fewer"
- Instead of "don't start with a summary" use "begin directly with the first recommendation"

**When negative constraints are appropriate:**
Use negative constraints when there is a specific default behaviour you need to suppress that cannot be easily expressed as a positive instruction. For example: "Do not include a disclaimer or caveat at the end of your response" is difficult to reframe positively and is appropriate as written.

---

## 5. Prompt Style Mirrors Output Style

Claude tends to match the style and format of the prompt in its output. This is a useful property to exploit deliberately.

- A prompt written in dense prose with no structure tends to produce prose output.
- A prompt written with clear headers and bullet points tends to produce structured output.
- A prompt written in a formal register tends to produce formal output.

**Practical application:** Write the prompt in the style and register you want the output to adopt. If you want a bullet-point executive summary, structure the prompt instructions using bullet points. If you want flowing prose, write the instructions in flowing prose.

---

## 6. Anti-Preamble Instructions

Claude has a tendency to open responses with a restatement of the task, a brief affirmation ("Certainly!"), or a meta-comment about what it is about to do. For most professional prompt outputs, this preamble reduces quality.

**Standard anti-preamble instruction to include:**
"Begin your response directly with the output. Do not restate the task, open with a greeting, or include any meta-commentary about what you are about to produce."

Place this instruction immediately before or after the output format specification.

---

## 7. Self-Check Instructions

Appending a self-check instruction at the end of the prompt improves output quality on complex tasks. Claude will review its own draft against the specified criteria before finalising its response.

**Effective self-check framing:**
"Before submitting your response, verify that: (1) [criterion one], (2) [criterion two], (3) [criterion three]. If any criterion is not met, revise before responding."

Make the criteria specific and binary where possible. "The output is under 200 words" is a better criterion than "the output is concise."

**When to include a self-check:**
- Analytical tasks where completeness matters
- Tasks with strict format requirements
- Tasks where a specific audience constraint must be honoured throughout
- Tasks where omission of a required element would make the output unusable

---

## 8. Thinking Vocabulary

When extended thinking is not enabled (the default in most deployments), avoid using the word "think" as an instruction verb. Claude may interpret this inconsistently.

**Preferred alternatives:**
- "reason through"
- "consider"
- "work through"
- "analyse"
- "evaluate step by step"

**Example:** Instead of "think carefully about the risks before listing them," use "reason through the risks systematically before listing them."

---

## 9. Context Window and Content Placement Summary

For reference when constructing prompts that will include large source documents:

- Place role and context at the top (brief)
- Place all source material in the middle (as long as needed)
- Place task instruction, output format, and self-check at the bottom
- The closer the task instruction is to the end of the prompt, the more reliably Claude follows it when processing long content

---

## 10. Multimodal and Tool Use for Claude

### Images and documents

Claude 3, 3.5, and 4 are vision-capable and read text, charts, screenshots, and scanned documents reliably.

- Pass images as content blocks (base64 or URL in the API, or attachments in the app), separate from the text.
- Place the image before the text that refers to it, so the reference resolves against content already in context.
- For multiple images, label each one ("Image 1: current dashboard, Image 2: target") and refer to them by label throughout.
- Wrap each attachment in its own tag when there are several: `<image_1>…</image_1>`, `<document_1>…</document_1>`.

### Tool use

Claude has native tool use. Define each tool with a `name`, a `description`, and a JSON Schema `input_schema`, and the model then emits a structured tool call instead of guessing.

- Write the description as a decision rule: what the tool does, when to call the tool, and when not to.
- State plainly that tools are available. Claude is conservative and might otherwise answer from memory.
- Keep schemas strict and enums closed, so the model cannot emit invalid arguments.
- Use `tool_choice` to force a specific tool when the decision is not the model's to make.

---

## 11. One-Shot Prompt Template for Claude

Use this structure as the default template when generating a one-shot Claude prompt. Omit elements that are not needed for the specific task.

```
<instructions>
You are [specific role with grounded expertise]. [One sentence on the purpose of this task and what the output will be used for.]

The audience for your output is [audience description: expertise level, relationship to output, what they care about].

[Core task instruction in direct, imperative language.]

[Constraints: positive framing where possible.]

[Output format specification: structure, length, format.]

Tone and register: [specify].

Begin your response directly with the output. Do not restate the task or include any preamble.

Before submitting, verify: [self-check criteria].
</instructions>

<content>
[Source material if applicable]
</content>
```

For prompts without source material, omit the `<content>` block and place all elements within `<instructions>`.

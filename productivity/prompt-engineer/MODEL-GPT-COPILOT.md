# Prompt Engineering Knowledge Base: ChatGPT and Microsoft Copilot

## Purpose

This document covers techniques specific to GPT-based models, including ChatGPT (GPT-4, GPT-4o, GPT-4.1 series) and Microsoft Copilot (which runs on GPT foundations within Microsoft 365). Load this alongside the shared base KB when the target model is ChatGPT or Microsoft Copilot.

A Copilot-specific addendum is included at the end of this document covering behaviours and constraints unique to the Microsoft 365 Copilot product.

---

## 1. Instruction Placement and Recency Bias

GPT models are susceptible to recency bias: content appearing later in the prompt can exert more influence over the output than content appearing earlier. This is the inverse of Claude's long-context positioning behaviour.

**Practical rules:**
- Place the primary task instruction at the beginning of the prompt, before providing context or source material.
- Repeat key instructions at the end of the prompt to reinforce them against recency bias.
- For short prompts, a single placement at the top is sufficient.
- For long prompts containing substantial source material, restate the core directive at the bottom after the material.

**Structure for long-context GPT prompts:**
1. Task instruction (at the top)
2. Context / motivation
3. Source material
4. Restatement of key instructions (at the bottom)
5. Output format specification

---

## 2. Section Separators and Syntax

GPT models respond well to clear visual separation between prompt sections. The `---` separator is idiomatic and effective. Markdown and XML both work. `---` with uppercase section labels is a common and reliable convention.

**Effective separator pattern:**
```
TASK
---
[Task instruction here]

CONTEXT
---
[Background information here]

SOURCE MATERIAL
---
[Document or data here]

OUTPUT FORMAT
---
[Format specification here]
```

**Uppercase section labels** help the model distinguish sections when processing long prompts. This is particularly effective for GPT-4 and GPT-4o.

---

## 3. Output Priming (Cue Technique)

Including a few words or a partial phrase at the end of the prompt to "prime" the output format is a reliable GPT technique. The model will continue from where the cue leaves off.

**Examples:**
- To get a bullet list: end the prompt with `Key points:\n-`
- To get a numbered plan: end with `30-60-90 Day Plan:\n\n**Days 1-30:**`
- To get a specific opening: end with `Executive Summary:\n\nThe`
- To get a structured table: end with `| Risk | Likelihood | Impact | Mitigation |\n|---`

**When to use output priming:**
- When the output format is specific and must start in a particular way
- When few-shot examples are impractical due to length
- When you want to lock the first element of a structured output

---

## 4. Providing a Fallback ("Give the Model an Out")

GPT models can hallucinate when asked to produce content they cannot reliably generate from the provided material. Including a fallback instruction reduces fabrication.

**Standard fallback framing:**
"If the source material does not contain sufficient information to answer a specific point, state 'Insufficient information provided' rather than inferring or fabricating."

**Alternative formulations:**
- "If you are not certain, flag the item with [NEEDS VERIFICATION] rather than guessing."
- "If a required section cannot be completed from the provided content, write 'Not addressed in source material' and move on."

Include a fallback instruction on any task where the model is working from provided source material rather than its own training knowledge.

---

## 5. Few-Shot Learning for GPT

GPT models benefit significantly from few-shot examples, particularly for classification, formatting, and style tasks. Few-shot examples are provided as input-output pairs before the actual task.

**Format for few-shot examples in GPT prompts:**
```
EXAMPLES
---
Input: [example input one]
Output: [desired output one]

Input: [example input two]
Output: [desired output two]

---
Now complete the following:
Input: [actual input]
Output:
```

**Notes:**
- Three to five examples is typically optimal. More examples improve consistency but increase token cost.
- Ensure examples are diverse, not slight variations of the same case.
- The final `Output:` with no content primes the model to continue in the demonstrated format.

---

## 6. System Message vs User Prompt

When prompting GPT via an interface with a system message (such as the API, Azure OpenAI, or Copilot Studio bots), the system message and user message serve different purposes.

**System message:** Persistent role, persona, standing instructions, constraints, and output format preferences. Written as standing directives, not task-specific commands. Does not contain the actual request.

**User message:** The specific task for this interaction. Should be focused and relatively brief if the system message has established context.

**For one-shot prompts** (no system message available, single text input): Combine all elements into the user message. Use section separators to distinguish role, context, task, and format.

---

## 7. Breaking Complex Tasks into Steps

GPT models perform better on complex tasks when the task is decomposed into explicit sub-steps within the prompt. Rather than asking for a complex output in a single instruction, sequence the reasoning.

**Example decomposition:**
Instead of: "Analyse this document and produce a risk register."

Use:
```
Step 1: Extract all statements that describe potential risks, uncertainties, or dependencies from the document below.
Step 2: For each risk identified, assess likelihood (High/Medium/Low) and impact (High/Medium/Low).
Step 3: Produce a risk register table with columns: Risk, Likelihood, Impact, Mitigation.
```

This technique is particularly effective for analytical and classification intent types.

---

## 8. Chain-of-Thought for GPT

Chain-of-thought prompting instructs the model to show its reasoning before producing the final output. This is applicable to non-reasoning GPT models (GPT-4o, GPT-4.1). Do not use this technique with reasoning models (o1, o3, o4 series) as they reason internally.

**Standard CoT instruction:**
"Before producing your final output, work through the following step by step: [reasoning steps]. Then provide your final answer."

**Lightweight CoT:**
"Take a step-by-step approach. Show your reasoning before giving your final answer."

---

## 9. Grounding and Citation Instructions

When GPT is working from provided source material, instruct it to cite or ground its statements in that material. This reduces hallucination and increases output reliability.

**Grounding instruction:**
"Base all statements in your response on the source material provided. Where you quote or paraphrase specific content, indicate the source with [Source: paragraph/section X]."

**Inline citation instruction:**
"When making a specific claim, include an inline citation immediately after the claim rather than in a footnote. Inline citations reduce the likelihood of unsupported statements."

---

## 10. Space Efficiency and Token Awareness

GPT models process text as tokens. Some formatting choices are more token-efficient than others.

**Practical guidance:**
- Tables are more token-efficient than repeating field labels in prose for structured data.
- Avoid consecutive whitespace, which consumes tokens without adding meaning.
- Spell out month names rather than using numeric date formats. Numeric dates tokenise less efficiently.
- Keep prompt structure tight. Unnecessary repetition within the prompt consumes context window.

---

## 11. Multimodal and Tool Use for GPT

### Images

GPT-4o and GPT-4.1 are vision-capable. In the API, an image is passed as an `image_url` content part with a `detail` setting (`low`, `high`, or `auto`). In ChatGPT, images are attached directly.

- Set `detail: "high"` when the task depends on small text, fine detail, or precise counts. Leave `auto` otherwise.
- Describe what to read before what to do: extraction first, reasoning second (see the multimodal framework).
- For several images, label them and refer to them by label. GPT will otherwise mix them.
- Say what to do when an element is unreadable rather than letting the model fill the gap.

### Function calling

GPT supports function calling with JSON Schema definitions passed in `tools`.

- Each function needs a clear `name`, a `description` that states when to call it, and a parameter schema.
- Set `strict: true` on the schema to guarantee the arguments validate. Without strict mode, the model emits malformed or invented fields.
- Use `tool_choice` (`auto`, `required`, or a named function) to control whether and which tool is called.
- Keep the tool set small and non-overlapping. Describe each tool's trigger conditions rather than relying on the name.
- The model returns a `tool_calls` object with JSON arguments. The caller executes the call, not the model.

Microsoft 365 Copilot does not expose raw function calling. Copilot uses connectors and plugins configured in Copilot Studio. Apply the Copilot addendum below for those surfaces.

---

## COPILOT-SPECIFIC ADDENDUM

Microsoft Copilot runs on GPT foundations but operates within the Microsoft 365 ecosystem. The following additional considerations apply when the target is Microsoft Copilot (M365 Copilot, Copilot Chat, Copilot in Teams, Word, Outlook, etc.).

### C1. Reference the Work Context Explicitly

Copilot has access to the user's Microsoft 365 data: emails, documents, calendar, Teams conversations, and SharePoint content. Prompts should reference this context explicitly to help Copilot locate and use the right information.

**Effective Copilot framing:**
- "Based on the email thread I have open..." (Copilot in Outlook)
- "Using the document I am currently editing..." (Copilot in Word)
- "From the Teams meeting that just ended..." (Copilot in Teams)
- "Referencing the files in [SharePoint folder name]..." (Copilot Chat)

Prompts that do not reference the work context will cause Copilot to use general knowledge rather than the user's actual content.

### C2. Keep Scope Tight

Copilot's effective prompt window in conversational interfaces (Teams, Copilot Chat) is shorter than the raw GPT-4 context window. Prompts that are overly long may be truncated or produce lower-quality responses.

**Recommendation:** Keep one-shot Copilot prompts under 400 words where possible. Use the output priming technique and section separators to compensate for reduced length.

### C3. Copilot Output Tends Toward Conciseness

Copilot is tuned for productivity use cases and tends to produce shorter, more concise outputs than raw ChatGPT. If a longer or more detailed output is needed, explicitly specify the desired length and depth.

**Example:** "Produce a detailed analysis of at least 500 words covering each of the following points in turn: [points]."

### C4. Copilot in Specific M365 Applications

Different Copilot surfaces have different effective behaviours:

- **Copilot in Word:** Best for drafting, rewriting, and summarising within a document. Reference specific sections of the open document.
- **Copilot in Outlook:** Best for drafting replies, summarising threads, and extracting action items. Reference the specific email or thread.
- **Copilot in Teams:** Best for meeting summaries, action item extraction, and follow-up drafts. Reference the specific meeting.
- **Copilot Chat:** Most general-purpose surface. Supports broader queries but benefits most from explicit file and context references.

### C5. Copilot System Prompts (Copilot Studio Bots)

When building a Copilot Studio bot, the system prompt is configured in the bot's topic or knowledge settings rather than in the user message. Apply the GPT system message guidance from section 6 above. Keep the system prompt focused. Copilot Studio bots perform better with clear, constrained system instructions than with broad open-ended ones.

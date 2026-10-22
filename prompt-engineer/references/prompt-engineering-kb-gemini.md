# Prompt Engineering Knowledge Base: Gemini-Specific Techniques

## Purpose

This document covers techniques specific to Google Gemini models (Gemini 1.5, Gemini 2.0, Gemini 2.5 series), including Gemini in Google Workspace (Docs, Gmail, Meet, Sheets). Load this alongside the shared base KB when the target model is Gemini.

---

## 1. Direct Commands with Action Verbs

Gemini responds reliably to direct, imperative instructions that open with a clear action verb. Vague or open-ended prompts produce lower-quality outputs.

**Preferred action verb openings:**
- "Summarise..."
- "Analyse..."
- "Write..."
- "Classify..."
- "Compare..."
- "Extract..."
- "Generate..."
- "Identify..."
- "Rewrite..."
- "Create..."

**Effective pattern:** Action verb + subject + scope + constraints.

Example: "Summarise the key risks in the project brief below in five bullet points, using plain English accessible to a non-technical executive."

Rather than: "Can you look at this project brief and maybe pull out some of the risks?"

---

## 2. Explicit Output Specification

Gemini benefits from explicit specification of both the desired length and the desired structure. Unlike Claude, which infers format from prompt style, Gemini performs more reliably when format is stated directly.

**Always specify:**
- Desired output length (word count, number of bullet points, number of sections, page length)
- Output structure (prose, bullet list, numbered list, table, structured sections with headings)
- Level of detail (high-level overview vs. detailed analysis)

**Examples:**
- "Produce a structured report with four sections: Executive Summary, Key Findings, Risks, and Recommended Actions. Each section should be 100-150 words."
- "Write a bulleted list of exactly five themes, each theme stated in one sentence."
- "Generate a table with three columns: Action Item, Owner, and Due Date. Include a row for each action identified."

---

## 3. Context and Background Before the Task

Gemini performs better when context and background information is placed before the task instruction. This is consistent with how Gemini processes input: understanding the context first, then applying the instruction.

**Recommended structure:**
1. Role / persona (brief)
2. Context and background
3. Source material (if applicable)
4. Task instruction
5. Output format specification
6. Constraints

This contrasts with GPT's instruction-first approach. For Gemini, front-loading context improves output relevance and reduces the need for fallback instructions.

---

## 4. Zero-Shot Chain-of-Thought

Gemini responds well to zero-shot CoT prompting: asking the model to reason step by step without providing the reasoning steps explicitly. This often produces better analytical output than specifying the steps yourself.

**Standard zero-shot CoT instruction for Gemini:**
"Reason through this step by step before producing your final answer."

Or, appended after the task instruction:
"Work through your reasoning systematically before providing your response."

**When to use:**
- Analytical and decisioning intent types
- Tasks where the reasoning process is as important as the conclusion
- Tasks where errors in intermediate steps would produce wrong final answers

**Note:** For Gemini 2.5 and later models with built-in thinking capability, zero-shot CoT may be handled internally. The instruction still helps orient the model's output structure even when thinking is internal.

---

## 5. Structured Data Handling

Gemini handles tabular and structured input data cleanly. When source material is structured (spreadsheets, tables, lists of records), present it in its natural structure rather than converting it to prose.

**Preferred input formats for structured data:**
- Markdown tables
- CSV-style rows with clear column headers
- Numbered or bulleted lists with consistent field labels

**Example prompt pattern for structured data:**
```
Analyse the following customer complaint data and identify the top three recurring issues by frequency.

| Date | Category | Description | Severity |
|------|----------|-------------|----------|
| ... | ... | ... | ... |

Produce a summary table ranking the top three issues with: Issue, Frequency, and Recommended Action.
```

Gemini will interpret the table structure and use it reliably in its analysis.

---

## 6. Specificity Over Ambiguity

Gemini's output quality degrades more noticeably than Claude or GPT when the prompt is ambiguous. Quantify requests wherever possible and break vague instructions into precise ones.

**Instead of vague:** "Write something about the project status that covers the main points."

**Use precise:** "Write a 150-word project status update covering: (1) current completion percentage, (2) the single most critical risk, and (3) the next milestone and its due date. Audience: project steering committee."

**Quantification examples:**
- "List five recommendations" not "list some recommendations"
- "Summarise in three sentences" not "write a brief summary"
- "Identify the top three themes" not "identify the main themes"

---

## 7. Multimodal Awareness

Gemini is natively multimodal and can process images, PDFs, audio, and video alongside text. When the prompt involves non-text content, explicitly describe the modality and what the model should do with it.

**Text prompts referencing attached content:**
- "Analyse the attached architecture diagram and identify any components that violate a microservices design pattern."
- "Review the PDF document attached and extract all action items assigned to named individuals."
- "From the spreadsheet attached, identify any rows where the budget column exceeds the approved threshold."

**When writing a prompt that will include attached content:**
- Reference the attachment explicitly by type (image, document, spreadsheet, audio file)
- Specify what the model should extract, analyse, or produce from it
- State what the model should do if the content is unclear or incomplete

---

## 8. Gemini API: System Instructions Field

When using Gemini via the API or in Google AI Studio, system instructions are passed as a separate field distinct from the user prompt. This is similar to the GPT system message.

**System instructions field:** Use for persistent role, persona, output format preferences, and standing constraints. Written as standing directives.

**User prompt field:** The specific task or question for this interaction.

**For one-shot prompts in consumer interfaces (Gemini.google.com, Gemini in Workspace):** There is no separate system instruction field. Combine role, context, task, and format in the single user message. Use clear section labels and direct language.

---

## 9. Gemini in Google Workspace

When the target is Gemini within a specific Google Workspace application, the following surface-specific guidance applies.

**Gemini in Google Docs:**
Best for drafting, rewriting, summarising, and expanding content within a document. Reference the specific document section where relevant. Use direct drafting instructions ("Write a new introduction paragraph for this document that...").

**Gemini in Gmail:**
Best for drafting replies, summarising email threads, and adjusting tone of existing drafts. Reference the specific email or thread explicitly. Keep prompts concise; Gmail Gemini is optimised for short, action-oriented tasks.

**Gemini in Google Sheets:**
Best for formula generation, data analysis, and table manipulation. Reference specific columns or ranges explicitly. Gemini can generate formulas from natural language descriptions of what the formula should calculate.

**Gemini in Google Meet:**
Best for meeting summaries, action item extraction, and follow-up email drafts after a meeting. Reference the meeting that just concluded.

---

## 10. Function Calling and Tool Use for Gemini

Gemini supports function calling: tools are declared (as `function_declarations`) and the model returns a structured call with arguments, which the caller executes.

- Each declaration needs a `name`, a `description`, and typed parameters. Write the description as a decision rule — what the tool does and the conditions that trigger it.
- Gemini can return several function calls at once and can chain calls across turns; state whether parallel calls are wanted.
- Use the system instruction field (section 8 above) to carry the standing tool-use policy, keeping the user prompt on the task.
- Keep declarations few and non-overlapping; where the domain is fixed, constrain parameters with enums.
- For image or document tools, state the modality explicitly and what should be extracted (see section 7).

---

## 11. One-Shot Prompt Template for Gemini

Use this structure as the default template when generating a one-shot Gemini prompt. Omit elements not needed.

```
You are [specific role with grounded expertise]. [One sentence on the purpose of this task and what the output will be used for.]

Context: [Background information relevant to the task. 2-3 sentences.]

The audience for your output is [audience description: expertise level, relationship to output, what they care about].

[Source material if applicable, in its natural structure.]

Task: [Direct imperative instruction beginning with an action verb. Specific and unambiguous.]

Output format: [Explicit structure, length, and format specification.]

Tone and register: [specify].

Constraints: [Any additional constraints, stated positively where possible.]

Reason through this step by step before producing your final output. Begin your response directly with the output.
```

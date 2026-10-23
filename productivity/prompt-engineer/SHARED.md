# Prompt Engineering Knowledge Base: Shared Foundations

## Purpose

This document provides the shared foundational frameworks used by the Prompt Engineer agent regardless of target model. Every section is applied on every prompt generation task. Model-specific techniques are loaded from the relevant model KB file in addition to this document.

---

## 1. Intent Taxonomy

Classify the user's request into one of the following intent types before selecting techniques. The classification determines which prompt engineering patterns to prioritise.

**Analytical / Reasoning**
The prompt must get the model to examine, evaluate, compare, or diagnose something. Output is structured insight or judgement.
Recommended techniques: chain-of-thought, step-back prompting, explicit reasoning instructions, structured output format, self-check.
Examples: gap analysis, risk assessment, root cause analysis, architectural review.

**Creative / Generative**
The prompt must get the model to produce original content: narratives, communications, plans, or written artefacts.
Recommended techniques: persona priming, tone and register direction, negative constraints on clichés, few-shot examples of style, output length guidance.
Examples: change communications, executive narratives, stakeholder updates, policy drafts.

**Extraction / Summarisation**
The prompt must get the model to distil, consolidate, or surface key information from source material.
Recommended techniques: grounding instructions (draw only from provided content), XML or structured output tags, explicit signal-to-noise direction, output format specification.
Examples: executive summaries, theme synthesis, meeting distillation, weekly wrap-ups.

**Classification / Decisioning**
The prompt must get the model to categorise, rank, score, or recommend from a defined set.
Recommended techniques: few-shot examples with correct classifications, enum output constraints, explicit decision criteria, confidence or rationale field in output.
Examples: risk registers, option papers, prioritisation frameworks, vendor assessments.

**Instructional / Procedural**
The prompt must get the model to produce a structured plan, roadmap, process, or set of steps.
Recommended techniques: numbered output format, milestone or phase structure, per-phase validation criteria, sample questions, explicit scope limits, owner and success criteria fields.
Examples: 30-60-90 day plans, onboarding plans, remediation roadmaps, implementation guides.

**Audience Reframing**
The prompt must get the model to take existing content and reframe it for a different audience, register, or purpose.
Recommended techniques: explicit audience profile in prompt, register and vocabulary constraints, what-to-include and what-to-exclude instructions, before/after framing if helpful.
Examples: technical-to-executive translation, legal-to-plain-English, client-facing post-mortems.

---

## 2. Audience Profiling Framework

Profile the intended audience of the prompt's output across five dimensions before writing. Embed the relevant findings directly into the generated prompt.

**Dimension 1: Domain expertise level**
- Novice: No assumed knowledge, all terms explained, analogies preferred.
- Practitioner: Working knowledge of the domain, jargon acceptable, detail welcome.
- Expert: Deep domain knowledge, precision expected, no hand-holding.

**Dimension 2: Relationship to the output**
- Consumer: Reads the output to be informed. Brevity and clarity are paramount.
- Decision-maker: Reads the output to act. Recommendation and rationale must be front-loaded.
- Co-creator: Uses the output as a starting point. Structure and editability matter.
- Critic: Reviews the output for quality or compliance. Completeness and rigour are paramount.

**Dimension 3: Assumed prior context**
What does this audience already know about the topic? What must be explained? What can be assumed? This determines how much scene-setting the prompt should instruct the model to include.

**Dimension 4: What they care about most**
- Accuracy and completeness
- Brevity and speed of consumption
- Actionability and next steps
- Creative quality and persuasiveness
- Compliance and rigour

**Dimension 5: Register**
- Formal: Board papers, legal documents, regulatory submissions.
- Professional: Executive communications, client-facing documents, project reports.
- Conversational: Team updates, internal messaging, coaching content.
- Technical: Architecture documents, engineering specs, API documentation.

---

## 3. Prompt Anatomy Framework

A complete prompt may contain up to ten elements. Not all are required for every task. Select the elements that add value for the specific intent type and audience. Do not include elements that add length without adding precision.

**Element 1: Role / Persona block**
Establishes who the model is in this context. Use "You are..." framing. Be specific about expertise, perspective, and purpose. Required for most tasks. Skip only for the simplest extraction tasks.

**Element 2: Context and motivation block**
Explains why the task matters and what the output will be used for. Models produce better outputs when they understand the purpose. Keep this concise, one to three sentences.

**Element 3: Audience direction**
Explicitly states who the output is for, their expertise level, their relationship to the output, and what they care about. This is distinct from the role block. The role is who the model is. The audience is who the model is writing for.

**Element 4: Task instruction**
The core directive. Written in clear, direct, imperative language. Specific and unambiguous. This is the most important element. If only one element is present, it must be this one.

**Element 5: Constraints and negative constraints**
What must be included, excluded, or limited. State positive constraints as positive instructions ("write in flowing prose") rather than prohibitions ("do not use bullet points") wherever possible. Negative constraints are appropriate for things the model might include by default that are unwanted (e.g. "do not include preamble", "do not hedge with qualifiers").

**Element 6: Output format specification**
Specifies the structure, length, and format of the response. Be explicit: section headings, bullet lists, numbered steps, tables, word counts, or prose. Match the format to the audience's consumption pattern.

**Element 7: Tone and register**
Specifies the voice, style, and register of the output. This is separate from output format. Examples: authoritative and direct, reassuring and practical, precise and neutral, engaging and accessible.

**Element 8: Success criteria / self-check instruction**
A statement the model uses to evaluate its own output before responding. Appended at the end of the prompt. Example: "Before responding, verify your output addresses all three questions, contains no jargon, and fits within 200 words."

**Element 9: Few-shot examples**
One to five examples of the desired input-output pattern. Use when the output format or style is difficult to specify in words alone. Wrap examples clearly to distinguish them from instructions.

**Element 10: Output language**
The language the response must be written in. Default to the language of the user's request. State it explicitly whenever the source material, the audience, or the target model's training could pull the output toward another language (for example, Chinese source material with a DeepSeek or Qwen target, or a bilingual audience). One short line: "Respond in English."

---

## 4. Quality Checklist

Before outputting the final prompt, verify each of the following. Do not output a prompt failing any check.

1. Is the role clearly established and appropriate to the task?
2. Is the audience explicitly addressed, including expertise level and what they care about?
3. Is the core task instruction direct, specific, and unambiguous?
4. Are constraints stated as positive instructions wherever possible?
5. Is the output format explicitly specified?
6. Is the tone and register appropriate to the audience and purpose?
7. Would a capable model with no prior context produce the correct output from this prompt alone?
8. Is every element earning its place, where removal would make the prompt worse?
9. Is the output language stated when the audience or source material could pull the output toward another language?
10. Where applicable, for a multimodal task, does the prompt name the attachment and say what to extract?
11. Where applicable, for a tool or agent prompt, are the tool set, the use policy, and the stopping conditions explicit?
12. Do the prompt and your surrounding message obey the house style: no em dash, no banned word, no markdown, no asterisk?

---

## 5. Universal Techniques

These techniques apply across all target models and intent types. Apply them where they add value regardless of which model-specific KB is loaded.

**Few-shot examples**
Include three to five diverse examples when the desired output format or style is difficult to specify in words. More examples improve consistency. Fewer reduce prompt length. Wrap in clear delimiters.

**Chain-of-thought**
Instruct the model to reason through the problem before producing the final output. Use "reason through this step by step" or "work through the following before answering." Particularly valuable for analytical and decisioning intent types.

**Output format specification**
Always specify the output format explicitly. Do not leave it to the model to decide. Match the format to how the audience will consume it.

**Negative constraints**
Use sparingly and only when the model would include something by default that is unwanted. Prefer positive alternatives where possible.

**Context-motivation pairing**
Always pair constraints with a brief reason. "Write in plain English because the audience has no legal background" produces better compliance than "write in plain English" alone.

**Anti-preamble instruction**
For most tasks, instruct the model not to open with a restatement of the task, a greeting, or a meta-comment about what it is about to do. Example: "Begin your response directly with the output."

**Prompt length calibration**
Simple tasks need short prompts. Complex tasks need longer ones. Do not pad a simple prompt with unnecessary structure, and do not under-specify a complex one in the name of brevity.

**Output language**
Name the output language whenever more than one is plausible: bilingual source material, a multilingual audience, or a target model trained across many languages. Default to the language of the request and stay silent only when the default is obvious.

---

## 6. Multimodal Prompts

Apply this section when the task involves images, PDFs, audio, or video rather than text alone. Multimodal is a modifier on the chosen prompt type, not a separate type. The job is the same, direct attention and specify the output, but the source material is non-text and must be addressed explicitly.

**Name the modality and the attachment.** Refer to the content by what it is and, when there is more than one, by label: "the attached screenshot", "Image 2, the org chart", "the contract PDF". A model given several images will guess which matters. A label removes the guess.

**Separate reading from reasoning.** First instruct what to extract from the content, then the task that uses the extraction. This mirrors the human sequence: read the chart, then answer the question.

```
Step 1: Transcribe every axis label, series name, and visible value from the chart in Image 1.
Step 2: Using that data, identify the single largest month-over-month change and state its likely cause.
```

**Match the extraction instruction to the content type.**

- **Images with text** (screenshots, scans, whiteboards): instruct transcription explicitly, and ask the model to flag anything illegible rather than guess.
- **Charts and diagrams**: ask for structured extraction of axis labels, series, values, and node/edge relationships before any interpretation.
- **Documents and PDFs with layout**: state the reading order or which pages, tables, or sections matter.
- **Audio and video**: state the unit of output: full transcript, timestamped summary, or segment-by-segment breakdown.

**Give a fallback for illegible or missing content.** "If any label is unreadable, mark it [ILLEGIBLE] rather than inferring it." This prevents invented values in the extraction step, which then poison every downstream step.

**Label multi-item inputs.** For more than one image or file, number them and refer to them by number throughout the prompt.

**Multimodal one-shot template:**

```
You are [specific role].

Image 1: [what it is].
[Image 2: what it is, if any.]

Step 1: Extract [what to transcribe or read from the attached content. State how to handle illegible or missing parts.]
Step 2: [The task using the extraction.]

Output format: [Structure, length, format.]
Tone and register: [specify].

Respond in [language]. Begin your response directly with the output.
```

---

## 7. Tool, Agent, and Function-Call Prompts

Apply this section when the target is not a single text response but a model that selects tools or runs in an act-observe loop. Two artefacts exist and are built differently: the tool (or function) definition, and the agent system prompt.

### 7a. Tool / function definitions

The description field, not the tool name, is what the model reads when deciding to call the tool. Write it as an instruction to the model, not documentation for a human.

**Each tool definition states:**
- **Name**: `verb_noun`, snake_case, unambiguous, for example `search_docs`, `create_ticket`, `send_email`.
- **Description**: what the tool does, when to use the tool, when not to use the tool, and what the tool returns. Put the trigger conditions here: "Use this when the user asks about existing content. Do not use this to create new content."
- **Parameters**: every parameter with its type, meaning, constraints, and one concrete example. Mark required versus optional and give defaults.

**Design the set, not just the items.** Tools should be few and non-overlapping. Near-duplicate tools cause wrong selection. Prefer explicit enum values over free text when the domain is fixed.

```
{
  "name": "search_docs",
  "description": "Search the internal document store for passages matching a query. Use this when the user asks about information already present in company documents. Do not use this tool to create or edit documents. Returns up to 5 ranked passages with source IDs.",
  "parameters": {
    "query": "string: the search phrase, in natural language. Example: 'remote work policy 2026'.",
    "top_k": "integer, optional, default 5: maximum number of passages to return."
  }
}
```

### 7b. Agent system prompts

An agent system prompt governs a model that plans, calls tools, observes results, and repeats. It carries the standing policy, never the task content. The task belongs in the user turn.

**Every agent system prompt states:**
- **Role and objective**: who the agent is and the single outcome the agent exists to produce.
- **The loop**: plan, act, observe, decide, and how the agent knows the work is finished.
- **Tool-use policy**: when to call a tool versus answer from knowledge. Prefer retrieval over memory for factual claims.
- **Stopping conditions**: when to stop, the step limit, and when to ask the user instead of continuing.
- **Error handling**: what to do when a tool fails, times out, or returns empty.
- **Output contract**: the shape of the final answer the user will see.
- **Safety constraints**: what the agent must never do, and which actions require confirmation.

**Anti-patterns to avoid:** describing tools vaguely, instructing "use tools when helpful", omitting a stopping condition, embedding the current task in the system prompt, letting the tool set grow without pruning overlaps.

**Agent system-prompt skeleton:**

```
You are [role] and your objective is [single outcome].

Work in a loop: plan the next step, call a tool if needed, evaluate the result, then decide
whether the objective is met. Stop when [stopping condition], and never exceed [N] tool calls.

Use [tool] when [trigger]. Answer from your own knowledge only for [safe categories].
If a tool fails or returns nothing useful, [fallback behaviour]. Ask the user before [irreversible action].

Return your final answer as [output contract]. Never [prohibited behaviour].
```

---

## 8. Multi-Model Prompts

Apply this section when one prompt must serve several target models, or when the user wants to optimise across a set.

**Build on the intersection.** Use the most conservative positioning rule among the targets. A prompt that leads with the task and restates it at the end satisfies both instruction-first models (GPT) and context-first models (Gemini). Do not blend two model KBs' contradictory placement rules into one prompt.

**Keep syntax portable.** Model-specific syntax, such as XML tags and output priming cues, should be dropped unless every target supports them. Fall back to plain labelled sections (`TASK`, `CONTEXT`, `SOURCE`, `OUTPUT FORMAT`) when portability matters.

**Prefer explicit specification over style-mirroring.** Style-mirroring exploits a Claude-specific property. State the format directly so every target reaches the format.

**To compare models,** give each the identical, self-contained prompt and demand the same structured output, so the results are directly comparable. Vary nothing but the model.

**To optimise for one target,** load that model's KB and adapt the structure while preserving intent and content.

---

## 9. Prompt Critique and Variant Generation

Apply this section when the user asks to review, test, or improve an existing prompt, or wants options rather than a single prompt.

**Critique.** Run the prompt against the quality checklist and the anatomy. Report specific gaps, such as a missing audience, an unspecified format, an ambiguous task verb, or an element earning no place, not general impressions. Name the checklist item each finding violates.

**Improve.** Preserve the user's intent and structure. Change only what the critique identified. Do not silently rewrite unaffected parts.

**Variants.** Generate two or three candidates that differ on a single labelled axis, such as length, register, or strictness, state each variant's trade-off, and recommend one. Do not pick silently. The user chooses.

**Test plan.** Propose concrete inputs the user can run: a typical case, an edge case, and a case where the source material lacks the answer. For each, state the expected behaviour, so correctness is checkable rather than judged by feel.

---

## 10. House Style (mandatory)

The user's writing rules. They govern every prompt you generate and every message you write back. They outrank any conflicting convention elsewhere in this knowledge base. The rules are reproduced verbatim:

```
# FOLLOW THIS WRITING STYLE:

• SHOULD use clear, simple language.
• SHOULD be spartan and informative.
• SHOULD use short, impactful sentences.
• SHOULD use active voice; avoid passive voice.
• SHOULD focus on practical, actionable insights.
• SHOULD use bullet point lists in social media posts.
• SHOULD use data and examples to support claims when possible.
• SHOULD use "you" and "your" to directly address the reader.
• AVOID using em dashes (—) anywhere in your response. Use only commas, periods, or other standard punctuation. If you need to connect ideas, use a period or a semicolon, but never an em dash.
• AVOID constructions like "...not just this, but also this".
• AVOID metaphors and clichés.
• AVOID generalizations.
• AVOID common setup language in any sentence, including: in conclusion, in closing, etc.
• AVOID output warnings or notes, just the output requested.
• AVOID unnecessary adjectives and adverbs.
• AVOID hashtags.
• AVOID semicolons.
• AVOID markdown.
• AVOID asterisks.
• AVOID these words:
"can, may, just, that, very, really, literally, actually, certainly, probably, basically, could, maybe, delve, embark, enlightening, esteemed, shed light, craft, crafting, imagine, realm, game-changer, unlock, discover, skyrocket, abyss, not alone, in a world where, revolutionize, disruptive, utilize, utilizing, dive deep, tapestry, illuminate, unveil, pivotal, intricate, elucidate, hence, furthermore, realm, however, harness, exciting, groundbreaking, cutting-edge, remarkable, it, remains to be seen, glimpse into, navigating, landscape, stark, testament, in summary, in conclusion, moreover, boost, skyrocketing, opened up, powerful, inquiries, ever-evolving"

# IMPORTANT: Review your response and ensure no em dashes!
```

### Applying the house style

- **No em dashes, ever.** The sharpest rule. Replace an em dash with a period, a comma, or a colon. One em dash fails the output.
- **No banned words.** The list above is banned outright, including common filler such as "can", "may", "just", "that", "very", "actually", "however", and "it". Rephrase around each.
- **Plain text, not markup.** The generated prompt and every message you write back carry no markdown. No headings, no bold, no asterisks, no bullet markers, no horizontal rules.
- **Short, active, specific.** Simple words, active voice, concrete examples, direct address with "you".
- **No filler.** Cut setup phrases, warnings, notes, metaphors, clichés, generalizations, and unnecessary adjectives and adverbs.
- **Structure without markup.** Separate ideas with blank lines or short labelled lines. Write a list as plain hyphen lines, never asterisks.
- **When the target needs markup.** A prompt sometimes must pass literal markup to its target model, such as a JSON schema or XML tags. Keep that markup inside the prompt as content, and keep your own surrounding text plain. Never decorate the wrapper.
- **The confirmation block follows the style.** Plain labels, no bold, no asterisks.

Before you output, re-read both the prompt and your own message against this list. Any em dash, any banned word, or any markdown fails the output.

### Conflicts with other sections

Where a model template above shows markup for illustration, deliver the prompt without that markup. Where an output format genuinely needs structure, request the structure in plain words and let the target model produce it. House style wins every conflict.

---

## 11. Phased Objectives, Validation, and Sample Questions

Apply this section when a prompt must produce a plan with phases: a roadmap, an implementation plan, a rollout, a learning path, or any multi-stage objective.

### Every phase carries a validation criterion

Each phase must state how the user confirms the phase succeeded. A validation criterion is observable, binary, and testable. It is never a sentiment or a restatement of the objective.

- Weak: phase one lays the groundwork.
- Strong: phase one closes when all five onboarding accounts are active and the first staging deploy runs green.

Structure every phase with four parts:

- Objective: the outcome the phase produces.
- Steps: the actions that reach the objective.
- Validation criteria: the checks that prove the objective met.
- Sample questions: questions the user asks to test the phase.

### Generate sample questions

Always produce sample questions alongside a phased plan. They serve two purposes:

- Exercise the plan. Questions a user asks to confirm a phase delivered, for example: what is the rollback step if the deploy fails.
- Seed the test plan. Questions that reveal whether the prompt itself works, at least one per phase plus one edge case.

Pair each sample question with its expected answer, or with the expected behaviour when no single answer exists. A question without an expected result proves nothing.

### How the prompt requests this

Instruct the target model to deliver the plan with the four parts per phase, and to append a sample-questions list. Where the target supports a self-check, add: before you respond, confirm every phase has at least one testable validation criterion.

Keep the request in plain words that follow the house style. Do not use markup to convey the phase structure.

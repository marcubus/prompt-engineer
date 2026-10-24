# Gemini guidance

Load with SKILL.md when the target is Gemini.

## Action verbs

Open with a direct action verb: summarise, analyse, write, classify, compare, extract, generate, identify, rewrite, create.

Pattern: action verb, subject, scope, constraints.

Example: "Summarise the key risks in the brief below in five bullet points for a non-technical executive."

## Explicit output specification

State the length, structure, and detail level. Gemini follows explicit format instructions more reliably than inferred format.

- Length: word count, bullet count, section count, page count.
- Structure: prose, list, table, or sections with headings.
- Detail: overview or detailed analysis.

Example: "Produce four sections: Executive Summary, Key Findings, Risks, and Recommended Actions. Give each section 100 to 150 words."

## Context first

Place role, context, and source material before the task. Gemini reads context first, then applies the instruction. This differs from GPT.

## Structured data

Present tables and records in their natural structure. Markdown tables, CSV rows with headers, or lists with consistent labels all work. State the output schema exactly.

## Specificity

Quantify every request. "List five recommendations" beats "list some recommendations". "Summarise in three sentences" beats "write a brief summary".

## Multimodal

Gemini reads images, PDFs, audio, and video. Name the attachment and the modality. State what to extract, analyse, or produce. State what to do when the content is unclear.

## System instructions

The API and Google AI Studio take system instructions in a separate field. Consumer Gemini and Gemini in Workspace have no separate field, so combine role, context, task, and format in the user message.

## Workspace surfaces

- Docs: drafting, rewriting, summarising, and expanding inside a document.
- Gmail: replies, thread summaries, and tone adjustment.
- Sheets: formulas, data analysis, and table work.
- Meet: meeting summaries, action items, and follow-up drafts.

## Function calling

Declare tools as `function_declarations` with a name, description, and typed parameters. Write the description as a decision rule. Gemini returns one or more calls and chains calls across turns. State whether parallel calls are wanted. Keep declarations few and non-overlapping.

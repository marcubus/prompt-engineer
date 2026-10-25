# Gemini deltas

- Action verbs: open with a direct action verb: summarise, analyse, write, classify, compare, extract, generate, identify, rewrite, create. Pattern: action verb, subject, scope, constraints. Example: "Summarise the key risks in the brief below in five bullet points for a non-technical executive."
- Context first: place role, context, and source material before the task. Gemini reads context first, then applies the instruction. This differs from GPT.
- Multimodal: Gemini reads images, PDFs, audio, and video. Name the attachment and the modality. State what to extract, analyse, or produce. State what to do when the content is unclear.
- System instructions: the API and Google AI Studio take system instructions in a separate field. Consumer Gemini and Gemini in Workspace have no separate field. Combine role, context, task, and format in the user message.
- Workspace surfaces: Docs for drafting, rewriting, summarising, and expanding inside a document. Gmail for replies, thread summaries, and tone adjustment. Sheets for formulas, data analysis, and table work. Meet for meeting summaries, action items, and follow-up drafts.
- Function calling: `function_declarations` with a name, description, and typed parameters. Write the description as a decision rule. Gemini returns one or more calls and chains calls across turns. State whether parallel calls are wanted. Keep declarations few and non-overlapping.

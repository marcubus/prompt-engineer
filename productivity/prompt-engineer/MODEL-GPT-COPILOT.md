# GPT and Copilot deltas

- Recency: GPT models weight later content more heavily. Put the task first, then context and source material, then restate the key instruction at the end.
- Sections: separate with `---` and uppercase labels `TASK`, `CONTEXT`, `SOURCE MATERIAL`, `OUTPUT FORMAT`. Markdown and XML both work.
- Output priming: end the prompt with the first few words or the first row of the output. Use priming when the output must start in a fixed shape and few-shot examples would run long. Examples: `Key points:` then a new line and `-`. `30-60-90 Day Plan:` then `**Days 1-30:**`. A table header row then the separator row.
- Fallback: "If the source material does not contain enough information, write 'Insufficient information provided' rather than inferring or fabricating."
- Vision: GPT-4o and GPT-4.1 read images. Pass each image as an `image_url` content part. Set `detail: "high"` when small text or fine detail matters. Label multiple images.
- Function calling: `name`, `description`, parameter schema. `strict: true` guarantees valid arguments. `tool_choice` controls whether and which tool runs. The model returns `tool_calls` with JSON arguments. The caller executes the call.
- Copilot: name the work context, the open email, document, meeting, or SharePoint folder. Copilot otherwise falls back to general knowledge. Keep one-shot prompts under 400 words. State length and depth when a longer answer is needed. Match the surface: Word drafts and rewrites, Outlook replies and thread summaries, Teams meeting summaries and action items, Copilot Chat for broader questions. Copilot Studio bots take the system prompt in bot settings. Keep the prompt focused and constrained.

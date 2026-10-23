# ChatGPT and Copilot guidance

Load with SHARED.md when the target is ChatGPT or Microsoft 365 Copilot.

## Recency bias

GPT models weight later content more heavily. Put the task first, then context and source material, then restate the key instruction at the end.

## Section separators

Separate sections with `---` and uppercase labels: `TASK`, `CONTEXT`, `SOURCE MATERIAL`, `OUTPUT FORMAT`. Markdown and XML both work.

## Output priming

End the prompt with the first few words or the first row of the output.

- `Key points:` then a new line and `-`
- `30-60-90 Day Plan:` then `**Days 1-30:**`
- `| Risk | Likelihood | Impact | Mitigation |` then the separator row

Use priming when the output must start in a fixed shape and few-shot examples would be too long.

## Fallback

Add a fallback when the model works from source material.

"If the source material does not contain enough information, write 'Insufficient information provided' rather than inferring or fabricating."

## Task decomposition

Break a complex task into explicit steps inside the prompt. Number the steps and state the output of each step.

## Grounding

Instruct the model to base claims on the source material and to cite the paragraph or section inline.

## Vision

GPT-4o and GPT-4.1 read images. Pass each image as an `image_url` content part. Set `detail: "high"` when small text or fine detail matters. Label multiple images.

## Function calling

Define each function with a `name`, a `description` stating when to call the function, and a parameter schema. Set `strict: true` to guarantee valid arguments. Use `tool_choice` to control whether and which tool is called. The model returns `tool_calls` with JSON arguments. The caller executes the call.

## Copilot addendum

Microsoft 365 Copilot runs on GPT models inside the Microsoft 365 ecosystem.

- Name the work context: the open email, document, meeting, or SharePoint folder. Copilot otherwise falls back to general knowledge.
- Keep one-shot prompts under 400 words. Conversational Copilot windows are shorter than the raw GPT window.
- Copilot tends to answer briefly. State the length and depth when a longer answer is needed.
- Match the surface. Word drafts and rewrites. Outlook replies and thread summaries. Teams meeting summaries and action items. Copilot Chat for broader questions.
- Copilot Studio bots take the system prompt in the bot settings. Keep the prompt focused and constrained.

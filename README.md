# Prompt Engineer

A skill that turns a rough request into one finished, ready-to-use prompt for a chosen AI model. Ask for a prompt, and get a single usable result rather than a draft. Give it an existing prompt, and it critiques, tests, or improves the prompt against best practice.

Works as an OpenCode skill, and as an agent or Gem on Microsoft 365 Copilot, ChatGPT, Gemini, and Claude.

![Prompt Engineer flow: a rough request is analysed, checked for ambiguity, then generated into one finished prompt](prompt-engineer/prompt-engineer-flow.png)

## What it does

- Writes one-shot prompts, system prompts, user turns, and tool or agent definitions.
- Handles multimodal prompts for images, documents, audio, and video.
- Profiling for audience, intent, and the target model before writing.
- Critiques an existing prompt and names the exact gap behind each finding.
- Produces labelled variants with a stated trade-off and a recommendation.
- Requires a validation criterion and sample questions for phased objectives.
- Returns the finished prompt alone, with no preamble and no commentary.

## Supported models

| Target | Knowledge file |
|---|---|
| Claude (default) | `prompt-engineer/claude.md` |
| ChatGPT, Microsoft Copilot | `prompt-engineer/gpt-copilot.md` |
| Gemini | `prompt-engineer/gemini.md` |
| DeepSeek | `prompt-engineer/deepseek.md` |
| Qwen | `prompt-engineer/qwen.md` |

The shared file, `prompt-engineer/shared.md`, always loads. It carries the frameworks and the house style. Model files add the mechanics each family needs.

## How it works

1. Analyse the request. Resolve intent, audience, model, prompt type, and modality.
2. Check for ambiguity. When the request is unclear, show a short confirmation block and wait. When the request is clear, generate.
3. Generate the prompt from the matching model template, then run the quality checklist and the house style. When a prompt produces phased objectives, add a validation criterion and sample questions for each phase.
4. Refine in place. Follow-ups adjust the same prompt and change only what was asked.
5. Critique and variants. On request, review or test an existing prompt, or return labelled options.

House style governs both the generated prompt and every reply. Any em dash, banned word, or markup fails the output.

## Install

See [prompt-engineer/INSTALL.md](prompt-engineer/INSTALL.md) for step-by-step setup on OpenCode, Microsoft 365 Copilot, ChatGPT, Gemini, and Claude.

Quick start on OpenCode. Copy the `prompt-engineer` folder into a skills directory, then start a new session.

```text
Project scope: <your-project>/.opencode/skills/prompt-engineer/
Global scope:  ~/.config/opencode/skills/prompt-engineer/
```

Then ask in plain language. For example: write me a prompt for a non-technical executive summary, targeting ChatGPT.

## Repository layout

```text
prompt-engineer/
  SKILL.md                          agent instructions
  INSTALL.md                        setup guide for five platforms
  shared.md                         frameworks and house style, always loaded
  claude.md                         Claude mechanics
  gpt-copilot.md                    ChatGPT and Copilot mechanics
  gemini.md                         Gemini mechanics
  deepseek.md                       DeepSeek mechanics
  qwen.md                           Qwen mechanics
  agents/openai.yaml                OpenAI and Codex interface manifest
  prompt-engineer-flow.excalidraw   editable diagram source
  prompt-engineer-flow.png          rendered diagram
```

## License

MIT. See [LICENSE](LICENSE).

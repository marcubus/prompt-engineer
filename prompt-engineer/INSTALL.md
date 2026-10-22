# Prompt Engineer: install guide

Prompt Engineer turns a rough request into one finished, ready-to-use prompt for a
target AI model. It also critiques, tests, and improves prompts you already have.

This guide covers five places to run it:

1. [OpenCode](#1-opencode) (native skill, no setup beyond the files)
2. [Microsoft 365 Copilot](#2-microsoft-365-copilot) (regular Copilot, not Copilot Studio)
3. [ChatGPT](#3-chatgpt)
4. [Gemini](#4-gemini-gems)
5. [Claude](#5-claude)

## What's in this pack

- `prompt-engineer/SKILL.md` — the agent instructions.
- `prompt-engineer/shared.md` — the shared knowledge file, carrying the house style and
  the frameworks.
- `prompt-engineer/claude.md`, `prompt-engineer/gpt-copilot.md`,
  `prompt-engineer/gemini.md`, `prompt-engineer/deepseek.md`, and
  `prompt-engineer/qwen.md` — the model knowledge files.
- `prompt-engineer/agents/openai.yaml` — the OpenAI and Codex interface manifest.
- `prompt-engineer/INSTALL.md` — this guide.

## The agent instructions

Every platform below needs the same instructions pasted into it. Copy the block between
the lines.

```text
You are a master prompt engineer. Turn a rough request into a single, complete,
ready-to-use prompt for a specific AI model: Claude, ChatGPT, Gemini, Microsoft Copilot,
DeepSeek, or Qwen. Default to Claude unless the user names a target. You also critique,
test, and improve prompts that the user brings you.

Apply the house style to everything you write, the generated prompt and your own reply.
Follow the house style defined in the shared knowledge file under House Style: plain
text only, no em dashes, no banned words, no markup. Any breach fails the output.

Before writing, work out:
1. Intent: analytical, creative, extraction or summarisation, decisioning, or instruction.
2. Audience: expertise level, relationship to the output, what they care about most,
   and register.
3. Target model and prompt type: one-shot (default), system prompt, user turn,
   tool or agent definition, or critique and variants.
4. Modality: text only, or multimodal (images, documents, audio, video).
5. Anatomy: the elements the prompt needs. Role, task, context, source material,
   output format, tone, constraints, examples, and output language.

Use the frameworks in your knowledge base. Apply only techniques suited to the target model.

If the request is ambiguous, first show a short confirmation block (intent, audience,
target model, prompt type, modality, output language, anatomy, assumptions) and wait.
If it is clear, generate directly. Keep the confirmation block in the house style too.

For a tool or agent prompt, define the tool set, the use policy, and the stopping
conditions explicitly. For a multimodal prompt, name the attachment and say what to
extract from it. Name the output language whenever more than one is plausible.

Output the finished prompt only, with no preamble and no commentary. Apply the quality
checklist before you output. Never output a prompt that fails a checklist item.

When asked to review, test, or improve a prompt, name the specific checklist gap each
finding violates. When asked for variants, differ on one labelled axis with a stated
trade-off and a recommendation.

After delivering, stay in the conversation and treat follow-ups as refinements to the
same prompt, changing only what the user asked for.
```

When a platform supports knowledge files, upload the six knowledge files from the
skill folder. When it does not, paste the shared file into the
knowledge or context area, or prepend it to a request.

---

## 1. OpenCode

OpenCode loads skills from disk. No account or app is needed.

1. Copy the `prompt-engineer` folder into a skills directory:
   - Project scope: `<your-project>/.opencode/skills/prompt-engineer/`
   - Global scope: `~/.config/opencode/skills/prompt-engineer/`
2. Keep the folder layout intact, so `SKILL.md` and the knowledge files stay together.
3. Start a new OpenCode session so it discovers the skill.
4. Ask in plain language, for example: write me a prompt for a non-technical executive
   summary, targeting ChatGPT.

Verify: the session lists `prompt-engineer` among available skills.

---

## 2. Microsoft 365 Copilot

Regular Microsoft 365 Copilot Chat, not Copilot Studio.

1. Sign in at <https://m365.cloud.microsoft/chat>, or open Copilot in Teams.
2. Choose Create an agent, or open the Agents area and pick New agent.
3. Name it Prompt Engineer and give it a short description.
4. Paste the agent instructions above into the Instructions box.
5. Under Knowledge, add the six knowledge files. Upload them
   from OneDrive or SharePoint, since Copilot grounds agents on your Microsoft 365 content.
6. Save, then open the agent from Copilot Chat and describe the prompt you want.

If your tenant does not show the agent builder, use the no-setup path: start a Copilot
chat, paste the agent instructions, then add your request on the next line. Save the
instructions somewhere handy to reuse.

---

## 3. ChatGPT

Two options. Custom GPTs support knowledge files. Custom instructions apply to every chat.

### Custom GPT (knowledge files supported)

1. Open ChatGPT and go to Explore GPTs, then Create.
2. Switch to the Configure tab.
3. Set the name to Prompt Engineer and add a short description.
4. Paste the agent instructions above into the Instructions box.
5. Under Knowledge, upload the six knowledge files.
6. Save, then choose who can use it. Open the GPT and describe the prompt you want.

### Custom instructions (no files)

1. Open Settings, then Personalization, then Custom instructions.
2. Paste the agent instructions above into the field about how ChatGPT should respond.
3. Save. Start a new chat and describe the prompt you want.

Note: custom instructions have a length limit. If the block does not fit, use the shared
file section and trim the model list.

---

## 4. Gemini (Gems)

Gems are Gemini's saved, reusable assistants.

1. Open <https://gemini.google.com> and choose Gems in the sidebar.
2. Choose New Gem.
3. Name it Prompt Engineer and add a short description.
4. Paste the agent instructions into the instructions field.
5. Attach the six knowledge files if your plan allows uploads.
6. Save, then open the Gem and describe the prompt you want.

Note: file attachments in Gems need a paid Google plan. Without uploads, paste the shared
file into the first message of a chat and keep the Gem open for the rest.

---

## 5. Claude

Claude Projects hold instructions plus a project knowledge base.

1. Open <https://claude.ai> and go to Projects.
2. Choose New Project and name it Prompt Engineer.
3. Open the project instructions and paste the agent instructions above.
4. Add the six knowledge files to the project knowledge.
5. Start a chat inside the project and describe the prompt you want.

Alternative without a Project: open Settings, then Profile, and paste the agent
instructions into the custom instructions field. This applies to every chat, but carries
no knowledge files.

---

## Confirm it works

Send this request on any platform:

> Write me a prompt that turns a meeting transcript into five action items with owners.

Check the reply against three things:

- It returns one finished prompt, with no preamble.
- It follows the house style: plain text, no markdown, no em dashes.
- It names the target model, or defaults to Claude when you do not name one.

## Tips

- Name the target model every time. Each model family has its own prompt mechanics.
- Say what the audience cares about. Non-technical executives beats general audience.
- Ask for a system prompt explicitly when you configure a bot.
- For images and documents, say what is attached and what to extract.
- Refine in place. Follow-ups adjust the same prompt rather than starting over.

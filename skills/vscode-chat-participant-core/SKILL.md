---
name: vscode-chat-participant-core
description: Design or implement a VS Code Chat Participant. Use when creating a participant, request handler, slash commands, participant registration, orchestration layer, or deciding how a participant should own an LLM interaction.
---

# VS Code Chat Participant Core

Build chat participants as thin, explicit orchestration layers around VS Code APIs, model calls, and narrowly scoped capabilities.

## Use this skill when

Use this skill when the task involves any of the following:

- `vscode.chat.createChatParticipant`
- `vscode.ChatRequestHandler`
- `contributes.chatParticipants`
- participant slash commands
- participant detection/disambiguation
- participant architecture
- chat participant model invocation
- converting a monolithic chatbot into a maintainable VS Code participant

## Core model

Treat the participant as the owner of the interaction, not as the owner of every implementation detail.

Preferred architecture:

```text
ChatRequestHandler
  ├─ normalize request
  ├─ gather bounded context
  ├─ determine command/mode/intent
  ├─ choose allowed capabilities
  ├─ invoke orchestration/model layer
  ├─ stream response
  └─ return metadata/result
```

Keep these concerns outside the request handler where practical:

```text
context/
  buildContext.ts
routing/
  resolveMode.ts
prompts/
  participantPrompt.tsx
tools/
  ...
orchestration/
  runParticipant.ts
ui/
  renderResponse.ts
```

## Rules

### One participant should represent one coherent domain

Prefer one participant per extension. Do not split every internal role into a separate `@participant`.

Internal concepts such as planner, reviewer, tester, researcher, or fixer should usually be modes, slash commands, skills, tools, or internal orchestration steps rather than separate public participants.

### Respect the user's selected model

Inside a chat participant, prefer `request.model` for model calls rather than independently choosing a model.

Do not hard-code a provider unless the extension's explicit purpose requires it.

### Use VS Code APIs before model inference

If VS Code can provide a fact deterministically, retrieve it directly.

Examples:

- diagnostics
- selected text
- active editor
- document language
- symbols
- references
- definitions
- workspace folders
- test state
- terminal state
- commands
- debug state

Then give the resulting structured data to the model only when reasoning or explanation is needed.

### Keep mode selection explicit

A useful mode set is:

```ts
type ParticipantMode =
  | 'answer'
  | 'explain'
  | 'inspect'
  | 'plan'
  | 'modify'
  | 'execute';
```

Map each mode to allowed capabilities. Do not expose every tool in every request.

### Treat slash commands as stable shortcuts

Use slash commands for predictable, recurring behavior such as `/review`, `/explain`, `/test`, or `/architecture`.

Do not create a slash command for every small intent that natural language can handle reliably.

## Minimal implementation shape

```ts
const handler: vscode.ChatRequestHandler = async (
  request,
  context,
  stream,
  token,
) => {
  const participantContext = await buildParticipantContext({
    request,
    context,
    token,
  });

  const mode = resolveParticipantMode(request, participantContext);

  return runParticipant({
    request,
    context: participantContext,
    mode,
    model: request.model,
    stream,
    token,
  });
};
```

## Before finishing

Check:

- Is the participant thin?
- Is the user-selected model respected?
- Are deterministic VS Code APIs used before LLM inference?
- Are read and write capabilities separated?
- Are commands/modes explicit?
- Is cancellation propagated?
- Are public participant names and descriptions domain-specific?
- Could any capability be a reusable language-model tool instead?

## References

- https://code.visualstudio.com/api/extension-guides/ai/chat
- https://code.visualstudio.com/api/extension-guides/ai/language-model
- https://code.visualstudio.com/api/extension-guides/ai/ai-extensibility-overview

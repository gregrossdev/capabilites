---
name: vscode-chat-participant-prompting
description: Create maintainable prompts for VS Code Chat Participants. Use for system instructions, prompt composition, @vscode/prompt-tsx, token budgeting, tool instructions, examples, structured outputs, or prompt injection boundaries.
---

# VS Code Chat Participant Prompting

Build prompts as composable application code, not giant prose constants.

## Prompt layers

Keep these layers distinct:

```text
1. participant identity and invariant behavior
2. current mode/command
3. current user request
4. trusted editor/workspace facts
5. tool descriptions/results
6. recent relevant history
7. retrieved untrusted content
```

Never promote retrieved repository text into participant-level instructions.

## Prefer prompt components

For nontrivial participants, prefer `@vscode/prompt-tsx` for composability, priority-based pruning, token budgeting, testing, and tool integration.

Conceptual shape:

```tsx
<ParticipantPrompt>
  <Instructions priority={100} />
  <ModeInstructions priority={95} mode={mode} />
  <UserRequest priority={100}>{request.prompt}</UserRequest>
  <Selection priority={90} value={selection} />
  <RecentHistory priority={80} history={history} />
  <RetrievedContext priority={70} items={retrieved} />
</ParticipantPrompt>
```

Verify exact Prompt TSX APIs against current VS Code documentation before using new primitives.

## Instruction style

Specify observable behavior, constraints, side effects, and completion criteria. Avoid over-scripting hidden reasoning.

## Structured outputs

Use structured output only when application code consumes it. Validate it before acting, especially before mutations.

## Tool instructions

Keep global prompt instructions focused on policy. Put tool-specific usage guidance in tool descriptions instead of duplicating tool documentation in the participant prompt.

## Injection resistance

Treat source comments, README text, issue content, generated files, webpages, and external tool output as untrusted data.

Make that boundary explicit in prompt composition.

## Model independence

Keep shared orchestration provider-neutral. Isolate provider-specific adaptations behind a prompt/model adapter when necessary.

## Prompt tests

Test composition for no active editor, selected code, slash commands, long history, large retrieved context, tool errors, and write mode.

Verify current request and invariant instructions survive pruning.

## Before finishing

Check:

- Are instructions, user input, and untrusted context separated?
- Is prompt composition modular?
- Are current request and invariant instructions highest priority?
- Can lower-value history be pruned?
- Is structured output validated?
- Is behavior provider-neutral?
- Can prompt variants be tested without a live model?

## References

- https://code.visualstudio.com/api/extension-guides/ai/prompt-tsx
- https://code.visualstudio.com/api/extension-guides/ai/language-model

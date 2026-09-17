---
name: vscode-chat-participant-ux
description: Design the chat UX of a VS Code Chat Participant. Use for ChatResponseStream, progress, markdown, file references, buttons, follow-ups, slash-command UX, participant naming, cancellation, or response ergonomics.
---

# VS Code Chat Participant UX

Make the participant feel integrated with VS Code rather than like a generic chatbot embedded in an editor.

## Stream useful progress

Stream output as soon as useful information exists.

Good progress examples:

```text
Inspecting current diagnostics…
Found 3 errors in src/router.ts
Checking references to createRouter…
Running the targeted test…
```

Avoid narrating low-level implementation noise.

## Use the response surface

Use appropriate response elements for markdown, file/location references, progress, buttons/commands, and follow-up suggestions.

Prefer direct references to files and ranges when the answer depends on source code.

## Buttons

Use buttons for concrete next actions such as Open file, Show diagnostics, Run targeted test, Apply proposed change, or View diff.

Buttons should invoke deterministic VS Code or extension commands, not hide ambiguous autonomous behavior.

## Follow-ups

Suggested follow-ups should advance the current task.

Prefer task-specific suggestions over generic prompts such as “Tell me more”.

## Slash commands

Slash commands should represent stable workflows such as `/review`, `/explain`, `/test`, or `/plan` and should materially alter behavior, tool policy, or response structure.

## Cancellation

Propagate cancellation through context retrieval, tool invocation, model requests, and stream rendering.

## Progressive disclosure

Default to concise, task-focused output. Put detailed diagnostics, traces, and secondary information behind explicit actions or follow-ups.

## Mutation UX

For consequential changes, explain the proposed scope, preserve reviewability, require consent where appropriate, make the action observable, and report exactly what changed.

## Error UX

Translate implementation errors into actionable language while keeping technical details available in logs.

## Before finishing

Check:

- Does output start streaming quickly?
- Are progress updates meaningful?
- Are file references included where useful?
- Are buttons concrete and deterministic?
- Are follow-ups task-specific?
- Is cancellation propagated?
- Are destructive actions reviewable?
- Is error language actionable?

## References

- https://code.visualstudio.com/api/extension-guides/ai/chat
- https://code.visualstudio.com/api/extension-guides/ai/chat-tutorial

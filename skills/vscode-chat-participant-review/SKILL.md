---
name: vscode-chat-participant-review
description: Review a VS Code Chat Participant implementation for architecture, LLM reliability, context discipline, tool design, UX, safety, and testability. Use before merge, during refactors, or when a participant feels brittle or overly agentic.
---

# VS Code Chat Participant Review

Review the participant as an orchestration system, not merely as TypeScript code.

## Review order

Inspect in this order:

1. public participant contract
2. request handler
3. context builder
4. routing/mode policy
5. prompt construction
6. tools and side effects
7. model invocation loop
8. response streaming/UI
9. tests/evals

## Architecture checks

Flag when the request handler contains most business logic, model calls are scattered, tool calls bypass policy, prompt strings are duplicated, context collection is tangled with rendering, or internal roles have become unnecessary public participants.

Prefer:

```text
participant → context/routing → orchestrator → tools/model → renderer
```

## Model checks

Verify the participant uses `request.model` unless documented otherwise, cancellation reaches model calls, retries are bounded, tool loops terminate, and malformed structured output cannot directly trigger a mutation.

## Context checks

Verify full history is not replayed blindly, selection/reference context has high priority, stale state is refreshed, untrusted repository content is data rather than instructions, duplicates are removed, and provenance is preserved.

## Tool checks

For each tool ask:

- Is it narrow?
- Is its input typed?
- Is its output structured?
- Is it read-only or mutating?
- Does its description tell the model when to use it?
- Is failure explicit?
- Can it be unit tested?
- Does it really need to be model-invoked?

Flag broad tools such as `doTask`, `fixProject`, `runAnything`, or `workspaceAgent` unless their broadness is constrained by a hard boundary.

## Safety and mutation

Verify read/write policies differ, edits are scoped, destructive actions are explicit, irreversible/costly operations receive appropriate consent, and external side effects are not hidden behind generic tool names.

## UX checks

Verify responses stream progressively, progress is meaningful, source locations are referenced, errors are actionable, buttons represent concrete actions, follow-ups are task-specific, and public descriptions are concise and domain-specific.

## Testability checks

Flag core behavior that requires a live model to test. Prefer injectable model, tool executor, context providers, and state/clock abstractions where relevant.

## Review output format

Return findings by severity:

```text
Critical
High
Medium
Low
```

For each finding include file/symbol, concrete behavior, why it matters, and the smallest reasonable correction.

Do not rewrite the entire implementation unless requested.

## Final review questions

- Could this participant be simpler?
- Is LLM reasoning used only where reasoning is useful?
- Are deterministic editor facts kept deterministic?
- Are tool boundaries obvious?
- Can a user understand what happened?
- Can a developer reproduce failures?
- Can the system stop safely?
- Does every public participant/command earn its place?

## References

- https://code.visualstudio.com/api/extension-guides/ai/chat
- https://code.visualstudio.com/api/extension-guides/ai/tools
- https://code.visualstudio.com/api/extension-guides/ai/prompt-tsx
- https://code.visualstudio.com/api/extension-guides/ai/ai-extensibility-overview

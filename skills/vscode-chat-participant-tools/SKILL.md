---
name: vscode-chat-participant-tools
description: Design tools and capabilities used by VS Code Chat Participants and LLM agents. Use for Language Model Tool API, tool schemas, read/write separation, tool calling, VS Code API wrappers, MCP decisions, approvals, or tool result design.
---

# VS Code Chat Participant Tools

Expose narrow, deterministic capabilities to the model instead of embedding implementation behavior in prompts.

## Decision order

```text
Can VS Code API answer it directly?
  yes → call VS Code API

Should an LLM choose when to invoke this reusable VS Code capability?
  yes → Language Model Tool

Should the capability be portable outside VS Code?
  yes → consider MCP

Does the participant need private orchestration logic only?
  yes → internal function/service
```

## Good tool properties

A good tool does one coherent thing, has a small typed input, deterministic side effects, structured output, explicit errors, and can be tested without an LLM.

Prefer:

```text
searchWorkspace(query)
readFile(uri, range?)
getDiagnostics(uri?)
findReferences(symbol)
runTests(testIds?)
applyPatch(uri, patch)
```

Avoid broad tools such as `doCodingTask(instructions)` or `doWorkspaceStuff(request)`.

## Separate reads from writes

Classify every capability as read-only or mutating.

Read-only examples: search, file reads, symbols, references, diagnostics, git status, test discovery.

Mutation examples: edit/create/delete file, execute commands with side effects, git commit/push, modify settings.

Require stronger validation or user consent for costly, irreversible, destructive, or externally visible actions.

## Schema design

Prefer concrete schemas and enums over free-form strings. Return structured facts instead of decorative prose when the result is consumed by the model.

## Tool-loop discipline

Use stopping conditions such as maximum iterations, duplicate-call detection, cancellation, bounded retries, and no-progress detection.

Do not allow an unbounded model → tool → model loop.

## Tool availability

Expose only tools relevant to the current mode/task. Smaller tool sets improve selection quality and simplify safety reasoning.

## MCP boundary

Prefer a VS Code extension tool when the capability depends on editor state or VS Code APIs. Prefer MCP when the capability should work across clients or accesses external systems.

## Before finishing

Check:

- Is each tool narrow?
- Is input typed and bounded?
- Is output structured?
- Are mutations labeled?
- Are dangerous actions gated?
- Are errors visible?
- Is cancellation supported?
- Is the active tool set smaller than the global catalog?

## References

- https://code.visualstudio.com/api/extension-guides/ai/tools
- https://code.visualstudio.com/api/extension-guides/ai/mcp
- https://code.visualstudio.com/api/extension-guides/ai/ai-extensibility-overview

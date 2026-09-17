---
name: vscode-chat-participant-testing
description: Test and evaluate VS Code Chat Participants and their LLM orchestration. Use for unit tests, integration tests, mocked models, tool contract tests, prompt snapshots, eval cases, regression suites, cancellation tests, or acceptance criteria.
---

# VS Code Chat Participant Testing

Test the deterministic shell heavily and model-dependent behavior with bounded evaluations.

## Testing pyramid

Prefer:

```text
many deterministic unit tests
       ↓
tool/adapter contract tests
       ↓
VS Code integration tests
       ↓
small targeted LLM eval suite
```

Do not make every test depend on a live model.

## Unit-test independently

Test:

- mode/intent routing
- context selection
- history pruning
- tool policy
- prompt composition
- structured output parsing
- response rendering helpers
- error translation
- confirmation rules

## Tool contract tests

For each tool verify valid input, malformed input, empty result, expected result, cancellation, VS Code API errors, permission/consent behavior, and mutation boundaries.

## Prompt tests

Verify that current request and invariant participant instructions survive pruning, untrusted workspace content stays separated, explicit references outrank incidental context, and write-mode instructions differ from read-only mode.

## Model adapter tests

Mock the model interface so retries, malformed responses, loop limits, cancellation, and tool-call sequences are deterministic.

## Integration tests

Use VS Code extension-host tests for participant activation, active-editor context, diagnostics, command registration, URI/range handling, workspace edits, and cancellation propagation.

## LLM eval suite

Maintain representative cases such as:

- simple question
- selected-code explanation
- workspace inspection
- ambiguous request
- read-only review
- requested modification
- destructive request
- tool failure
- large context
- follow-up turn

Define observable expectations rather than exact prose.

## Regression cases

Whenever a real failure occurs, add a regression case.

Examples:

- chose edit tool during explanation
- replayed stale history
- ignored current selection
- repeated the same tool forever
- failed after empty tool data
- did not stop on cancellation
- participant detection triggered on unrelated input

## Before finishing

Produce or update unit tests, tool contract tests, at least one integration path, eval fixtures for new behavior, and regression coverage for bugs being fixed.

## References

- https://code.visualstudio.com/api/extension-guides/testing
- https://code.visualstudio.com/api/extension-guides/ai/chat

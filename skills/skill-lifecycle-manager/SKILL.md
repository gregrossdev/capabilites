---
name: skill-lifecycle-manager
description: Continuously improve a skills registry by reviewing how skills perform in real work, patching stale or weak skills, and creating new skills when repeated workflows or lessons emerge. Use after non-trivial tasks, after user corrections, after discovering a better workflow, when a skill caused errors, or when the current registry lacks a reusable procedure.
---

# Skill Lifecycle Manager

Treat the skills registry as procedural memory that should improve from real use.

The goal is not to create skills constantly. The goal is to preserve workflows that are repeatedly useful, correct skills that drift, and keep the registry small enough that discovery remains reliable.

This pattern is inspired by Hermes Agent's agent-managed skills model: successful multi-step workflows, corrected approaches, and recovered failure paths are candidates for durable skill updates. Hermes also prefers targeted patches over full rewrites where possible. This skill applies that idea to a Git-backed `skills.sh` style registry.

## Use this skill when

Review the registry when one or more of these signals occur:

- a non-trivial multi-step workflow succeeded and is likely to recur
- an existing skill was used and its instructions were incomplete, stale, ambiguous, or inefficient
- the agent hit dead ends, then discovered a reliable working path
- the user corrected the agent's procedure or preferences
- the same explanation or workflow has appeared multiple times
- a task required several ad-hoc steps that should become reusable
- a new tool, API, library, or platform materially changes an existing skill
- the registry contains overlapping skills that should be consolidated
- a skill's description causes poor triggering or discoverability

Do not modify skills merely because wording could be prettier.

## Core loop

Use this lifecycle:

```text
perform task
  ↓
observe what actually worked
  ↓
compare against current skills
  ↓
classify learning
  ├─ no durable value → do nothing
  ├─ small correction → patch existing skill
  ├─ new procedure → create skill
  ├─ major structural drift → revise existing skill
  └─ obsolete/duplicate skill → propose consolidation/removal
  ↓
validate
  ↓
commit minimal change
```

## Step 1 — Extract the durable lesson

After a meaningful task, identify only reusable procedural knowledge.

Good candidates:

- the correct sequence of operations
- the command/API/tool that actually worked
- prerequisites that were easy to miss
- a failure mode and its reliable recovery path
- the user's stable preferred output/workflow
- decision rules that distinguish two similar approaches
- validation steps that prevented bad changes

Poor candidates:

- one-off project data
- secrets, tokens, IDs, or credentials
- transient status information
- facts that belong in project documentation rather than an agent skill
- speculative advice that was never validated
- information already covered well by an existing skill

## Step 2 — Search before creating

Before creating a skill:

1. inspect skill names and descriptions
2. search for overlapping instructions
3. load the most relevant existing skill
4. decide whether the new lesson belongs there

Default to improving an existing skill when the new procedure is the same capability.

Create a new skill only when it represents a distinct reusable capability with its own trigger conditions.

## Step 3 — Choose the smallest mutation

Prefer changes in this order:

### A. No change

Use when the lesson is not durable or already represented correctly.

### B. Targeted patch

Preferred for:

- adding one missing rule
- fixing one obsolete command
- refining a trigger description
- adding one failure/recovery case
- correcting a misleading example

Do not rewrite the whole skill for a local correction.

### C. Supporting file update

Use `references/`, `scripts/`, or `assets/` when detailed material would bloat `SKILL.md`.

Examples:

```text
references/api-notes.md
references/migration-guide.md
scripts/validate.sh
scripts/bootstrap.ts
assets/template.yaml
```

### D. Structural revision

Use when the existing workflow itself has materially changed.

Preserve still-valid instructions instead of replacing the skill from scratch without cause.

### E. New skill

Create a new skill when all are true:

- the capability is reusable
- the workflow is non-trivial
- it is distinct from existing skills
- its trigger can be described clearly
- the procedure has enough evidence to be useful

## Step 4 — Create high-signal skills

For new skills, use the repository's `skill-authoring` conventions.

Minimum structure:

```text
skills/<skill-name>/SKILL.md
```

Minimum frontmatter:

```yaml
---
name: concise-kebab-case-name
description: State what the skill does and concrete situations that should trigger it.
---
```

The body should usually contain:

1. purpose
2. when to use it
3. preferred workflow
4. decision rules
5. failure/recovery guidance
6. validation / done criteria
7. references when needed

Keep reusable procedure in the skill; keep transient observations out.

## Step 5 — Validate every skill change

Before committing, verify:

- YAML frontmatter parses
- directory name and `name` agree
- description contains useful trigger language
- instructions do not depend on hidden conversation context
- commands and APIs are real and current
- examples do not contain user secrets or environment-specific credentials
- the change does not duplicate another skill unnecessarily
- destructive or mutating behavior has appropriate safeguards
- current skill remains useful to agents that did not see the task that inspired the change

For version-sensitive technologies, verify current documentation before changing durable guidance.

## Step 6 — Maintain provenance

When practical, include references for externally sourced or version-sensitive guidance.

Prefer official documentation over secondary posts.

Do not copy large source passages into a skill. Convert sources into concise procedural guidance.

## Step 7 — Keep the registry coherent

Periodically review for:

- duplicate skills
- overlapping trigger descriptions
- stale commands or APIs
- oversized skills that should be split
- tiny skills that should be merged
- skills that never provide procedural value
- inconsistent naming
- missing references for rapidly changing technologies

The registry should become more useful, not merely larger.

## Skill creation threshold

A useful heuristic is to create or update a skill when at least one strong signal exists:

- the workflow required several meaningful steps and succeeded
- a prior approach failed and the recovered path is broadly reusable
- the user explicitly corrected how the work should be done
- the workflow has now appeared more than once
- forgetting the procedure would predictably cost time next time

Do not use raw tool-call count as the sole trigger.

## User corrections

Treat explicit user corrections as high-value signals.

When a correction describes a stable procedural preference:

1. determine which existing skill owns the behavior
2. patch that skill if appropriate
3. avoid encoding the preference into unrelated skills

Examples:

```text
"Always use the VS Code diagnostics API before asking the model to infer errors."
→ likely update a VS Code participant/tool skill

"For this one project call the service foo."
→ project context, not a reusable global skill
```

## Failure recovery

Failures are often more useful than happy-path prose.

When a workflow fails and a working path is found, capture:

```text
Symptom
Likely cause
Working recovery
How to verify recovery
```

Do not preserve every failed experiment. Preserve the lesson that prevents recurrence.

## Autonomous behavior boundaries

This skill may identify and prepare skill improvements during normal work, but repository mutation must respect the host agent's actual permissions and approval model.

If direct writes are permitted:

- prefer minimal commits
- change only relevant skills
- report what was changed

If writes require approval:

- present the proposed patch/new skill clearly
- do not claim it was persisted until the write succeeds

Never delete a skill automatically solely because it appears unused. Deletion or consolidation should have clear evidence that the skill is obsolete, harmful, or redundant.

## Suggested Git workflow

For a personal registry with direct-write permission, small isolated changes may go directly to the default branch.

For substantial changes, prefer:

```text
branch
  ↓
edit/create skill
  ↓
review diff
  ↓
validate
  ↓
pull request
```

Group related lifecycle changes together; do not mix unrelated skill edits into one change.

## Review output

When reviewing skills, classify findings as:

```text
Create
- missing reusable capability

Patch
- localized correction or improvement

Revise
- significant workflow drift

Consolidate
- overlapping skills

Retire
- obsolete or harmful skill

No change
- current skill is already adequate
```

For each proposed change, state:

- evidence from actual work
- target skill
- smallest useful change
- expected future benefit

## Anti-patterns

Avoid:

- creating a new skill after every task
- storing project facts as global procedural memory
- rewriting good skills wholesale for small changes
- creating multiple skills with nearly identical triggers
- adding unverified commands learned from model recall
- encoding temporary failures as permanent rules
- silently deleting user-authored guidance
- allowing skill maintenance to distract from the user's primary task

## Done criteria

A lifecycle review is complete when:

- the durable lesson has been identified or intentionally discarded
- relevant existing skills were checked first
- the smallest justified mutation was selected
- any new/updated skill is self-contained and discoverable
- version-sensitive facts were verified
- the registry is no more redundant than before
- the change is persisted or clearly presented for approval

## References

- Hermes Agent Skills System: https://hermes-agent.nousresearch.com/docs/user-guide/features/skills
- Hermes Agent Creating Skills: https://github.com/NousResearch/hermes-agent/blob/main/website/docs/developer-guide/creating-skills.md
- Agent Skills specification: https://agentskills.io/

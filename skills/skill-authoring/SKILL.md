---
name: skill-authoring
description: Create or revise reusable Agent Skills in this repository. Use whenever the user asks to create a new skill, convert instructions into a skill, improve an existing SKILL.md, split a large skill into smaller skills, add supporting scripts/references/assets, or standardize skills for use with skills.sh-compatible agents.
---

# Skill Authoring

Create small, reusable, discoverable Agent Skills that are easy for coding agents to trigger and follow.

## Repository contract

Each skill belongs under:

```text
skills/<skill-name>/
  SKILL.md
  scripts/      # optional
  references/   # optional
  assets/       # optional
```

`SKILL.md` is always the entrypoint.

## Start with the capability

Before writing the skill, identify the single capability it should provide.

Good scopes:

- review a VS Code chat participant
- create a GitHub Actions workflow
- design a React component API
- inspect a Proxmox cluster configuration
- write a product requirements document

Poor scopes:

- software engineering
- frontend
- DevOps
- do everything related to AI

If the requested capability contains several independent workflows, split it into multiple skills unless they must always be used together.

## Naming

Use a short, lowercase, kebab-case name.

Prefer capability-oriented names:

```text
vscode-chat-participant-review
react-component-architecture
github-actions-review
skill-authoring
```

Avoid names that are vague or tied to an internal project unless the skill truly is project-specific.

## Frontmatter

Every `SKILL.md` starts with YAML frontmatter:

```yaml
---
name: example-skill
description: Do X. Use when the user asks for Y, Z, or related tasks.
---
```

### `name`

The `name` must match the skill directory.

### `description`

The description is both a summary and a trigger contract.

It should state:

1. what the skill does
2. when the agent should use it
3. important synonyms or neighboring requests that should trigger it

Prefer:

```yaml
description: Review VS Code Chat Participant implementations for architecture, context discipline, tool design, UX, and testability. Use before merge, during refactors, or when a participant feels brittle.
```

Avoid:

```yaml
description: Helps with VS Code.
```

Do not depend on the body of the skill to explain when the skill should trigger. Put that in `description`.

## Body structure

A strong skill usually contains these sections when relevant:

```text
# Skill title

Purpose / governing principle

## Use this skill when

## Core workflow

## Rules / constraints

## Patterns or examples

## Anti-patterns

## Before finishing

## References
```

Do not include sections merely to satisfy a template. Keep only what improves execution.

## Write instructions for an agent

Use imperative, operational language.

Prefer:

```text
Inspect the existing file before editing it.
Use the smallest relevant tool set.
Return findings grouped by severity.
```

Avoid explanatory prose that does not affect behavior.

The skill should tell the agent:

- what to inspect
- what decisions to make
- what constraints matter
- what sequence is useful
- what completion looks like

## Separate deterministic work from judgment

When a task can be done deterministically, instruct the agent to use the deterministic mechanism first.

Examples:

```text
compiler diagnostics before model speculation
API/schema inspection before guessing
repository search before assuming a file location
tests before claiming behavior works
```

Use model judgment for interpretation, prioritization, design tradeoffs, or synthesis rather than facts an available tool can directly establish.

## Keep skills composable

A skill should not absorb every neighboring concern.

For example:

```text
vscode-chat-participant-core
vscode-chat-participant-context
vscode-chat-participant-tools
vscode-chat-participant-testing
```

is usually better than:

```text
vscode-everything
```

Use references between skills only when helpful. Do not make a web of mandatory dependencies for simple tasks.

## Supporting directories

### `scripts/`

Add scripts when deterministic execution is more reliable than repeatedly asking an agent to recreate code.

Good uses:

- validation
- generation
- formatting
- repository checks
- migrations
- repeatable transformations

A script should be runnable independently and should fail clearly.

### `references/`

Put stable, reusable reference material here when including it in `SKILL.md` would make the main instructions too large.

Examples:

- API notes
- architecture examples
- checklists
- schemas
- command references

Do not copy large third-party documentation when a link or concise summary is sufficient.

### `assets/`

Use for templates or small artifacts the skill needs to produce outputs consistently.

Examples:

- starter files
- config templates
- sample manifests

## Progressive disclosure

Keep `SKILL.md` useful when loaded alone.

Put the highest-value behavior in the main file. Move deep detail to supporting references only when necessary.

An agent should understand the basic workflow without reading every supporting file.

## Examples

Examples should demonstrate behavior, not merely restate rules.

Good:

```text
User: "Review this participant before I merge it."

Expected behavior:
1. Inspect participant registration and handler.
2. Inspect context and tool policies.
3. Verify writes are gated appropriately.
4. Review tests.
5. Return concrete findings by severity.
```

Keep examples short enough that they do not dominate the skill.

## Avoid overfitting

Do not encode temporary details such as:

- current file names unless they are part of an API contract
- one project's directory structure
- one provider's model name
- transient implementation hacks

If the skill is intentionally project-specific, say so explicitly in the description.

## Version-sensitive subjects

For APIs, frameworks, products, standards, or tools that change frequently:

- instruct the agent to verify current official documentation when accuracy depends on the version
- prefer official primary sources
- avoid hard-coding unstable details unless the skill intentionally targets a pinned version

## Skill creation workflow

When creating a new skill:

1. Understand the requested capability.
2. Search the repository for overlapping skills.
3. Decide whether to create a new skill or extend an existing one.
4. Choose a concise kebab-case name.
5. Write trigger-rich frontmatter.
6. Write the minimal operational workflow.
7. Add scripts/references/assets only if they materially help.
8. Check for project-specific assumptions.
9. Check that the skill can be used independently.
10. Update the repository README or catalog if the repo maintains one.

## Revising an existing skill

Preserve the existing capability unless the user requests a redesign.

Prefer improving:

- trigger clarity
- instruction precision
- boundaries
- examples
- acceptance criteria
- separation of concerns

If the skill has become too large, identify coherent sub-capabilities and split them rather than continuously appending instructions.

## Quality checklist

Before finishing, verify:

- Does the directory name match `name`?
- Does the description clearly explain when to trigger?
- Is there one coherent capability?
- Are instructions operational rather than conversational?
- Are deterministic sources preferred over guessing?
- Are side effects and boundaries clear?
- Is project-specific context avoided unless intentional?
- Can supporting detail be moved out of `SKILL.md`?
- Are examples representative and concise?
- Is there a clear definition of done?
- Would another agent know when not to use this skill?

## Default output for a new skill

Unless the user asks otherwise, create:

```text
skills/<skill-name>/SKILL.md
```

Only add supporting directories when they are actually needed.

## Repository maintenance

When adding a skill to this repository:

- preserve existing skills
- avoid duplicate capability names
- update the README's skill list
- keep naming consistent
- make changes directly when authorized rather than returning only draft text

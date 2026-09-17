# Capabilites

Personal registry of reusable agent skills and capabilities.

This repository follows the common Agent Skills / skills.sh pattern: each skill lives in its own folder under `skills/` with a `SKILL.md` entrypoint and optional supporting `scripts/`, `references/`, and `assets/` directories.

## Install

Install the collection:

```bash
npx skills add gregrossdev/capabilites
```

Install a specific skill:

```bash
npx skills add gregrossdev/capabilites --skill vscode-chat-participant-core
```

## Layout

```text
skills/
  <skill-name>/
    SKILL.md
    scripts/      # optional
    references/   # optional
    assets/       # optional
```

## Current skills

### Skill authoring and lifecycle

- `skill-authoring` — create, revise, split, and maintain reusable skills in this repository
- `skill-lifecycle-manager` — review real work, patch weak/stale skills, and create new skills when durable workflows emerge

### VS Code Chat Participants

- `vscode-chat-participant-core`
- `vscode-chat-participant-context`
- `vscode-chat-participant-tools`
- `vscode-chat-participant-prompting`
- `vscode-chat-participant-ux`
- `vscode-chat-participant-testing`
- `vscode-chat-participant-review`

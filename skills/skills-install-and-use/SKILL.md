---
name: skills-install-and-use
description: Discover, install, load, and maintain Agent Skills (SKILL.md packages) with the skills.sh CLI or by hand, for any coding agent or LLM. Use when the user asks to add, install, find, update, remove, or pin a skill, asks how to use a skill they installed, wants a skill available in another agent (Codex, Cursor, pi, Gemini CLI, plain API), or asks whether to copy a third-party skill into their own registry.
---

# Skills: install and use

A skill is a folder with a `SKILL.md` entrypoint (YAML frontmatter `name` and `description`, then instructions) and optional `scripts/`, `references/`, and `assets/`. Any agent that can read files can use one. The `skills` CLI (skills.sh) only automates discovery, installation, and updates; nothing in a skill depends on it.

Governing rule: own what you author, install what you don't. Third-party skills are never copied into your own registry.

## Use this skill when

- adding a skill from a GitHub repo, skills.sh, or a local path
- making an installed skill visible to another agent
- deciding whether to vendor, symlink, or install a skill
- updating, pinning, or removing skills
- an agent has no native skill support and needs the skill in its prompt
- an installed skill is not triggering or seems stale

## Core workflow

1. **Identify the source.** A skill comes from a repo (`owner/repo`), a full URL, or a local folder. Prefer the upstream repo over any mirror so updates keep flowing.
2. **List before installing.** `npx skills add <source> --list` shows every skill in the source with its description. Pick by name; do not install `*` from a repo you have not read.
3. **Review the skill.** Read `SKILL.md` and any `scripts/`. Skills run with the agent's full permissions. Reject anything that downloads and executes remote code, reaches for credentials, or has an instruction set that does not match its description.
4. **Choose scope.** Global (`-g`) for skills you use in every project; project-level (default, writes into the project's agent directories and `skills-lock.json`) for skills tied to one codebase.
5. **Choose agents.** `-a <agent>` once per agent (`-a codex -a pi`). Omit it to be prompted. See `references/cli.md` for the agent list and how each is wired.
6. **Install.** `npx skills add <source> -s <skill> -g -a <agent> -y`. The CLI stores the files under `~/.agents/skills/<name>` (global) or `.agents/skills/<name>` (project) and symlinks each agent directory to them. Use `--copy` only for agents or filesystems that cannot follow symlinks.
7. **Record it.** The CLI writes a lockfile: `~/.agents/.skill-lock.json` for global, `skills-lock.json` in the project otherwise. Commit the project lockfile. `npx skills experimental_install` restores from it on another machine.
8. **Verify.** `npx skills list` must show the skill, and the agent must see it (most agents list skills at startup or on `/skills`). Trigger it with a request matching its description.

## Using an installed skill

- Read the whole `SKILL.md` when a request matches its description. Do not act from the description alone.
- Open `references/` files only when the skill's instructions point to them for the current task. Progressive disclosure keeps context small.
- Run `scripts/` as provided rather than re-implementing them. A script exists because deterministic execution beat prose.
- Follow the skill's "Before finishing" or completion criteria before reporting done.
- Skills compose. If two match, load both and follow the more specific one where they conflict.

## Agents without native skill support

Any LLM can use a skill through its prompt:

- `npx skills use <owner/repo>@<skill>` prints the skill as a prompt without installing it. Paste it as a system or first user message.
- With direct API access, put the `SKILL.md` body in the system prompt and attach `references/` only when the task needs them.
- For a chat UI, paste `SKILL.md` and say "follow this skill for the next task".

The frontmatter `name` and `description` are metadata for routing; the body is the instruction set. Both travel together.

## Maintaining skills

| Task | Command |
|---|---|
| See what is installed | `npx skills list` |
| Update everything in the lockfile | `npx skills update` (`-g` or `-p` to limit) |
| Update one skill | `npx skills update <name>` |
| Remove | `npx skills remove -s <name> [-g] [-a <agent>]` |
| Search skills.sh | `npx skills find <query>` (`--owner` to filter) |
| Scaffold your own | `npx skills init <name>` |

Third-party skills belong to their upstream repo. Your own registry (a repo of `skills/<name>/SKILL.md`) holds only skills you author or have forked with intent to diverge. To depend on an external skill from your registry, document the install command and let the lockfile be the record.

## Rules

- Never copy a third-party skill folder into your own registry. It loses provenance and stops updating. Install it and document the source.
- Fork only when you will change it. Rename the fork so both can coexist, and note the upstream commit in the fork's `SKILL.md`.
- One skill, one name, one location on disk. Agent directories hold symlinks, not copies, unless `--copy` was unavoidable.
- Review before install, and again after `update`. An update is new code running with your agent's permissions.
- Keep the lockfile in version control for project scope. Global lockfiles are per machine.
- If a plugin or marketplace already ships a skill for one agent, do not also install it through the CLI for that agent. Install it only for the agents the plugin does not reach.

## Anti-patterns

- `npx skills add <repo> --all` on an unread repo.
- Editing a symlinked third-party skill in place. The next `update` overwrites it; fork instead.
- Two agents holding different copies of the same skill.
- Installing a skill to fix a one-off task. Use `npx skills use` for one-offs.
- Trusting a skill because it is popular. Read it.

## Before finishing

- The skill is listed by `npx skills list` and visible to every intended agent.
- The lockfile records it with its source.
- No third-party skill was copied into a registry you own.
- The user knows the update and removal commands for what was installed.

## References

- `references/cli.md`: `skills` CLI commands, flags, scope, lockfile layout, and agent directory wiring.
- Agent Skills specification: https://agentskills.io
- Registry and search: https://skills.sh

# `skills` CLI reference

Run as `npx skills <command>` (package `skills` on npm, from vercel-labs/skills). Verified against the CLI on 2026-09-17; run `npx skills --help` for the current list.

## Commands

| Command | Purpose |
|---|---|
| `add <source>` (`a`) | Install skills from `owner/repo`, a GitHub URL, a Notion page, or a local path |
| `add <source> --list` | List the skills in a source without installing |
| `use <owner/repo>@<skill>` | Print one skill as a prompt, no install |
| `list` (`ls`) | List installed skills |
| `find [query]` | Search skills.sh interactively; `--owner <owner>` to filter |
| `update [skills...]` (`upgrade`) | Update to the latest upstream; `-g` global only, `-p` project only, `-y` skip scope prompt |
| `remove [skills]` | Remove installed skills; `-g`, `-a <agent>`, `-s <skill>`, `--all` |
| `init [name]` | Scaffold `<name>/SKILL.md` (or `./SKILL.md`) |
| `experimental_install` | Restore project skills from `skills-lock.json` |
| `experimental_sync` | Sync skills from `node_modules` into agent directories |

## `add` flags

| Flag | Effect |
|---|---|
| `-g`, `--global` | User-level install under `~/.agents/skills/` |
| (no `-g`) | Project-level install under `./.agents/skills/` plus `skills-lock.json` |
| `-a <agent>` | Target agent; repeat the flag for several (`-a codex -a pi`). Comma lists are rejected. `'*'` for all |
| `-s <skill>` | Skill name; repeat for several; `'*'` for all |
| `-y`, `--yes` | Skip prompts |
| `--all` | Shorthand for `-s '*' -a '*' -y` |
| `--copy` | Copy files into agent directories instead of symlinking |
| `--full-depth` | Search subdirectories even when the root has a `SKILL.md` |
| `--json` | Machine-readable output (not with `--list`) |

## Where things land (global)

```text
~/.agents/skills/<name>/          the installed skill (single source of truth)
~/.agents/.skill-lock.json        lockfile: source, sourceUrl, skillPath, skillFolderHash, timestamps
~/.claude/skills/<name>   -> symlink to ~/.agents/skills/<name>
~/.pi/agent/skills/<name> -> symlink to ~/.agents/skills/<name>
~/.cursor/skills/<name>   -> symlink (and so on per agent)
```

Codex reads `~/.agents/skills` directly, so the CLI creates no `~/.codex/skills` link for it.

Project scope mirrors this under the project root: `.agents/skills/<name>`, `skills-lock.json`, and per-agent directories such as `.claude/skills/`.

## Lockfile entry

```json
"svelte-core-bestpractices": {
  "source": "sveltejs/ai-tools",
  "sourceType": "github",
  "sourceUrl": "https://github.com/sveltejs/ai-tools.git",
  "skillPath": "skills/svelte-core-bestpractices/SKILL.md",
  "skillFolderHash": "<git tree hash>",
  "installedAt": "...",
  "updatedAt": "..."
}
```

`update` compares `skillFolderHash` with upstream and rewrites the folder when it differs.

## Supported agents

The CLI prints the full list on an invalid `-a` value. Common ones: `claude-code`, `codex`, `cursor`, `pi`, `gemini-cli`, `github-copilot`, `opencode`, `windsurf`, `zed`, `hermes-agent`, `amp`, `cline`, `droid`, `kilo`, `warp`. `universal` writes only to `~/.agents/skills` for agents that read it natively.

## Skill folder contract

```text
<name>/
  SKILL.md        required; frontmatter `name` (matches folder) and `description`
  scripts/        optional; runnable, fail loudly
  references/     optional; loaded on demand
  assets/         optional; templates, images, fixtures
```

Specification: https://agentskills.io

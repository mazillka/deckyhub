# DeckyHub umbrella agent guide

This file provides guidance to coding agents (including Claude Code) working in this repository.

## What this is

`mazillka/deckyhub` is an umbrella repository with no code of its own. It pins DeckyHub's two repositories as git submodules:

| Path | Repository | Its own guide |
| --- | --- | --- |
| `deckyhub-plugin/` | `mazillka/deckyhub-plugin`: the Decky Loader plugin (Python backend `main.py`, React/TypeScript frontend `src/`) | `deckyhub-plugin/AGENTS.md`, `deckyhub-plugin/SKILLS.md`, `deckyhub-plugin/.claude/skills/` |
| `deckyhub-registry/` | `mazillka/deckyhub-registry`: `registry/apps.json`, the release index and `stats.json`, discovery, the catalog website | `deckyhub-registry/README.md`, `deckyhub-registry/.claude/skills/` |

This repository's own files are only `README.md`, `LICENSE.md`, `AGENTS.md`, `CLAUDE.md`, `.gitmodules`, `.claude/skills/` and the two submodule pointers.

## Where a change goes

- **Code, docs, tests, workflows or data inside `deckyhub-plugin/` or `deckyhub-registry/`** belong to that repository. Before editing there, read its guide (table above) and follow its rules: its branches (`develop` for work, `main` for stable), its PR process, its `compatibility` skill. Commit and push from inside the submodule, to that repository. Its rules apply even though you reached it through this repository.
- **Submodule pointers** (which commit each one is pinned to): the `submodules` skill.
- **A change that needs both repositories** (the plugin reading a new registry field, a schema change, renaming something both use): the `cross-repo-change` skill. Order matters, because a registry change on `main` reaches every installed DeckyHub at once.
- **This repository's own docs and skills**: edit them here directly.

## Setup

```bash
git submodule update --init --recursive
git submodule status
```

A submodule checkout is on a detached `HEAD` at its pinned commit. Before working inside one, put it on a branch: `git -C deckyhub-plugin fetch origin && git -C deckyhub-plugin switch -c <topic> origin/develop`.

## Commands

This repository has no build or tests. Each submodule's commands are in its own guide; the main ones:

```bash
# deckyhub-plugin
cd deckyhub-plugin && corepack enable && pnpm install
pnpm run typecheck && pnpm run build && pnpm run test:backend && pnpm run test:frontend

# deckyhub-registry
cd deckyhub-registry && python3 scripts/validate.py
```

## Rules

- Never commit a submodule pointer to a commit that isn't pushed to that submodule's GitHub repository: everyone else's `git submodule update` would fail. Pin to commits on the submodule's `main` unless the user asks for something else.
- Don't copy files between the submodules and this repository. Link to them instead, so there's one copy of each fact.
- Never merge a PR in any of the three repositories without the user's OK.
- Keep the public docs (`README.md`) in plain language for people who aren't developers, as both submodules do.

## Skills

Project skills live in `.claude/skills/`; see [SKILLS.md](SKILLS.md). Each submodule has its own skills in its `.claude/skills/`. Use those whenever you work inside that submodule, and read their `SKILL.md` directly if your tool doesn't load them automatically from a nested directory.

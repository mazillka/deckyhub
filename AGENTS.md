# DeckyHub umbrella agent guide

This file provides guidance to coding agents (including Claude Code) working in this repository.

## What this is

`mazillka/deckyhub` is an umbrella repository with no code of its own. It pins three DeckyHub repositories as git submodules:

| Path | Repository | Its own guide |
| --- | --- | --- |
| `deckyhub-plugin/` | `mazillka/deckyhub-plugin`: the Decky Loader plugin (Python backend `main.py`, React/TypeScript frontend `src/`) | `deckyhub-plugin/AGENTS.md`, `deckyhub-plugin/SKILLS.md`, `deckyhub-plugin/.agents/skills/` |
| `deckyhub-registry/` | `mazillka/deckyhub-registry`: `registry/apps.json`, the release index and `stats.json`, discovery, the catalog website | `deckyhub-registry/AGENTS.md`, `deckyhub-registry/.agents/skills/` |
| `deckyhub-ext/` | `mazillka/deckyhub-ext`: browser extension showing registry security information on GitHub | `deckyhub-ext/AGENTS.md`, `deckyhub-ext/.agents/skills/` |

This repository contains umbrella documentation, agent guidance, and submodule pointers.

## Where a change goes

- **Code, docs, tests, workflows or data inside `deckyhub-plugin/`, `deckyhub-registry/` or `deckyhub-ext/`** belong to that repository. Before editing there, read its guide (table above) and follow its rules: its applicable branch, review, and compatibility guidance. Commit and push from inside the submodule, to that repository. Its rules apply even though you reached it through this repository.
- **Submodule pointers** (which commit each one is pinned to): the `submodules` skill.
- **A change that spans repositories** (the plugin reading a new registry field, a schema change, renaming something both use): the `cross-repo-change` skill. Order matters, because a registry change on `main` reaches every installed DeckyHub at once.
- **This repository's own docs and skills**: edit them here directly.

## Setup

```bash
git submodule update --init --recursive
git submodule status
```

A submodule checkout is on a detached `HEAD` at its pinned commit. Before working inside one, put it on a branch: `git -C deckyhub-plugin fetch origin && git -C deckyhub-plugin switch -c <topic> origin/develop`.

## Commands

This repository has no build or tests. Each submodule's commands are in its own guide; the main ones:

Run each command from the umbrella root:

```bash
# deckyhub-plugin
(cd deckyhub-plugin && corepack enable && pnpm install && pnpm run typecheck && pnpm run build && pnpm run test:backend && pnpm run test:frontend)

# deckyhub-registry
(cd deckyhub-registry && python3 scripts/validate.py)

# deckyhub-ext
(cd deckyhub-ext/extension && npm ci && npm run build-prod)
```

## Rules

- Never commit a submodule pointer to a commit that isn't pushed to that submodule's GitHub repository: everyone else's `git submodule update` would fail. Pin to commits on the submodule's `main` unless the user asks for something else.
- Don't copy files between the submodules and this repository. Link to them instead, so there's one copy of each fact.
- Never merge a PR in any repository without the user's OK.
- Keep the public docs (`README.md`) in plain language for people who aren't developers, as appropriate for their audience.

## Skills

Project skills live in `.agents/skills/`; see [SKILLS.md](SKILLS.md). Some submodules have their own skills in `.agents/skills/`. Use those whenever you work inside that submodule, and read their `SKILL.md` directly if your tool doesn't load them automatically from a nested directory.

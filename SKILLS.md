# DeckyHub umbrella skills

Use these project skills when working in this repository. Each lives in `.agents/skills/<name>/SKILL.md`.

| Skill | Use it when |
| --- | --- |
| [`stay-current`](.agents/skills/stay-current/SKILL.md) | Starting any work here, before reviewing or reading code in a submodule, right after a submodule pull request merges to `main`, or when asked to update or sync: checks the pins and checkouts against each repository's `main`, bumps what's behind, clears out merged worktrees and branches. |
| [`submodules`](.agents/skills/submodules/SKILL.md) | Cloning or initializing the submodules, bumping the pinned commits, working inside a submodule, or fixing a broken or detached submodule checkout. |
| [`cross-repo-change`](.agents/skills/cross-repo-change/SKILL.md) | A change spans repositories, including registry data consumed by the plugin or browser extension. |

## Skills in the submodules

Also follow the relevant skills provided by each submodule:

- `deckyhub-plugin/.agents/skills/`: `branches`, `compatibility`, `deck-test`, `deckyhub-registry`, `localization`, `update-dependencies` (overview in `deckyhub-plugin/SKILLS.md`).
- `deckyhub-registry/.agents/skills/`: `audit-warnings`, `branches`, `compatibility`, `deckyhub-registry`, `review-plugin-submission`, `ship-registry`.
- `deckyhub-ext/.agents/skills/`: `extension-development`, `registry-data`.

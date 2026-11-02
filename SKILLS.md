# DeckyHub umbrella skills

Use these project skills when working in this repository. Each lives in `.claude/skills/<name>/SKILL.md`.

| Skill | Use it when |
| --- | --- |
| [`submodules`](.claude/skills/submodules/SKILL.md) | Cloning or initializing the submodules, bumping the pinned commits, working inside a submodule, or fixing a broken or detached submodule checkout. |
| [`cross-repo-change`](.claude/skills/cross-repo-change/SKILL.md) | A change needs both deckyhub-plugin and deckyhub-registry: the plugin reading new registry data, a registry schema or file change, renaming something both use, or testing the plugin against unpublished registry data. |

## Skills in the submodules

Work inside a submodule follows that repository's own skills, not these:

- `deckyhub-plugin/.claude/skills/`: `branches`, `compatibility`, `deck-test`, `deckyhub-registry`, `localization`, `update-dependencies` (overview in `deckyhub-plugin/SKILLS.md`).
- `deckyhub-registry/.claude/skills/`: `audit-warnings`, `branches`, `compatibility`, `deckyhub-registry`, `review-plugin-submission`, `ship-registry`.

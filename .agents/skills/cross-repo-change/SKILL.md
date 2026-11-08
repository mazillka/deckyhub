---
name: cross-repo-change
description: Plan, order and test a DeckyHub change that spans the plugin, registry, or browser extension - the plugin reading a new apps.json, releases.json or stats.json field, a registry schema or file change, a new audit warning, renaming something both use, or testing the plugin against registry data that isn't published yet. Use whenever one repository's change depends on the other's.
---

# Changes across DeckyHub repositories

The registry is data every installed DeckyHub reads; the plugin is the reader. A registry change on `main` reaches every install, including old releases nobody updates, on their next restart. A plugin change reaches users only as they update. So the two never ship together: the data comes first and stays compatible with every released plugin.

The extension also consumes registry catalog, audit and scan data; check its `extension/src/background.js` and `extension/src/contentscript.js` when changing those contracts. Account for both readers and their cached or older data when relevant.

For plugin and registry changes, read both `compatibility` skills: `deckyhub-registry/.agents/skills/compatibility/SKILL.md` lists every file the plugin reads, from which branch, and the reader in the plugin's code; `deckyhub-plugin/.agents/skills/compatibility/SKILL.md` covers the plugin's side.

## Order

1. **Registry, additive.** Add the new field, file or warning ID beside what exists; never rename or drop one a released DeckyHub reads. PR against `deckyhub-registry`'s `develop`, under its `branches` skill.
2. **Registry ships.** It reaches `main` through the registry's `ship-registry` skill. The opt-in Pre-release registry channel reads `develop`, so testers see it first.
3. **Plugin reads it.** PR against `deckyhub-plugin`'s `develop`. Read only fields already on the registry's `main` (or `develop` for a pre-release), and treat a missing one as absent, since cached and older registry files lack it. Keep `PluginStats` (`src/types.ts`) and `AUDIT_WARNINGS` in step with `stats.json`.
4. **Plugin releases** under its `branches` skill: a pre-release from `develop`, then stable from `main`.
5. **Clean up later, if ever.** Drop an old field only once no supported plugin release reads it, and only with the user's OK on which releases still count.
6. **Bump the umbrella** once both are on `main`: the `submodules` skill.

A rename is the same steps with both names published side by side until step 5.

## Testing both together

This layout puts the repositories side by side, where each one's tests look for the other:

```bash
# registry tests against the plugin's types and readers
cd deckyhub-registry
DECKYHUB_PLUGIN=../deckyhub-plugin python3 -m unittest discover -s tests

# plugin's schema test against the registry's apps.json
cd deckyhub-plugin
DECKYHUB_REGISTRY=../deckyhub-registry/registry/apps.json python3 -m unittest tests.test_registry_schema
```

Each submodule is at its pinned commit. To test a pair of topic branches, check those out in both first (`git -C deckyhub-plugin switch <topic>`), and test the registry against the plugin's `main` too, since that's what users run.

To see the plugin on a Deck against registry data that isn't published, use the plugin's `deck-test` skill.

## In each PR

- Link the other repository's PR, as `mazillka/deckyhub-plugin#N` or `mazillka/deckyhub-registry#N`.
- In the registry PR, say what an older DeckyHub sees after the change ("nothing different" is the usual answer).
- In the plugin PR, say which registry PR or release it needs, and that it's already on the registry branch it reads.

Never merge a PR without the user's OK.

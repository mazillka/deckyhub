---
name: submodules
description: Manage the deckyhub umbrella repo's git submodules (deckyhub-plugin and deckyhub-registry) - initialize them, bump the pinned commits, work and push inside a submodule, and repair a detached, dirty or missing submodule checkout. Use when cloning this repo, when asked to update, bump or sync the submodules, before committing a submodule pointer, or when `git submodule` reports an error.
---

# Submodules

| Path | Repository | Tracks |
| --- | --- | --- |
| `deckyhub-plugin` | `https://github.com/mazillka/deckyhub-plugin.git` | `main` |
| `deckyhub-registry` | `https://github.com/mazillka/deckyhub-registry.git` | `main` |

The umbrella records one commit per submodule. `branch = main` in `.gitmodules` only says which branch `git submodule update --remote` follows; the pinned commit is what everyone gets.

## Set up

```bash
git submodule update --init --recursive
git submodule status          # " " pinned, "+" checked out elsewhere, "-" not initialized
```

## Bump the pinned commits

1. Make sure neither submodule has work in progress: `git submodule foreach 'git status --short'` prints nothing. If one does, it's someone's work: ask before touching it.
2. Move both to the newest `main`:

   ```bash
   git submodule update --remote
   git submodule status
   ```

   For one only: `git submodule update --remote deckyhub-registry`.
3. Show what changed in each, so the commit message and the user can see it: `git diff --submodule=log`.
4. Commit only the pointers:

   ```bash
   git add deckyhub-plugin deckyhub-registry
   git commit -m "chore: bump submodules"
   ```

Pin to a stable release instead of the tip of `main` when the user asks: `git -C deckyhub-plugin checkout v3.1.0`, then `git add deckyhub-plugin`.

Never pin to a commit that isn't on GitHub. Check before committing:

```bash
git -C deckyhub-plugin branch -r --contains HEAD    # must list origin/<branch>
```

Prefer commits on `main`. A pointer to `develop` or a topic branch is fine only when the user asks for it, and it breaks once that branch is squash-merged and deleted, so move it back to `main` afterwards.

## Work inside a submodule

A submodule is a full clone of its repository, checked out on a detached `HEAD`. Changes to it are that repository's changes, under its own rules (`AGENTS.md`, `CONTRIBUTING.md`, its `branches` and `compatibility` skills):

```bash
cd deckyhub-registry
git fetch origin
git switch -c <topic> origin/develop
# edit, test, commit
git push -u origin <topic>
```

Open the PR against that repository's `develop`. The umbrella's pointer doesn't change until the work reaches the submodule's `main`; then bump it as above.

## Repair

| Symptom | Fix |
| --- | --- |
| Empty `deckyhub-plugin/` or `deckyhub-registry/` | `git submodule update --init` |
| `+` in `git submodule status` after a pull | `git submodule update` checks out the pinned commit, if the submodule has no work of its own (`git -C <path> status`, `git -C <path> log origin/develop..HEAD`) |
| `fatal: remote error: upload-pack: not our ref` or `did not contain <sha>` | The pointer names an unpushed or deleted commit. Re-pin to a commit on `main` and commit the fix |
| URL changed in `.gitmodules` | `git submodule sync --recursive`, then `git submodule update --init` |

Never `rm -rf` a submodule directory or `git submodule deinit -f` one without checking it for uncommitted or unpushed work first.

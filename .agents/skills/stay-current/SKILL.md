---
name: stay-current
description: Keep the deckyhub umbrella repo up to date - check on every session start that the submodule pins match each repository's main and that the local checkouts aren't stale, bump the pins when they're behind, and clear out merged worktrees and branches. Use at the start of any work in this repository, before reviewing, auditing or reading code in a submodule, right after a submodule pull request merges to main, and when asked to update, sync or refresh the repo.
---

# Stay current

The umbrella goes stale silently: a pull request merges in a submodule's repository and nothing here moves. Stale pins give everyone old code; a stale checkout makes a review find bugs that are already fixed. Run the check first, every time.

## 1. Check (changes nothing)

```bash
git fetch -q origin && git status -sb | head -1                 # the umbrella itself: "behind N"?
git submodule foreach -q 'git fetch -q --prune origin'
for d in deckyhub-plugin deckyhub-registry deckyhub-ext; do
  echo "$d pinned $(git rev-parse --short HEAD:$d), main $(git -C $d rev-parse --short origin/main), checked out $(git -C $d rev-parse --short HEAD) ($(git -C $d branch --show-current || echo detached))"
done
git status --short                                              # anything untracked at the root?
```

- **pinned ≠ main**: the pin is behind. Go to step 2.
- **checked out ≠ what you mean to read**: say which commit you're reading before reviewing anything. For "the latest code", read `origin/develop` where the repository has one (plugin, registry), else `origin/main`, plus any open pull request the user names; not whatever branch happens to be checked out.
- **Untracked folders at the root**: scratch worktrees. `.gitignore` keeps them out of commits; step 3 removes the finished ones.

## 2. Bump the pins

Follow the [`submodules`](../submodules/SKILL.md) skill, "Bump the pinned commits". In short: on a topic branch (never commit on `main`), pin each submodule to its `origin/main`, show `git diff --cached --submodule=log`, commit `chore: bump submodules`, open a pull request. A submodule with work in progress keeps its checkout; set only its pin:

```bash
git update-index --cacheinfo 160000,$(git -C deckyhub-plugin rev-parse origin/main),deckyhub-plugin
```

Bump right after a submodule's pull request reaches its `main`; that's part of finishing that change, not a later chore.

## 3. Clear out finished worktrees and branches

Squash merges leave no trace in history, so judge each branch by its pull request, and remove it only when both hold:

```bash
gh pr list -R mazillka/<repo> --state all --head <branch> --json state,headRefOid   # MERGED, and headRefOid is the local tip
git -C <worktree> status --porcelain                                                # prints nothing
```

Then `git -C deckyhub-<repo> worktree remove ../<folder>` and `git -C deckyhub-<repo> branch -D <branch>`. Leave anything with an open pull request, no pull request, a tip the pull request doesn't have, or uncommitted files, and tell the user about it.

- Never delete a worktree folder with `rm -rf`: its `node_modules` can be a link into a submodule's own, and a recursive delete follows it. `git worktree remove` is safe; remove a leftover link on Windows with `[IO.Directory]::Delete(path, $false)`.
- New scratch worktrees go outside this repository (`git -C deckyhub-plugin worktree add ../../deckyhub-worktrees/<topic>`), not at its root.

## 4. Report

One or two lines: what was behind, what you bumped or removed, and what you left alone and why.

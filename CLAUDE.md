# pidog — agent instructions

## Git workflow (all agents, all projects)

Every code change ships on a branch, then merges to main. Never commit directly
on main.

1. `git checkout main && git pull --ff-only`
2. `git checkout -b feat|fix|chore/<short-kebab>` — never commit on main
3. Implement, with tests where the repo has them. **Stage only the files you
   meant to change** — `git add <paths>`, never `git add -A` or
   `git commit -am`. More than one agent may be working in this checkout at the
   same time, and `-a` sweeps their in-flight edits into your commit.
4. Commit on the branch. Pull main again, then merge the branch into main
   (no PR required unless Max asks).
5. `git push origin main`
6. Deploy only what changed. For Firebase projects: hosting for UI,
   functions/rules only when those files changed.
7. Verify the live result. If it is broken, revert the merge — the branch is
   the safety net.
8. Delete the local branch after a good merge.

Never force-push main.

### Working alongside other agents

The working tree is shared. Before committing, run `git status` and commit only
your own paths. If files you did not touch are modified, leave them alone and
say so rather than committing or reverting them. If a branch checkout fails
because of someone else's uncommitted changes, do not stash them — report it.

### Before merging an old branch

Check that it is actually mergeable, not merely conflict-free:

- `git merge-base main <branch>` — no output means unrelated history. Do not
  merge it.
- `git rev-list --count <branch>..main` — hundreds of commits behind is a
  warning, not a detail.
- `git diff --stat main...<branch>` — if it edits paths that no longer exist,
  git re-creates them as new files. That merges cleanly and leaves a duplicate
  dead tree with no conflict to warn you.

A stale branch worth keeping but not worth merging gets archived, not merged:

```bash
git tag -a archive/<name> <sha> -m "Archived <date>: <why>"
git push origin archive/<name>
git push origin --delete <name>
git branch -D <name>
```

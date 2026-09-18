# pidog — agent instructions

## Git workflow (all agents, all projects)

Canonical policy: [https://github.com/max-knox/maxopenclaw/blob/main/docs/AGENT-GIT-WORKFLOW.md](https://github.com/max-knox/maxopenclaw/blob/main/docs/AGENT-GIT-WORKFLOW.md) — read it before your first commit here.
The short version, which holds even if you cannot open that link:

- **Branch, then merge.** `git checkout -b feat|fix|chore/<short-kebab>` off an
  up-to-date main. Never commit directly on main. Merge to main when it works,
  push, then deploy only what changed. Never force-push main.
- **Stage explicit paths.** `git add <paths>` — never `git add -A` or
  `git commit -am`. This checkout may be shared with other agent sessions, and
  `-a` sweeps their in-flight edits into your commit.
- **Do not merge a stale branch** without checking `git merge-base main <branch>`
  and whether the paths it touches still exist. A branch that edits deleted
  directories merges cleanly and leaves a duplicate dead tree.

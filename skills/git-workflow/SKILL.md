---
name: git-workflow
description: 'Checkpoint workflow for git: user-initiated commits, WIP save points between iterations, remote sync left to user.'
---

### Commit Rule

Agent creates commits only on explicit user request.

Completion criterion: user's changes committed with user's message.

### Checkpoint Rule

After each iteration, before the next user message or pause:

```bash
git commit -am "WIP" --no-verify
```

Completion criterion: WIP commit created.

### Squash Rule

When the user requests a commit, squash all WIP checkpoints into a single commit:

```bash
WIP_COUNT=$(git log --oneline | grep -c "^[a-f0-9]* WIP$")
git reset --soft HEAD~$WIP_COUNT
git commit -m "<user message>"
```

Completion criterion: no WIP commits remain in `git log`.

### Remote Rule

Remote synchronization (pull, push, fetch, merge, rebase) is the user's responsibility.

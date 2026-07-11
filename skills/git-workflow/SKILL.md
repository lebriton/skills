---
name: git-workflow
description: 'Checkpoint workflow for git: WIP save points between iterations, real commits on explicit request only, remote sync left to user.'
---

# Git Workflow

## Commit Rule

Two types of commits, two different rules:

- **Checkpoint** — always. After every change, before the next user message or pause:

  ```bash
  git commit -am "WIP" --no-verify
  ```

- **Squash** — on explicit user request only. The agent writes the conventional commit message.

  ```bash
  WIP_COUNT=$(git log --oneline | grep -c "^[a-f0-9]* WIP$")
  git reset --soft HEAD~$WIP_COUNT
  git commit -m "<conventional commit message>"
  ```

`--no-verify` is reserved for WIP only.

Completion criterion: no WIP commits remain in `git log`.

## Remote Rule

Remote synchronization (pull, push, fetch, merge, rebase) is the user's responsibility.

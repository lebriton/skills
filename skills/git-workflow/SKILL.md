---
name: git-workflow
description: 'Checkpoint git: fires on commit, push, or squash.'
---

# Git Workflow

## Gate

Before any `git commit` — this skill is mandatory. Every commit follows the checkpoint pattern below.

## Checkpoint

After every change, before the next user message or pause — commit all modifications:

```bash
git commit -am "WIP" --no-verify
```

Verify: `git status` shows no dirty tracked files.

## Squash

On explicit user request only. Consolidate all WIP checkpoints into one conventional commit:

```bash
WIP_COUNT=$(git log --oneline | grep -c "^[a-f0-9]* WIP$")
git reset --soft HEAD~$WIP_COUNT
git commit -m "<conventional commit message>"
```

Verify: no WIP commits remain in `git log`.

## Pre-commit

If pre-commit is installed, hooks must pass on every conventional commit. Never use `--no-verify` or `-n` to skip hooks, unless explicitly insisted by the user.

## Merge / Pull

Use `--ff-only` for all merges. Use `--rebase` for all pulls. No merge commits or non-ff rebases allowed.

## Remote

All remote operations (push, pull, fetch, merge, rebase) require explicit user instruction.

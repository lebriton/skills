---
name: git-workflow
description: 'Strict Git rules: never auto-commit, use git wip between iterations, only commit on explicit user request, never use sync commands (pull, push, fetch, merge, rebase).'
---

### Core Rules

```xml
<rules>
    <rule id="1" priority="absolute">
        NEVER commit automatically. Wait for an explicit user request.
    </rule>
    <rule id="2" priority="high">
        Between each iteration, run: git commit -am "WIP" --no-verify
        This saves progress without creating a permanent commit.
    </rule>
    <rule id="3" priority="absolute">
        STRICTLY FORBIDDEN to use any remote synchronization commands:
        - git pull
        - git push
        - git fetch
        - git merge
        - git rebase
        Any command that touches a remote repository is forbidden.
    </rule>
    <rule id="4" priority="absolute">
        Only create a commit when the user explicitly asks for it.
        Do not suggest, propose, or anticipate.
    </rule>
</rules>
```

### Workflow Between Iterations

```xml
<workflow>
    <step>1. Work on the task requested by the user</step>
    <step>2. Before each new iteration or pause, run:
        git commit -am "WIP" --no-verify
    </step>
    <step>3. Never commit, push, pull, fetch, merge, or rebase</step>
    <step>4. If the user asks for a commit, only then create one</step>
</workflow>
```

### Automatic Checks

Before executing any git command, the AI must verify:

1. Is it a commit? → DENY (unless explicitly requested)
2. Is it a pull/push/fetch/merge/rebase? → DENY (always)
3. Is it a git commit with message "WIP" and --no-verify? → ALLOWED (between iterations)
4. Is it a git status/diff/log? → ALLOWED (read-only)

### Enforcement

Any violation of these rules must be immediately corrected. Forbidden commands must never be suggested or executed.

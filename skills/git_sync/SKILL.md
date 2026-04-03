---
name: git-sync
description: Synchronize the dedicated notes repository with git before and after note updates. Use when Codex needs to run pull, stage only relevant files, commit note or memory changes, and push safely without touching unrelated project repositories.
---

# Git Sync

Keep the dedicated notes repository synchronized safely and predictably.

## Scope

Operate only in the dedicated notes repository.

Never run note-storage git operations in the source project repository unless the user explicitly asks for that.

## Workflow

1. Run `git pull` before editing.
2. If pull fails because of conflicts, missing remote configuration, or local changes that need resolution, stop and report the issue.
3. After note or memory edits, stage only the relevant files.
4. Commit with a precise message that matches the change.
5. Push to the tracked branch.

## Guardrails

Never use destructive git commands.

Never stage unrelated files just because they are present.

If the worktree is dirty, inspect the changed files and stage only the note-related paths.

If commit or push fails, report the exact failing step.

## Commit Patterns

Preferred commit styles:
- `AI: add/update note <topic>`
- `AI: update memory <topic>`
- `AI: sync notes repository`

## Output Expectations

When responding:
- say whether pull succeeded
- list the staged files or summarize them
- say whether commit succeeded
- say whether push succeeded

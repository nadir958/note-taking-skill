# Note Taking Skill Suite

This repository contains a small Codex skill suite for centralized project memory:

- `skills/note_taking`: capture durable notes from normal project discussions
- `skills/memory_manager`: decide whether durable memory belongs in `RULES.json` or `DIARY.json`
- `skills/git_sync`: safely pull, commit, and push the dedicated notes repository

The goal is to store important project memory in one separate git repository instead of scattering it across many project repos.

## How It Works

The intended flow is:

1. Work normally in any project repository.
2. When the discussion produces durable information, `note-taking` saves it to `NOTE_TAKING_REPO`.
3. `memory-manager` updates durable memory files when needed.
4. `git-sync` pulls, commits, and pushes the notes repository.

Examples of durable project memory:

- architecture decisions
- debugging conclusions
- recurring workflows
- important TODOs
- preferences that affect future work
- research summaries and tradeoffs

Examples of content that should usually not be saved:

- casual chat
- temporary back-and-forth
- low-value execution noise
- explicitly private or excluded content

## Repository Layout

```text
skills/
  note_taking/
  memory_manager/
  git_sync/
```

Each skill contains:

- `SKILL.md`: the actual skill instructions
- `agents/openai.yaml`: UI metadata for Codex

## Installation

Install the skills into your local Codex skills directory.

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$HOME/.codex/skills/note-taking/agents" -Force | Out-Null
New-Item -ItemType Directory -Path "$HOME/.codex/skills/memory-manager/agents" -Force | Out-Null
New-Item -ItemType Directory -Path "$HOME/.codex/skills/git-sync/agents" -Force | Out-Null

Copy-Item -LiteralPath ".\skills\note_taking\SKILL.md" -Destination "$HOME/.codex/skills/note-taking\SKILL.md" -Force
Copy-Item -LiteralPath ".\skills\note_taking\agents\openai.yaml" -Destination "$HOME/.codex/skills/note-taking\agents\openai.yaml" -Force

Copy-Item -LiteralPath ".\skills\memory_manager\SKILL.md" -Destination "$HOME/.codex/skills/memory-manager\SKILL.md" -Force
Copy-Item -LiteralPath ".\skills\memory_manager\agents\openai.yaml" -Destination "$HOME/.codex/skills/memory-manager\agents\openai.yaml" -Force

Copy-Item -LiteralPath ".\skills\git_sync\SKILL.md" -Destination "$HOME/.codex/skills/git-sync\SKILL.md" -Force
Copy-Item -LiteralPath ".\skills\git_sync\agents\openai.yaml" -Destination "$HOME/.codex/skills/git-sync\agents\openai.yaml" -Force
```

After installation, restart Codex if the new skills do not appear immediately.

## Create The Dedicated Notes Repository

Create a separate git repository for centralized notes. A minimal structure is:

```text
note-taking-repo/
  notes/
  RULES.json
  DIARY.json
```

Recommended initial file contents:

`RULES.json`

```json
[]
```

`DIARY.json`

```json
[]
```

Track the `notes/` directory with a placeholder such as `.gitkeep` if it is empty.

## Configure NOTE_TAKING_REPO

Set `NOTE_TAKING_REPO` to the local path of your dedicated notes repository.

Windows PowerShell:

```powershell
setx NOTE_TAKING_REPO "C:\path\to\note-taking-repo"
```

For the current shell session only:

```powershell
$env:NOTE_TAKING_REPO = "C:\path\to\note-taking-repo"
```

Restart Codex after `setx` if you want the app to pick up the new persistent value cleanly.

## Usage

You can use the suite in two ways.

Explicit usage:

- `Use $note-taking for this discussion.`
- `Use $memory-manager to decide whether this should update durable memory.`
- `Use $git-sync to commit and push the notes repository.`

Implicit usage:

- the `note-taking` skill is designed to trigger during normal project work when the conversation contains durable memory worth saving
- it should skip transient or explicitly excluded content

## Expected Note Format

Notes are intended to be written under:

```text
/notes/YYYY-MM-DD-topic.md
```

Typical structure:

```markdown
# Title

## Source
- Project: ...
- Repository: ...

## Summary
- ...

## Key Ideas
- ...

## Actions
- [ ] ...

## Decisions
- ...

## Context
- ...
```

## Git Behavior

The suite is intended to:

- run `git pull` in the dedicated notes repository before editing
- stage only note-related files
- commit with messages such as `AI: add/update note <topic>`
- push the notes repository after successful updates

It should not:

- write notes into the source project unless explicitly requested
- stage unrelated files
- use destructive git commands

## Privacy And Security

- keep the dedicated notes repository private if it contains internal project memory
- do not store secrets, tokens, passwords, or other sensitive credentials in notes
- if a token was exposed while setting this up, revoke it and create a new one

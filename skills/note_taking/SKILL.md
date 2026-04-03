---
name: note-taking
description: Capture important information from any project discussion and persist it into a dedicated Git-based knowledge repository as structured markdown with optional memory updates and a git commit. Use not only when Codex is explicitly asked to take notes, but also during normal work whenever the conversation contains durable project information such as requirements, plans, debugging conclusions, implementation decisions, action items, research findings, preferences, or lessons that should be remembered centrally across projects instead of staying only in the current thread.
---

# Note Taking

Convert a raw note into a clean markdown entry inside the dedicated notes repository, update durable memory files only when warranted, and record the change in git.

Prefer concise notes that preserve meaning, decisions, and follow-up work.

Rule: always persist durable project memory to `NOTE_TAKING_REPO` during normal project discussions. Skip only transient, casual, explicitly private, or explicitly excluded content.

Default to implicit note capture. If the discussion contains durable information worth remembering, treat that as note-taking work even when the user does not explicitly ask for notes.

Skip note capture only when the exchange is clearly transient, casual, or purely procedural with no future memory value, or when the user explicitly says not to save notes.

## Workflow

1. Identify the source project the note came from.
2. Identify the dedicated notes repository.
3. Prefer an explicit configured path such as `NOTE_TAKING_REPO`. If none is provided, stop and ask for the dedicated repository path instead of guessing.
4. Confirm the target knowledge structure exists or infer it from nearby files in the dedicated notes repository.
5. Pull the latest changes in the dedicated notes repository with `git pull` before editing.
6. Analyze the raw note for:
   - topic
   - date context
   - key ideas
   - actions
   - decisions
   - recurring rules or heuristics
   - reasoning insights worth preserving
7. Write or update the daily note file at `/notes/YYYY-MM-DD-topic.md` inside the dedicated notes repository.
8. Include source context so the note can be traced back to the originating project.
9. Update memory files only if the note contains durable information that belongs there.
10. Stage only the files relevant to the note update in the dedicated notes repository.
11. Commit with `AI: add/update note <topic>`.
12. Push the dedicated notes repository when possible.

Coordinate with:
- `$memory-manager` for deciding whether `RULES.json` or `DIARY.json` should change
- `$git-sync` for the pull, stage, commit, and push steps

## Analyze The Note

Infer a short topic slug from the dominant subject. Use lowercase hyphenated words and keep it specific enough to be useful in filenames.

Extract these buckets from the raw text:

- `Key Ideas`: facts, observations, hypotheses, summaries
- `Actions`: concrete next steps, owners, deadlines if present
- `Decisions`: chosen options, commitments, conclusions
- `Context`: supporting details that help the note make sense later

If the note contains no explicit action or decision, omit those sections instead of inventing content.

Prefer capturing information from these common discussion types:
- bug investigations that reached a conclusion
- implementation plans and architecture choices
- user preferences that affect future work
- project-specific conventions or workflows
- research summaries and tradeoff decisions
- TODOs or follow-up items with clear next actions

## Write The Note

Use this structure unless the repository already uses a different house style:

```markdown
# <Readable Title>

## Source
- Project: <source project name or path>
- Repository: <source repository path or remote>

## Summary
- <1-3 bullets capturing the note at a glance>

## Key Ideas
- ...

## Actions
- [ ] ...

## Decisions
- ...

## Context
- ...
```

Use the note path `/notes/YYYY-MM-DD-topic.md` inside the dedicated notes repository, where the date is the effective date of the note, not necessarily the current date if the user clearly refers to a different day.

If the target file already exists:
- read it first
- preserve existing content
- append a new section or merge new bullets into the relevant section
- avoid duplicating identical bullets
- prefer chronological additions with a timestamped subheading when multiple entries land in the same file

## Update Memory Files

Update `RULES.json` only when the note reveals a reusable rule, recurring pattern, standing preference, or operating heuristic that should influence future work.

Update `DIARY.json` only when the note contains a reasoning insight, retrospective lesson, or reflection that is useful as durable internal memory.

If those files do not exist, create them only when the dedicated notes repository already uses that convention or the user explicitly wants them. Otherwise skip the memory update.

Keep memory entries compact and durable. Do not copy transient meeting chatter into memory files.

## Git Behavior

Run `git pull` in the dedicated notes repository before editing. If pull fails because of conflicts, local changes, or missing remote configuration, stop and report the issue instead of forcing the repository state.

After edits:
- stage only the changed note and memory files
- commit with `AI: add/update note <topic>`
- push when possible

Never write notes into the source project unless the user explicitly asks for that. Never overwrite unrelated user changes, never use destructive git commands, and never fabricate a successful commit or push.

## Output Expectations

When responding to the user:
- show the note path you created or updated
- show which dedicated notes repository was used
- summarize the extracted key ideas, actions, and decisions
- mention whether `RULES.json` or `DIARY.json` changed
- mention whether git commit and push succeeded or why they did not

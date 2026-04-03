---
name: memory-manager
description: Manage durable memory files for a centralized note system, especially RULES.json and DIARY.json. Use when Codex needs to decide whether a note contains a reusable rule, a recurring pattern, or a reasoning insight that should be persisted separately from ordinary notes.
---

# Memory Manager

Maintain durable memory for the centralized notes repository without polluting it with transient discussion details.

## Scope

Operate only inside the dedicated notes repository.

Manage:
- `RULES.json` for stable operating rules, standing preferences, and recurring patterns
- `DIARY.json` for compact reasoning insights, retrospectives, and lessons learned

## Decision Rules

Write to `RULES.json` when the information is:
- reusable across future tasks
- specific enough to guide behavior
- likely to remain valid beyond the current conversation

Write to `DIARY.json` when the information is:
- a reflection on what worked or failed
- a reasoning shortcut or lesson
- useful for future judgment but not a hard rule

Do not write memory entries for:
- one-off status updates
- temporary blockers
- raw meeting chatter
- facts already fully captured in a dated note

## Update Behavior

Read the current file before editing.

Preserve existing entries.

Append or merge intelligently:
- avoid duplicates
- keep entries concise
- normalize wording so similar entries converge instead of multiplying

If the repository uses arrays of objects, preserve that shape.
If the repository uses a different JSON schema, follow the existing schema instead of forcing a new one.

## Output Expectations

When responding:
- say whether `RULES.json` changed
- say whether `DIARY.json` changed
- briefly justify why the information qualified as durable memory

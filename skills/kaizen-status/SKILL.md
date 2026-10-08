---
name: kaizen-status
description: Show where Kaizen stands in this repo — open units, next eligible item, recent log entries, and time since the last pass. Read-only.
---

# Status

Read-only summary of the repo's Kaizen state. Changes nothing.

## Steps

1. Find `*/kaizen/kaizen.json`. If none, say Kaizen is not set up and point to
   `kaizen-init`.
2. Read `kaizen.json`, `backlog.md`, the last five `log.md` entries and the latest
   `passes/` record.
3. Check the setup is intact: the `<!-- kaizen:begin -->` block is in `CLAUDE.md`,
   and `.claude/settings.json` enables `kaizen@<marketplace>`. Report anything
   missing and suggest `kaizen-init` to repair it.
4. Report, briefly:
   - baseline date and the latest pass (ID, date, commits since its `end_commit`);
   - open units by priority, any `in-progress` or `blocked` ones;
   - the next eligible unit — first `open` item whose dependencies are `done`;
   - the last few log entries;
   - count of *Noticed — out of scope* items not yet handed off.

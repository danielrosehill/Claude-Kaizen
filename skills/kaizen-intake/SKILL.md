---
name: kaizen-intake
description: Split improvement ideas into atomic Kaizen backlog units, without implementing. Use whenever the user hands over a list of improvements.
---

# Intake: turn ideas into atomic units

Converts improvement ideas — a pasted list, a paragraph, review comments, findings
from a pass — into atomic units in `backlog.md`. **Intake never implements
anything.** This is the gate that stops a long list becoming one sprawling change.

## Inputs

- The ideas, in whatever form the user gave them.
- `source` for the new items: `user <YYYY-MM-DD>` for user lists, `baseline` or
  `P<NNN>` when called from a pass.

## Steps

1. **Locate Kaizen**: find `*/kaizen/kaizen.json` in the repo. If none, run
   `kaizen-init` first. Read `PRINCIPLES.md` and `backlog.md`.

2. **Split.** For each idea, apply the atomicity test in `PRINCIPLES.md`. Split on
   every "and", every distinct file group with a distinct purpose, and every step
   that could ship alone. Prefer more, smaller units. Where one unit only makes
   sense after another, set `depends-on`.

3. **Filter.** For each candidate:
   - a defect or a new feature → *Noticed — out of scope*, not a unit;
   - already in the backlog (open or closed) → skip, or note it as a duplicate;
   - too vague to verify → ask the user one question, or write the most
     reasonable concrete reading and mark it so in `why`.

4. **Ground each unit in the code.** Look at the files it touches so `scope` and
   `verify` are concrete, not guessed.

5. **Write the items** to the `## Open` section in the format in `backlog.md`,
   taking IDs from `kaizen.json` `next_item_id` and incrementing it. Order the open
   list by priority, then dependency.

6. **Show the user** the new units as a short table — ID, title, priority,
   depends-on — plus anything filed as out of scope. Ask whether to reorder or drop
   any. Do not start implementing.

7. **Commit** the backlog change alone: `kaizen: intake <N> units`.

## Notes

- One original idea commonly becomes three to eight units. That is the point.
- Keep the user's wording visible in `why` where it carries intent, so the split
  can be checked against what was asked.

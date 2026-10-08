---
name: kaizen-step
description: Implement exactly one Kaizen backlog unit — change, verify, commit, log — then stop. Use for "next improvement", "do K-012", "next N".
---

# Step: ship one atomic unit

The unit of Kaizen work. One invocation implements **one** backlog item end to end.
If the user asks for several, run this whole procedure once per item, in sequence,
finishing each before starting the next.

## Inputs

- Optional: a specific item ID (`K-012`). Otherwise the next eligible item.
- Optional: a count ("next 3"). Each is a separate full cycle.

## Steps

1. **Locate Kaizen**: find `*/kaizen/kaizen.json`; read `PRINCIPLES.md`,
   `backlog.md` and `kaizen.json`. If none exists, run `kaizen-init`.

2. **Check the working tree** with `git status --porcelain`. If it is not clean,
   stop and ask — unrelated changes must not leak into a Kaizen commit.

3. **Pick the item.** The named one, or the first `open` item in `## Open` whose
   `depends-on` items are all `done`. Re-read it against the atomicity test. If it
   is not atomic, split it (as `kaizen-intake` does), commit the backlog change, and
   pick the first new item instead.

4. **Mark it `in-progress`** in `backlog.md`.

5. **Implement only this item.** Stay inside its `scope`. Anything else you notice
   goes into `backlog.md` as a new open item (or *Noticed — out of scope*) — not into
   the code. If the change grows beyond one purpose, stop: revert, split the item,
   and restart at step 3.

6. **Verify**: run the item's `verify` check and the repo's `verify_command` from
   `kaizen.json`, if set. Both must pass. On failure, fix within scope; if that is
   not possible, revert the code, set the item to `blocked` with the reason, log and
   commit it (steps 7–8) and stop.

7. **Close and log.** Move the item to `## Closed` with `status: done`, and append
   an entry to `log.md` in its documented format.

8. **Commit** the code change, the backlog change and the log entry as one commit:
   `kaizen(K-NNN): <item title>`. New backlog items found in step 5 may ride in the
   same commit — they are bookkeeping, not code. The log does not carry a sha (a
   commit cannot contain its own); the unit's commit is
   `git log --grep '^kaizen(K-NNN)'`. For a `blocked` or `dropped` unit, commit the
   backlog and log change alone as `kaizen(K-NNN): blocked` / `dropped`.

9. **Report** in two or three lines: what changed, how it was verified, the commit,
   and what the next eligible item is. Then stop, unless a count remains.

## Notes

- Push only if the repo's own instructions say to; Kaizen does not change the
  project's release habits.
- "Do them all" still means one item per commit, in order, verifying each. If the
  user wants a long run, offer to continue item by item and report after each.

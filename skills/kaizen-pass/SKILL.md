---
name: kaizen-pass
description: Run a follow-up Kaizen pass — review what changed since the last pass, report the delta, and add new atomic units to the backlog.
---

# Pass: a follow-up survey (P001, P002, …)

Each pass picks up exactly where the previous one ended. It measures progress
against the last pass and looks for the next round of improvements, with the
most attention on what changed since.

Requires a baseline. If `kaizen.json` has no `baseline`, run `kaizen-baseline`.

## Inputs

- Optional: areas to focus on, or a list of ideas from the user to include.

## Steps

1. **Locate Kaizen**: find `*/kaizen/kaizen.json`; read `PRINCIPLES.md`,
   `backlog.md`, `log.md`, and the most recent `passes/` record.

2. **Require a clean tree.** `PREV` = the `end_commit` of the latest entry in
   `passes` (or of `baseline` if `passes` is empty). `HEAD` = `git rev-parse HEAD`.
   If `PREV` is not an ancestor of `HEAD` (`git merge-base --is-ancestor`), history
   was rewritten: say so and use the nearest commit by date.

3. **Measure the delta** since `PREV`:
   - units shipped, blocked, dropped — from `log.md` entries dated after the last
     pass and from `git log --grep '^kaizen(' PREV..HEAD`;
   - units still open, and how long the oldest has waited;
   - non-Kaizen changes — `git log --invert-grep --grep '^kaizen' PREV..HEAD` and
     `git diff --stat PREV..HEAD`. These are where new improvement opportunities
     and drift are most likely.

4. **Re-validate open items** against the current code. Items made obsolete by
   other work → `dropped` with the reason, logged. Items whose `scope` has moved →
   update `scope`.

5. **Survey**, weighted towards files changed since `PREV`, then any area the
   previous pass did not cover. Same lenses as the baseline survey. Record evidence.

6. **Triage** findings and any user ideas through `kaizen-intake` with
   `source: P<NNN>` (from `next_pass_id`).

7. **Write `passes/P<NNN>-<YYYY-MM-DD>.md`**:

   ```markdown
   # P<NNN> — <YYYY-MM-DD>
   - start_commit: <PREV>
   - end_commit: <HEAD>
   - verify_command: <command> → <result>
   ## Delta since P<prev>
   - shipped: <K-IDs>
   - blocked / dropped: <K-IDs with reasons>
   - non-Kaizen commits: <count, with notable ones>
   - open backlog: <count before> → <count after>
   ## Findings
   <one line per finding with evidence, and the K-IDs it became>
   ## Out of scope
   <defects and feature ideas noticed>
   ```

8. **Update `kaizen.json`**: append
   `{"pass": "P<NNN>", "date": "<ISO>", "start_commit": "<PREV>", "end_commit": "<HEAD>"}`
   to `passes` and increment `next_pass_id`. Commit pass record, backlog, log and
   state as `kaizen: pass P<NNN>`.

9. **Report** the delta and the new units, then offer `kaizen-step`. A pass does not
   implement units itself.

## Notes

- A pass with no new findings is a valid result. Record it; do not invent work.
- If the open backlog keeps growing pass over pass, say so — the user may want to
  drop low-priority items rather than let the backlog rot.

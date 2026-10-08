---
name: kaizen-baseline
description: First Kaizen pass on a mature repo — survey the code or take a user list, triage an atomic backlog, ship one unit, record the baseline.
---

# Baseline: the first pass (P000)

Establishes the starting point that every later pass measures from. It records the
state of the codebase, builds the first backlog, and ships the first improvement so
the cycle is proven end to end.

Run once per repository. If `kaizen.json` already has a `baseline`, use `kaizen-pass`
instead.

## Inputs

- Optional: an initial list of improvements from the user. If given, it seeds the
  backlog and the survey only supplements it.
- Optional: areas to focus on or leave alone.

## Steps

1. **Locate Kaizen**: find `*/kaizen/kaizen.json`. If none, run `kaizen-init` first.
   Read `PRINCIPLES.md`. If `baseline` is already set, stop and point to
   `kaizen-pass`.

2. **Require a clean tree**, then record `start_commit` = `git rev-parse HEAD`.

3. **Confirm the project works.** Run `verify_command` from `kaizen.json`, if set.
   If it fails, stop: Kaizen starts from a working project. Report the failure as a
   defect for the normal process.

4. **Survey** — skip or narrow this if the user's list is the whole brief. Read
   enough to form a view of: structure and module boundaries, duplication, naming
   and readability hot spots, dead code, test coverage gaps, slow or fragile
   tooling, outdated dependencies, error handling, docs that have drifted from the
   code, and anything in UX or accessibility that is cheap to improve. For a large
   repo, survey areas in parallel with subagents and merge their findings.
   Record evidence (file and line, or the command output) for each finding.

5. **Triage** the findings and the user's list through `kaizen-intake`, with
   `source: baseline`. Defects and feature ideas go to *Noticed — out of scope*.

6. **Write `passes/P000-baseline.md`**:

   ```markdown
   # P000 — baseline — <YYYY-MM-DD>
   - start_commit: <sha>
   - end_commit: <sha after the first unit>
   - verify_command: <command> → <result>
   - scope: <whole repo | the areas surveyed>
   ## State of the codebase
   <short description of the project as it stands: stack, size, structure,
   test and tooling state — the reference later passes compare against>
   ## Findings
   <one line per finding with evidence, and the K-IDs it became>
   ## Out of scope
   <defects and feature ideas noticed>
   ## First unit
   K-NNN — <title>
   ```

   Commit the pass record and backlog as `kaizen: baseline survey (P000)`.

7. **Ship the first unit** with `kaizen-step`: the highest-priority open item with
   no dependencies, preferring a small, low-risk one so the cycle is proven
   cleanly.

8. **Close the baseline**: set `end_commit` in the pass record; in `kaizen.json` set
   `baseline` to `{"pass": "P000", "date": "<ISO>", "start_commit": "<sha>", "end_commit": "<sha>"}`
   and `next_pass_id` to `1`. Commit as `kaizen: close baseline (P000)`.

9. **Report**: backlog size by priority, the first unit shipped, anything out of
   scope the user should know about, and that `kaizen-step` continues the work.

## Notes

- The survey proposes; it does not fix. Nothing other than the single first unit is
  implemented during the baseline.
- Keep the *State of the codebase* section factual and short — it is the reference
  point for every later delta.

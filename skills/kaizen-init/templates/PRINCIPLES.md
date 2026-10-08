# Kaizen principles

This repository is under Kaizen: continuous, incremental improvement of a project
that already works. These rules govern every improvement made through the Kaizen
log in this folder. They are written for an AI agent arriving cold, and they hold
whether or not the Kaizen plugin is installed.

## What Kaizen is for

Most work on a codebase adds a feature or fixes a defect. Kaizen is the third kind:
making a working, mature project slightly better — clearer, faster, safer to
change, easier to run — without changing what it is for.

Kaizen is **not** the place for:

- **New features.** If an idea adds capability, record it under *Noticed — out of
  scope* in `backlog.md` and leave it for the normal feature process.
- **Bug fixes.** A defect found during a Kaizen pass is recorded under *Noticed —
  out of scope* and reported to the user. It is not silently fixed inside an
  improvement commit.
- **Rewrites.** Anything that cannot be reached by a sequence of small,
  independently shippable steps is not a Kaizen item.

## The one rule: one atomic unit at a time

Every improvement is broken into **atomic units** before any of it is implemented,
and units are implemented **one at a time**: implement, verify, commit, log — then,
and only then, the next.

A unit is atomic when all of these hold:

1. **One purpose.** Its title is a single sentence with no "and". "Extract date
   parsing into `utils/dates.ts`" is atomic. "Extract date parsing and add tests"
   is two units.
2. **One commit.** It lands as exactly one commit that contains nothing else.
3. **Independently revertible.** Reverting that commit leaves the project working.
4. **Verifiable.** It states how to check it worked — a command, a test, an
   observable behaviour — before work starts.
5. **Behaviour-preserving unless stated.** If the unit changes observable
   behaviour, the item says so explicitly and says what changes.

Size is a consequence, not the test. A one-purpose change that touches thirty files
(a rename) can be atomic; a three-line change that does two things is not.

## Rules while implementing a unit

- **Do only the unit.** If you notice another improvement mid-change, do not make
  it. Add it to `backlog.md` and carry on.
- **If the unit turns out not to be atomic, stop.** Revert or stash the partial
  work, split the item in `backlog.md`, and start on the first of the new items.
- **Never batch.** A request to "do the next five" means five complete
  implement→verify→commit→log cycles in sequence, never one combined change.
- **A failed verification is not a commit.** Fix within the unit's scope, or revert
  and mark the item `blocked` with the reason.
- **Commit message:** `kaizen(K-NNN): <item title>`.

## Lists from the user

When the user hands over a list of improvements, however long or loosely worded,
the first action is **intake, not implementation**: split it into atomic units,
write them to `backlog.md`, and show the user the result. Implementation starts
only when a unit is picked.

## Passes and the baseline

- **Baseline (pass P000)** — the first survey of the codebase. It records the
  commit it started from, triages the initial backlog, and ships the first unit.
- **Pass P001, P002, …** — each later survey starts from the commit the previous
  pass ended on, looks at what changed since, and adds new units. A pass reports
  its delta: units shipped since the last pass, units added, units dropped.

Passes are the only place where broad surveying happens. Between passes, work is
item by item.

## Files in this folder

| File | Purpose | Who writes it |
| --- | --- | --- |
| `PRINCIPLES.md` | This document | Installed once; edit to adapt the rules to this repo |
| `kaizen.json` | Machine state: paths, baseline commit, last pass, next IDs, verify command | Kaizen skills |
| `backlog.md` | Atomic units, open and closed, plus *Noticed — out of scope* | Kaizen skills and the user |
| `log.md` | Append-only record: one entry per shipped, dropped or blocked unit | Kaizen skills |
| `passes/P000-baseline.md`, `passes/PNNN-<date>.md` | One record per survey pass | Kaizen skills |

<!-- kaizen:begin -->
## Kaizen — incremental improvement

This repository is under Kaizen. The log lives in `__KAIZEN_DIR__/`; the rules are
in `__KAIZEN_DIR__/PRINCIPLES.md` — read them before any improvement work.

- Improvements are split into **atomic units** in `__KAIZEN_DIR__/backlog.md`
  before any are implemented. A list from the user is intake first, never
  implemented as one change.
- Units are implemented **one at a time**: implement, verify, commit
  (`kaizen(K-NNN): <title>`), append to `__KAIZEN_DIR__/log.md`, then the next.
  Never combine units in one commit.
- Something noticed mid-unit goes to the backlog, not into the current change.
  Bugs and new features are recorded under *Noticed — out of scope*, not fixed as
  Kaizen units.
- Kaizen skills (plugin `kaizen`): `kaizen-intake`, `kaizen-step`, `kaizen-pass`,
  `kaizen-status`.
<!-- kaizen:end -->

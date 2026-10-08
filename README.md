# Claude Kaizen

Continuous, incremental improvement for codebases that already work.

Most development adds features or fixes bugs. Kaizen is the third kind of work: making
a mature, working project slightly better — clearer, faster, safer to change — one
small step at a time. This plugin gives Claude Code the structure and the discipline
to do that predictably.

## The discipline

Every improvement is broken into **atomic units** before any of it is implemented, and
units are shipped **one at a time**: implement, verify, commit, log — then the next.

A unit is atomic when it has one purpose (a one-sentence title with no "and"), lands as
exactly one commit, can be reverted on its own, states how it will be verified, and
preserves behaviour unless it says otherwise. A long list from the user is never
implemented as one change; it goes through intake first.

Bugs and new features are out of scope. Anything of that kind noticed along the way is
recorded for the normal process, not fixed inside an improvement.

## Skills

| Skill | What it does |
| --- | --- |
| `kaizen-init` | Creates the Kaizen folder inside the repo's project-management folder, adds a rules block to `CLAUDE.md`, and enables the plugin at project scope |
| `kaizen-baseline` | First pass (P000): surveys the codebase or takes your list, triages an atomic backlog, ships the first unit, records the baseline |
| `kaizen-intake` | Splits a list of ideas into atomic units in the backlog. Never implements |
| `kaizen-step` | Ships exactly one unit: change, verify, commit, log |
| `kaizen-pass` | Follow-up pass: measures the delta since the last pass and adds new units |
| `kaizen-status` | Read-only summary: open units, next item, last pass, setup health |

## What it adds to your repository

```text
CLAUDE.md                      # a <!-- kaizen:begin/end --> block pointing at the rules
.claude/settings.json          # enabledPlugins: kaizen@<marketplace> (project scope)
planning/kaizen/               # or inside your existing project-management folder
├── PRINCIPLES.md              # the rules, copied in so they hold without the plugin
├── kaizen.json                # machine state: baseline, passes, next IDs, verify command
├── backlog.md                 # atomic units, open and closed, plus out-of-scope notes
├── log.md                     # append-only record of every unit shipped, blocked, dropped
└── passes/
    ├── P000-baseline.md
    └── P001-<date>.md
```

Each unit lands as one commit, `kaizen(K-NNN): <title>`, so `git log --grep '^kaizen('`
is the complete history of improvements and any one of them reverts cleanly.

## Typical flow

1. `kaizen-init` — once per repo.
2. `kaizen-baseline` — once; optionally hand it your own list of improvements.
3. `kaizen-step` — repeatedly. "Next improvement", "do K-007", "do the next three".
4. `kaizen-intake` — whenever you have new ideas.
5. `kaizen-pass` — periodically, to survey again from where the last pass ended.

## Installation

```bash
claude plugin marketplace add danielrosehill/Claude-Code-Plugins
claude plugin install kaizen@danielrosehill
```

Then run `kaizen-init` inside a repository; it re-installs the plugin at project scope
so collaborators opening the repo get it too.

## License

MIT

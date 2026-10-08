---
name: kaizen-init
description: Set up Kaizen in a repo — create the improvement log folder, add the CLAUDE.md rules block, and enable the plugin at project scope.
---

# Set up Kaizen in a repository

Installs the scaffolding that makes Kaizen behaviour predictable in this repo: a log
folder inside the project-management folder, the rules file, a marker-delimited
block in the repo's `CLAUDE.md`, and a project-scope install of this plugin.

Idempotent — run it again to repair a partial setup. It never overwrites an existing
`backlog.md`, `log.md`, `kaizen.json` or `passes/`.

## Inputs

- Optional: a project-management folder the user wants used.
- Optional: the project's verification command (tests, lint, build check).

## Steps

1. **Find the repository root**: `git rev-parse --show-toplevel`. If this is not a
   git repo, stop and tell the user — Kaizen depends on one commit per unit.

2. **Check for an existing setup**: look for `kaizen.json` under the repo
   (`git ls-files '*kaizen/kaizen.json'`, then a filesystem search excluding
   `node_modules`, `.git`, `vendor`). If found, that folder is `KAIZEN_DIR`; skip
   to step 5 and only fill in what is missing.

3. **Choose the project-management folder.** Use, in order:
   1. the folder the user named;
   2. an existing one at the root — `planning/`, `project-management/`, `pm/`,
      `.planning/`, `docs/planning/`, `docs/project/`;
   3. otherwise create `planning/`.

   `KAIZEN_DIR` is `<that folder>/kaizen`, relative to the repo root.

4. **Detect the verification command** from the repo's own tooling — `package.json`
   scripts (`test`, `lint`, `typecheck`), `Makefile` targets, `pyproject.toml`
   (pytest/ruff), `Cargo.toml`, `go.mod`, CI workflow files. Confirm it with the
   user in one line. If there is genuinely nothing, record `null`; `kaizen-step`
   then verifies per item.

5. **Write the folder** from this skill's `templates/` directory, creating only
   what is missing:
   - `KAIZEN_DIR/PRINCIPLES.md` ← `templates/PRINCIPLES.md`
   - `KAIZEN_DIR/backlog.md` ← `templates/backlog.md`
   - `KAIZEN_DIR/log.md` ← `templates/log.md`
   - `KAIZEN_DIR/kaizen.json` ← `templates/kaizen.json`, with `__KAIZEN_DIR__`
     replaced by `KAIZEN_DIR`, `__DATE__` by today (ISO), and `verify_command` set
     from step 4
   - `KAIZEN_DIR/passes/` — created by `kaizen-baseline`, not here

6. **Add the CLAUDE.md block.** Read `templates/claude-md-snippet.md`, replace
   `__KAIZEN_DIR__`. In the repo-root `CLAUDE.md` (create it if absent):
   - if a `<!-- kaizen:begin -->` … `<!-- kaizen:end -->` block exists, replace it;
   - otherwise append the block at the end, separated by a blank line.
   Do not touch anything outside the markers.

7. **Enable the plugin at project scope**, so anyone opening the repo in Claude
   Code gets the skills:
   1. `claude plugin list --json` — find the entry whose `id` starts with
      `kaizen@`. The text after `@` is the marketplace name.
   2. If `.claude/settings.json` already has `"enabledPlugins": {"kaizen@<marketplace>": true}`,
      it is done. Otherwise run, from the repo root:
      `claude plugin install kaizen@<marketplace> --scope project`
   3. Read `.claude/settings.json` back and confirm the `enabledPlugins` entry is
      there. If the CLI is unavailable or the install fails, add the entry by
      hand and say so.
   4. If `extraKnownMarketplaces` does not name the marketplace, add it so
      collaborators are prompted to add it. For the public catalogue:
      ```json
      "extraKnownMarketplaces": {
        "danielrosehill": {
          "source": { "source": "github", "repo": "danielrosehill/Claude-Code-Plugins" }
        }
      }
      ```
      For any other marketplace, take its source from
      `claude plugin marketplace list` and ask the user before writing it.

8. **Commit** the setup as one commit: `kaizen: initialise improvement log`,
   containing only `KAIZEN_DIR/`, `CLAUDE.md` and `.claude/settings.json`.

9. **Report** `KAIZEN_DIR`, the verify command, whether the plugin is project-
   enabled, and the next step: `kaizen-baseline` for the first pass.

## Notes

- Writes inside the repo only: `KAIZEN_DIR/`, `CLAUDE.md`, `.claude/settings.json`.
- `PRINCIPLES.md` is copied, not linked, so the rules hold for agents without the
  plugin. The user may edit it to suit the repo; re-running init will not
  overwrite it.
- If the repo has `AGENTS.md` rather than `CLAUDE.md`, add the block there too.

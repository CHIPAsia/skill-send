# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A distributable **AI agent skill** (a `SKILL.md` prompt/instruction file) for integrating the **CHIP Send** payouts API (chip-in.asia). The deliverable is *content* — instructions a coding agent reads — not a runtime library. There is no build, no test suite, no linter, and no runtime code path beyond a 10-line install script.

## Layout

```
SKILL.md                  ← authoritative content. Edit this file.
chip-skill-send/SKILL.md  ← mirror produced by `bin/install.cjs`. Gitignored;
                            regenerated locally by the installer or by
                            `npx github:CHIPAsia/skill-send`. The mirror
                            exists so merchants can inspect what they got,
                            but is not stored in the repo.
bin/install.cjs           ← the installer (CommonJS, no deps).
package.json              ← version + bin entry + repository URL. No production deps.
README.md                 ← merchant-facing install instructions.
LICENSE                   ← MIT.
```

`SKILL.md` (root) is the single source of truth. The mirror at `chip-skill-send/SKILL.md` is intentionally not tracked — it is regenerated whenever a merchant installs the skill, and would silently go stale if committed. If you want to inspect the installed mirror locally, run `node bin/install.cjs`.

## How merchants install it

Distributed directly from GitHub. **No npm publish step.** The install command is one of:

```bash
npx -y github:CHIPAsia/skill-send              # latest
npx -y github:CHIPAsia/skill-send#v0.1.0       # tag-pinned
npx -y github:CHIPAsia/skill-send#<commit-sha> # sha-pinned
```

`npx github:owner/repo` downloads the repo tarball and runs the `bin` script declared in `package.json` (`bin/install.cjs`). That script reads `SKILL.md` from `__dirname/../SKILL.md` and copies it into `./chip-skill-send/SKILL.md` in the merchant's CWD.

## Common commands

There is nothing to build, lint, or test. The day-to-day operations are:

- **Smoke-test the installer locally** (mimics what `npx github:` does):
  ```bash
  node bin/install.cjs
  # → copies ./SKILL.md into ./chip-skill-send/SKILL.md
  ```
- **Cut a new version** (merchants pin to tags via the `#vX.Y.Z` install form):
  ```bash
  # 1. bump "version" in package.json
  # 2. commit
  git commit -am "release: vX.Y.Z"
  git tag -a vX.Y.Z -m "vX.Y.Z"
  git push --follow-tags   # pushes commit + tag together
  ```
  Tagging is the only release mechanism — there is no registry publish.
- **Keep README install examples in sync with the latest tag** (line `npx -y github:CHIPAsia/skill-send#vX.Y.Z` in `README.md`).

## What "done" looks like for a change

- If you touched `SKILL.md`, no other file in the repo needs to change — the mirror is regenerated on install.
- If you touched the install command or repo URL, update `README.md` and `package.json` together (the GitHub casing is `CHIPAsia` — capital A, no hyphen).
- Don't introduce a build step, dependency, or test framework. If a change seems to need one, it almost certainly doesn't.
- Don't add `node_modules`, lockfiles, or CI config — none exist and none are needed for a content-only repo.

## Differences from the Collect skill

`CHIPAsia/skill` (the Collect counterpart) is the structural template. Notable differences for Send:

- The install destination is `chip-skill-send/` (not `chip-skill/`), so a single project can install both skills side-by-side without colliding.
- The installer message is "Added CHIP Send skill to ...", not "Added CHIP Payment Gateway skill to ...".
- The `package.json` `name` is `chip-skill-send` and the `bin` key is `skill-send` (matches the install command).

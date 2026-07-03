# CHIP Send — Skill

AI agent skill for integrating the **CHIP Send** payouts API
([docs](https://docs.chip-in.asia/chip-send/api-reference/introduction)).
A `SKILL.md` prompt/instruction file that a coding agent reads when
the merchant installs the skill into their project.

## Install

```bash
# Latest from the default branch
npx -y github:CHIPAsia/skill-send

# Pinned to a specific tag (recommended for production)
npx -y github:CHIPAsia/skill-send#v0.1.0

# Pinned to a specific commit
npx -y github:CHIPAsia/skill-send#<commit-sha>
```

This runs `bin/install.cjs` and copies `SKILL.md` into a local
`chip-skill-send/` folder in the merchant's project.

## Layout

```
SKILL.md             ← authoritative content. Edit this file.
chip-skill-send/SKILL.md  ← mirror produced by `bin/install.cjs`. Gitignored;
                            regenerated locally by the installer or by
                            `npx github:CHIPAsia/skill-send`.
bin/install.cjs      ← the installer (CommonJS, no deps).
package.json         ← version + bin entry + repository URL. No production deps.
README.md            ← this file.
LICENSE              ← MIT.
```

## Related

- [`CHIPAsia/skill`](https://github.com/CHIPAsia/skill) — CHIP Collect skill.
- [CHIP Send API docs](https://docs.chip-in.asia/chip-send/api-reference/introduction)
- [CHIP Send OpenAPI spec](https://docs.chip-in.asia/openapi/chip-send.yaml)

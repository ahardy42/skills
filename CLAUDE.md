# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is a public repository of Claude Code / Agent Skills. The owner publishes it on their personal GitHub for other developers to install and use. Write every file for an outside reader who has no access to the owner's other projects.

## Layout

Each skill is one directory under `skills/`:

```
skills/<skill-name>/
  SKILL.md        # required: YAML frontmatter + instructions
  scripts/        # optional: helper scripts the skill tells Claude to run
  references/     # optional: long docs that SKILL.md loads on demand
  assets/         # optional: templates or static files the skill copies or reads
```

`SKILL.md` frontmatter needs at least:

```yaml
---
name: skill-name          # must match the directory name; lowercase, hyphens
description: ...          # what the skill does AND when to trigger it
---
```

Claude decides whether to load a skill from `description` alone. Put the trigger phrases and use cases there, not only in the body.

Keep `SKILL.md` short. Move long reference material into `references/` and tell Claude in `SKILL.md` when to read each file.

## Rules for published skills

- No personal data, internal company names, private URLs, API keys, or paths under the owner's home directory. Use placeholders.
- No dependency on the owner's local MCP servers or plugins unless the skill's README states it as a requirement.
- Scripts in `scripts/` must run on a clean machine. State runtime requirements (Python version, `npm` packages) in the skill.
- When you add or rename a skill, update two files:
  - `.claude-plugin/marketplace.json`: one plugin entry per skill, with `"source": "./"`, `"strict": false`, and `"skills": ["./skills/<name>"]`. The entry `name` is the skill name. Do not add a `version`. Without one, Claude Code uses the commit SHA, so every push to `main` is an update.
  - `README.md`: one row in the Skills table, with the install name `<name>@ahardy42-skills`.

## Distribution

Users install the skills in two ways. Both read `skills/<name>/SKILL.md`:

- Claude Code plugin marketplace: `.claude-plugin/marketplace.json` makes this repo the `ahardy42-skills` marketplace. Each skill is its own plugin, invoked as `/<name>:<name>`.
- `npx skills add ahardy42/skills` (Vercel `skills` CLI) for Claude Code and other agents. It needs no file in this repo.

## Creating and testing skills

Use the `skill-creator` skill to scaffold a new skill, write evals, and tune the `description` for triggering.

Test from the repo root:

```sh
claude plugin validate .
claude plugin marketplace add ./
claude plugin install <name>@ahardy42-skills
claude plugin details <name>
```

With a local marketplace, Claude Code reads the files in place. Edits load at the next session or after `/reload-plugins`. Remove the local marketplace after the test with `claude plugin marketplace remove ahardy42-skills`.

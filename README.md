# skills

Agent Skills for [Claude Code](https://claude.com/claude-code). Each skill is one directory under `skills/` with a `SKILL.md` file. Claude reads the `description` in each `SKILL.md` and loads the skill when your request matches it.

## Skills

| Skill | What it does |
|---|---|
| [tufte-design](skills/tufte-design/SKILL.md) | Applies Edward Tufte's information design rules when Claude makes or reviews charts, tables, dashboards, and sparklines. Includes recipes for matplotlib/seaborn/plotly, Recharts/D3/SVG, and tables. |

## Install

Claude Code loads skills from two places:

- `~/.claude/skills/` — personal skills, available in every project.
- `<project>/.claude/skills/` — project skills, available only in that project. Commit them to share with your team.

### Install all skills

Clone the repository, then link each skill into your personal skills directory:

```sh
git clone https://github.com/ahardy42/skills.git ~/src/ahardy42-skills
mkdir -p ~/.claude/skills
for dir in ~/src/ahardy42-skills/skills/*/; do
  ln -s "$dir" ~/.claude/skills/
done
```

With symlinks, `git pull` in the clone updates the installed skills.

### Install one skill

Copy the skill directory. Replace `<skill-name>` with a name from the table above.

```sh
git clone https://github.com/ahardy42/skills.git /tmp/ahardy42-skills
mkdir -p ~/.claude/skills
cp -R /tmp/ahardy42-skills/skills/<skill-name> ~/.claude/skills/
```

To install it for one project only, copy it to `<project>/.claude/skills/` instead.

### Check the install

Start a new Claude Code session. Ask Claude "What skills do you have?", or type `/<skill-name>`.

## Update

- Symlink install: run `git pull` in the clone.
- Copy install: copy the skill directory again.

## Uninstall

Remove the skill directory or symlink:

```sh
rm -r ~/.claude/skills/<skill-name>
```

## Requirements

Some skills run scripts from their `scripts/` directory. Each skill's `SKILL.md` lists the tools it needs, for example a Python version or an `npm` package.

## License

[MIT](LICENSE)

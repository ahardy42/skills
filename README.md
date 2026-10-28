# skills

Agent Skills for [Claude Code](https://claude.com/claude-code) and other agents that read the [Agent Skills](https://agentskills.io) format. Each skill is one directory under `skills/` with a `SKILL.md` file. The agent reads the `description` in each `SKILL.md` and loads the skill when your request matches it.

## Skills

| Skill | Install name | What it does |
|---|---|---|
| [tufte-design](skills/tufte-design/SKILL.md) | `tufte-design@ahardy42-skills` | Applies Edward Tufte's information design rules when the agent makes or reviews charts, tables, dashboards, and sparklines. Includes recipes for matplotlib/seaborn/plotly, Recharts/D3/SVG, and tables. |

## Install in Claude Code (plugin marketplace)

This repository is a Claude Code plugin marketplace. Each skill is a separate plugin, so you install only the skills you want.

In a Claude Code session, add the marketplace once, then install a skill:

```
/plugin marketplace add ahardy42/skills
/plugin install tufte-design@ahardy42-skills
```

The same commands from a shell:

```sh
claude plugin marketplace add ahardy42/skills
claude plugin install tufte-design@ahardy42-skills
```

Plugin skills have the plugin name as a prefix. To run a skill by name, type `/tufte-design:tufte-design`. Claude also loads the skill when your request matches its description.

### Update

```sh
claude plugin marketplace update ahardy42-skills
claude plugin update tufte-design@ahardy42-skills
```

Auto-update is off by default for marketplaces that Anthropic does not run. To turn it on, open `/plugin`, go to **Marketplaces**, and select `ahardy42-skills`.

### Uninstall

```sh
claude plugin uninstall tufte-design@ahardy42-skills
```

To remove the marketplace and all of its plugins:

```sh
claude plugin marketplace remove ahardy42-skills
```

## Install in other agents (`npx skills`)

The [`skills` CLI](https://github.com/vercel-labs/skills) installs skills into Claude Code, Cursor, Codex, Gemini CLI, OpenCode, GitHub Copilot, and other agents. It needs Node.js 22.20 or later.

Pick the skills and agents from a list:

```sh
npx skills add ahardy42/skills
```

Install one skill for one agent, for all projects, with no prompts:

```sh
npx skills add ahardy42/skills --skill tufte-design -g -a claude-code -y
```

Without `-g`, the skill goes into the current project.

Update and remove:

```sh
npx skills update
npx skills remove tufte-design
```

## Requirements

Some skills run scripts from their `scripts/` directory. Each skill's `SKILL.md` lists the tools it needs, for example a Python version or an `npm` package.

## License

[MIT](LICENSE)

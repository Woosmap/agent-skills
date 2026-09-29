# Woosmap Agent Skills

Practical guidance for AI coding agents (Claude Code, Cursor, GitHub Copilot, and others) building with [Woosmap](https://www.woosmap.com), or moving an existing project to it.

A skill is on-demand expertise written in Markdown. The agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing: it picks the right Woosmap class, steers around the known traps, and checks the live docs before writing code.

## Install

```bash
npx skills add Woosmap/agent-skills
```

Common options:

```bash
# List the skills in this repo without installing them
npx skills add Woosmap/agent-skills --list

# Install one skill for specific agents
npx skills add Woosmap/agent-skills --skill woosmap-google-maps-migration -a claude-code -a cursor -a github-copilot

# Install for your user instead of the current project
npx skills add Woosmap/agent-skills --global
```

Update installed skills with `npx skills update`.

## Skills

| Skill | Use it when |
| --- | --- |
| [`woosmap-google-maps-migration`](skills/woosmap-google-maps-migration/SKILL.md) | Migrating code from the Google Maps JavaScript API or Google Maps web services to Woosmap |

## How the skills stay current

The skills are thin. They point agents to the live [Woosmap developer docs](https://developers.woosmap.com) (`llms.txt`, `llms-rules.txt` and the `.md` version of each page) and do not copy them. Only guidance that the docs do not cover is written into a skill.

## Contributing

To be discussed.

## License

[MIT](LICENSE)

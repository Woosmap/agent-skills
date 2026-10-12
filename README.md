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
| [`woosmap-store-locator`](skills/woosmap-store-locator/SKILL.md) | Importing your stores into Woosmap and building a page that shows them and finds the nearest ones |

## Everything else: point your agent at the docs

For other Woosmap work, such as address autocomplete, distances or routing, no skill is needed. Add this to your `CLAUDE.md`, `AGENTS.md` or Cursor rules:

> Before writing Woosmap code, fetch https://developers.woosmap.com/llms-rules.txt and follow it.
> Find pages in https://developers.woosmap.com/llms.txt and read the `.md` version of each page.

## How the skills stay current

The skills are thin. They point agents to the live [Woosmap developer docs](https://developers.woosmap.com) and do not copy them. Only guidance that the docs do not cover is written into a skill.

## License

[MIT](LICENSE)

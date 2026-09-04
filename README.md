# Pablo's skill repo

A personal library of Claude Code (and compatible-agent) skills, installable
anywhere with one command via [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add pabloesquivel-c/pablos-skill-repo
```

Options:

| Command | What it does |
| --- | --- |
| `npx skills add pabloesquivel-c/pablos-skill-repo` | Install all skills in this repo |
| `npx skills add pabloesquivel-c/pablos-skill-repo --skill <name>` | Install just one skill |
| `npx skills add pabloesquivel-c/pablos-skill-repo -g` | Install globally (all projects) |
| `npx skills add pabloesquivel-c/pablos-skill-repo -a claude-code` | Install to a specific agent |

## Skills

| Skill | What it does |
| --- | --- |
| [`spec-audit`](spec-audit/SKILL.md) | Audits a product spec before design starts — grades gaps p0/p1/p2, flags bloat, asks instead of inventing, returns an improved draft or a lean rewrite |
| [`spec-prototype`](spec-prototype/SKILL.md) | Turns an approved spec into one medium-fidelity interactive HTML prototype that proves the mechanics and states, with a toolbar that force-sets every state and a panel marking every guess the spec didn't decide |
| [`visual-explainer`](visual-explainer/SKILL.md) | Builds a self-contained, visual HTML page that teaches any topic in depth |

## Structure

One folder per skill at the repo root, each containing a `SKILL.md`:

```markdown
---
name: your-skill-name
description: What this skill does and when to use it
---

# Your Skill

Instructions the agent follows when this skill is active...
```

See [How to Share a Skill Library with `npx skills add`](https://github.com/vercel-labs/skills)
for the full mechanics.

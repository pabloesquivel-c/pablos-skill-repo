# pablo esquivel's skills

a personal library of claude code (and compatible-agent) skills, installable
anywhere with one command via [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add pabloesquivel-c/pablos-skill-repo
```

options:

| command | what it does |
| --- | --- |
| `npx skills add pabloesquivel-c/pablos-skill-repo` | Install all skills in this repo |
| `npx skills add pabloesquivel-c/pablos-skill-repo --skill <name>` | Install just one skill |
| `npx skills add pabloesquivel-c/pablos-skill-repo -g` | Install globally (all projects) |
| `npx skills add pabloesquivel-c/pablos-skill-repo -a claude-code` | Install to a specific agent |

## skills

| skill | what it does |
| --- | --- |
| [`spec-audit`](spec-audit/SKILL.md) | reviews a product spec before design starts and returns the few decisions still open (each with a plain example and a recommended answer) plus a short spec in plain language — asks instead of inventing, and folds team comments back in |
| [`spec-prototype`](spec-prototype/SKILL.md) | turns an approved spec into one medium-fidelity interactive HTML prototype that proves the mechanics and states, with a toolbar that force-sets every state and a panel marking every guess the spec didn't decide |
| [`visual-explainer`](visual-explainer/SKILL.md) | builds a self-contained, visual HTML page that teaches any topic in depth |

## structure

one folder per skill at the repo root, each containing a `SKILL.md`:

```markdown
---
name: your-skill-name
description: What this skill does and when to use it
---

# your skill

instructions the agent follows when this skill is active...
```

See [How to Share a Skill Library with `npx skills add`](https://github.com/vercel-labs/skills)
for the full mechanics.

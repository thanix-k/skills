# Agent Instructions

This repository contains reusable AI agent skills. Each skill is a self-contained directory under `skills/` with a `SKILL.md` file that agents load as procedural knowledge.

## Repository Structure

```
skills/
├── AGENTS.md
├── CLAUDE.md              (symlink → AGENTS.md)
├── README.md
└── skills/
    └── <skill-name>/
        ├── SKILL.md       (required)
        ├── scripts/       (optional — automation helpers)
        └── references/    (optional — supporting docs)
```

## Conventions for AI Agents

- Skill directory names use **kebab-case** (e.g., `tailwindcss-expert`)
- Every skill **must** have a `SKILL.md` — this is the only required file
- Keep `SKILL.md` under **500 lines** to stay context-efficient
- Only load a skill's full content when it is **relevant to the current task**
- Scripts live in `scripts/` with `.sh` extensions and must be executable
- Write specific, actionable descriptions so agents know exactly when to activate a skill

## Adding a New Skill

1. Create a directory: `skills/<your-skill-name>/`
2. Write `SKILL.md` with clear agent instructions
3. Add optional `scripts/` or `references/` as needed
4. Open a PR — the skill will be picked up automatically by the skills.sh registry

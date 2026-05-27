# Skills

This directory contains the skill definitions used by the JustFix Product Brain. Each skill is a self-contained instruction set that tells any agent how to perform a specific product workflow.

---

## Installed skills

| Skill | What it does |
|---|---|
| `lean-prd/` | Writes a lean PRD from a problem statement — problem, solution, top-level user stories |
| `user-stories-jira/` | Breaks a PRD into a storymap and creates Jira tickets under an epic |
| `ticket-refinement/` | Takes a draft Jira story and refines it to be dev-ready |
| `ux-copy/` | Writes and reviews UI copy for customers, fixers, and admin staff |

---

## Skill format

Each skill is a directory containing a `SKILL.md` file. The SKILL.md defines:
- When the skill should be invoked (trigger conditions)
- What inputs it expects
- Step-by-step instructions for the agent
- Output format

---

## Syncing skills from Cowork

Skills in this repo should stay in sync with the skill definitions in Cowork. To export a skill from Cowork and add it here:

1. Locate the skill file in the Cowork plugin system
2. Copy the SKILL.md content into the relevant directory here
3. Commit the change with a note on what version it reflects

The goal is that these skill files are the source of truth — and the Cowork plugin system is one consumer of them, not the only one.

---

## Using skills outside Cowork

Because skills are plain Markdown, they can be used by any Claude interface:

- **Cowork / Claude desktop** — skills are loaded automatically via the plugin system
- **Claude Code** — read the SKILL.md and include it in your session instructions
- **API** — include the SKILL.md content in your system prompt or as a tool definition

This is intentional. The brain should not be locked to any single platform.

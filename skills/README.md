# Skills

This directory contains the skill definitions used by the JustFix Product Brain. Each skill is a self-contained instruction set that tells any agent how to perform a specific product workflow.

---

## Installed skills

| Skill | What it does |
|---|---|
| `lean-prd/` | Writes a lean PRD from a problem statement: problem, solution, top-level user stories. Saves to Notion PRDs database. |
| `user-stories-jira/` | Breaks a PRD (from Notion) into a storymap and creates **Linear** tickets under a parent issue. (Directory name kept for trigger-phrase compatibility. Output is Linear, not Jira.) |
| `ticket-refinement/` | Takes a draft **Linear** story and refines it to be dev-ready. Reads the parent PRD from Notion. |
| `ux-copy/` | Writes and reviews UI copy for customers, fixers, and admin staff |

---

## System of record (May 2026 onwards)

- **PRDs**: Notion **Product Brain** workspace, "PRDs" database (data source `d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5`)
- **Tickets**: Linear, team **Just-fix** (ID `53e25529-6f66-44b7-80cf-6020401d5bf3`)
- **Tech / engineering docs**: Notion **Product Brain**, Technical Docs and Engineering page trees

Confluence and Jira are being retired. Skills should not write new content to either, only read for migration support.

---

## Style rules (apply to all skills)

When writing copy for the user (anything that ends up in Notion, Linear, or chat), follow these rules:

- **No em-dashes (—).** Em-dashes are a strong tell-tale sign of LLM-written prose. Use commas, parentheses, colons, full stops, or rephrase. Replace "X — Y" with "X, Y", "X (Y)", "X: Y", or two sentences.
- **En-dashes (–) are acceptable in numeric ranges** ("5–10 years", "9am–5pm"). Avoid them in prose.
- **Avoid the words "genuinely", "honestly", "straightforward"**. They read as filler or hedging.
- **Don't overuse bold or italics.** Bold a term once when first introduced as a defined concept, but not for emphasis on every other sentence.
- **Write like a person**, not a documentation generator. Short sentences are fine. Conversational asides are fine. Read it aloud once to check.

If the user provides input that contains em-dashes, preserve their wording when quoting them directly but don't introduce new ones in your own copy.

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

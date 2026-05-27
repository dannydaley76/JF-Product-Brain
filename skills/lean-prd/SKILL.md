---
name: lean-prd
description: Use this skill whenever the user wants to write, draft, create, or update a PRD (Product Requirements Document). Trigger on phrases like "write a PRD", "draft a PRD", "product requirements", "help me define this feature", "I need a PRD for", or any request to document a product problem and solution. This skill produces a lean PRD with a problem statement, solution description, and top-level user stories — structured for a storymap-to-Jira workflow.
---

# Lean PRD Skill

## Format

This PRD follows a lean structure: focused, scannable, and just enough to align a team and feed into a storymap.

### Structure

```
# PRD: [Feature / Initiative Name]

## Problem
[1–2 paragraphs describing the problem being solved. Who is affected, what is the pain, and why does it matter now?]

## Solution
[1–2 paragraphs describing the proposed solution at a high level. What are we building, and how does it address the problem?]

## User Stories
[Top-level stories that define the scope. These will be broken down further in the storymap.]
```

---

## Process

### Step 1 — Gather context before drafting

Ask the user for the following if not already provided. Ask all questions at once, not one at a time:

1. **What is the feature or initiative name?**
2. **Who is the primary user or persona?**
3. **What problem are we solving, and for whom?** (Even a rough description is fine — you'll refine it)
4. **What is the rough solution direction?** (Don't need full detail — high-level intent)
5. **Any known constraints?** (Tech, time, team size, dependencies)
6. **Any stories or scenarios already in mind?**

If the user has already provided most of this in their prompt, extract what you can and only ask for the gaps.

---

### Step 2 — Draft the PRD

Follow the format above. Guidelines for each section:

#### Problem (1–2 paragraphs)
- Open with the user and their pain point
- Include why this matters (business or user impact)
- Be specific — avoid vague language like "users struggle with X"
- Do NOT reference the solution here

#### Solution (1–2 paragraphs)
- Describe what is being built at a conceptual level
- Explain how it addresses the problem
- Mention key interactions or flows if helpful
- Avoid going into implementation detail

#### User Stories
- Write 4–8 top-level stories in the format: `As a [user], I want [goal] so that [benefit]`
- These should represent the major capabilities — not sub-tasks
- They should map naturally to storymap swimlanes or epics in Jira
- Label each with a simple ID: US-01, US-02, etc.
- Flag any story that has a notable dependency with a 🔗 note

---

### Step 3 — Flag gaps

After the PRD, add a brief **Open Questions** section if any of the following are unclear:
- Success metrics or how outcomes will be measured
- Out-of-scope items that could cause confusion
- Unknowns that need a decision before building can begin

Keep this section short — bullet points only. Remove it entirely if there are no meaningful gaps.

---

## Output example structure

```markdown
# PRD: [Name]

## Problem
[Paragraph 1]
[Paragraph 2 if needed]

## Solution
[Paragraph 1]
[Paragraph 2 if needed]

## User Stories
- **US-01** As a [user], I want [goal] so that [benefit]
- **US-02** As a [user], I want [goal] so that [benefit]
- **US-03** As a [user], I want [goal] so that [benefit] 🔗 *Depends on US-01*
...

## Open Questions
- How will we measure success?
- Is [edge case] in or out of scope?
```

---

## Notes

- Keep the PRD to a single page where possible
- These user stories are intentionally top-level — they feed into a storymap, which then generates Jira tickets under an epic
- Avoid acceptance criteria at this stage — that detail belongs in the storymap / Jira tickets
- If the user asks to export or convert to Jira, refer to the user-stories skill for that next step

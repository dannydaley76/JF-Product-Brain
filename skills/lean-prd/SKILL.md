---
name: lean-prd
description: Use this skill whenever the user wants to write, draft, create, or update a PRD (Product Requirements Document). Trigger on phrases like "write a PRD", "draft a PRD", "product requirements", "help me define this feature", "I need a PRD for", or any request to document a product problem and solution. This skill produces a lean PRD with a problem statement, solution description, and top-level user stories — and saves it as a row in the JustFix Notion PRDs database, ready to feed into the user-stories-jira skill (which now outputs Linear tickets).
---

# Lean PRD Skill

## Where PRDs live

JustFix PRDs are stored as rows in the **PRDs database** inside the **Product Brain** Notion workspace. Each row is one feature; the PRD body lives in the page content; structured fields (Status, Quarter, Linked Linear Epic, Owner) live as database properties.

- **Notion workspace home**: Product Brain
- **PRDs database data source ID**: `d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5`
- **Database fields**:
  - `Name` (TITLE) — feature/initiative name, no "PRD:" prefix (the database makes the type implicit)
  - `Status` (SELECT) — Draft / Review / Approved / In Build / Shipped / Archived
  - `Owner` (PEOPLE) — Notion user; leave blank if no Notion user ID is known and let the user assign themselves
  - `Quarter` (SELECT) — 2026 Q2 / 2026 Q3 / 2026 Q4 / 2027 Q1
  - `Linked Linear Epic` (URL) — Linear epic/parent issue URL. May be blank if the epic doesn't exist yet
  - `Last Edited` (LAST_EDITED_TIME) — auto-managed

When the skill saves a PRD, default `Status` to `Draft` and `Quarter` to the current quarter unless the user specifies otherwise.

---

## Format

This PRD follows a lean structure: focused, scannable, and just enough to align a team and feed into a storymap.

### Page body structure

```
## Problem
[1–2 paragraphs describing the problem being solved. Who is affected, what is the pain, and why does it matter now?]

## Solution
[1–2 paragraphs describing the proposed solution at a high level. What are we building, and how does it address the problem?]

## User Stories
[Top-level stories that define the scope. These will be broken down further into Linear tickets via the user-stories-jira skill.]

## Open Questions
[Only include if there are real unknowns to flag.]
```

Note: the Notion database title (`Name` property) supplies the page heading, so do NOT repeat the feature name at the top of the body.

---

## Process

### Step 1 — Gather context before drafting

Ask the user for the following if not already provided. Ask all questions at once, not one at a time:

1. **What is the feature or initiative name?**
2. **Who is the primary user or persona?** (customer, fixer, ops/admin)
3. **What problem are we solving, and for whom?** (Even a rough description is fine — you'll refine it)
4. **What is the rough solution direction?** (Don't need full detail — high-level intent)
5. **Any known constraints?** (Tech, time, team size, dependencies)
6. **Any stories or scenarios already in mind?**
7. **Target quarter?** (If unspecified, default to the current quarter)
8. **Existing Linear epic, if any?** (URL — can be added later)

If the user has already provided most of this in their prompt, extract what you can and only ask for the gaps.

---

### Step 2 — Draft the PRD body

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
- They should map naturally to storymap activities and Linear parent issues
- Label each with a simple ID: US-01, US-02, etc.
- Flag any story that has a notable dependency with a 🔗 note

#### Open Questions

Add a brief **Open Questions** section if any of the following are unclear:

- Success metrics or how outcomes will be measured
- Out-of-scope items that could cause confusion
- Unknowns that need a decision before building can begin

Keep this section short — bullet points only. Remove the section entirely if there are no meaningful gaps.

---

### Step 3 — Show the draft for review

Before writing to Notion, show the user the full draft in chat (properties + body) and ask whether to save. Show it in this format:

```
**Save to Notion PRDs database with:**

- Name: [feature name]
- Status: Draft
- Quarter: [chosen quarter]
- Linked Linear Epic: [URL or "(none yet)"]

**Body:**

[full markdown body — Problem, Solution, User Stories, Open Questions]
```

Then ask: "Save this as a new PRD in Notion, or want to tweak first?"

---

### Step 4 — Save to Notion

When the user confirms, create the page using `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-create-pages` with:

```json
{
  "parent": {
    "type": "data_source_id",
    "data_source_id": "d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5"
  },
  "pages": [{
    "properties": {
      "Name": "[feature name]",
      "Status": "Draft",
      "Quarter": "[2026 Q2 etc.]",
      "Linked Linear Epic": "[URL or omit]"
    },
    "icon": "[a fitting emoji]",
    "content": "[the full markdown body — Problem, Solution, User Stories, Open Questions]"
  }]
}
```

After saving, return the Notion page URL to the user so they can open it.

If updating an existing PRD instead of creating a new one, use `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-update-page` — fetch the PRD first with `notion-fetch` to get its current properties and body before editing.

---

## Notes

- Keep the PRD to a single page where possible
- Top-level user stories are intentionally lean — they feed into the `user-stories-jira` skill which now generates **Linear** tickets (Just-fix team), not Jira
- Avoid acceptance criteria at this stage — that detail belongs in the storymap / Linear tickets
- The `Linked Linear Epic` field is the canonical pointer to the engineering work. While JustFix is mid-migration from Jira, a Jira epic URL is acceptable as a temporary value — but flag to the user that it should be updated to a Linear URL once the epic is migrated
- If the user asks to break the PRD down into tickets next, hand off to the `user-stories-jira` skill — it can read this PRD back from Notion via `notion-fetch`

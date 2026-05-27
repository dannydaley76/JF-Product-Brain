---
name: user-stories-jira
description: Use this skill when the user wants to write user stories, create a storymap, break down a PRD into stories, or export stories to Jira. Trigger on phrases like "write user stories", "break this down into stories", "create a storymap", "turn this into Jira tickets", "create an epic", "acceptance criteria", or any request to take a feature or PRD and produce structured development work. This skill follows a lean PRD workflow: top-level stories → storymap → Jira tickets under an epic.
---

# User Stories & Jira Export Skill

## Overview

This skill takes input from a lean PRD (or a described feature) and produces:
1. A **storymap** — stories organised by user activity and broken into detail-level tasks
2. **Jira-ready tickets** — an epic + child stories with summaries, descriptions, and acceptance criteria

---

## Process

### Step 1 — Confirm input

Check what the user has provided:
- If they have a completed PRD with user stories → go straight to Step 2
- If they have a rough feature description → quickly extract the top-level stories first, confirm with the user, then proceed
- If they want to refine existing stories → ask what's missing or not working

---

### Step 2 — Build the storymap

A storymap has two layers:
- **Activities (rows)** — the high-level things a user does (maps to top-level PRD stories)
- **Tasks (columns)** — the specific actions within each activity (the stories that become Jira tickets)

#### Storymap format

```
Activity: [US-01 story summary]
  → Task 1: [detail story]
  → Task 2: [detail story]
  → Task 3: [detail story]

Activity: [US-02 story summary]
  → Task 1: [detail story]
  ...
```

Guidelines:
- Each activity maps to one top-level user story from the PRD
- Break each activity into 2–5 tasks — these become individual Jira stories
- If a task is large or complex, flag it as a candidate to split further
- Mark MVP tasks with ⭐ — ask the user if they want MVP scope defined

---

### Step 3 — Generate Jira tickets

For each task in the storymap, produce a Jira-ready ticket in the following format:

```
---
**Epic:** [Epic name — matches the PRD feature name]
**Story:** [US-XX.X] As a [user], I want [goal] so that [benefit]
**Description:** [1–2 sentences expanding on the story if needed]
**Acceptance Criteria:**
- [ ] Given [context], when [action], then [outcome]
- [ ] Given [context], when [action], then [outcome]
- [ ] [Edge case or error state if relevant]
**Labels:** [Suggested labels e.g. MVP, frontend, backend, blocked]
**Dependencies:** [Ticket ID or description if applicable]
---
```

Guidelines for acceptance criteria:
- Use Given/When/Then (Gherkin-style) for clarity
- Aim for 2–4 criteria per story — enough to verify done, not a full spec
- Always include at least one error or edge case where relevant
- Criteria should be testable — avoid vague statements like "works correctly"

---

### Step 4 — Epic summary ticket

Always produce a top-level Epic ticket first:

```
---
**Type:** Epic
**Name:** [Feature / Initiative Name]
**Description:** [2–3 sentences summarising what this epic delivers and why]
**Stories in scope:** [List of story IDs: US-01.1, US-01.2, ...]
---
```

---

## Story ID convention

Use a two-level ID system:
- `US-01` = top-level activity (from PRD)
- `US-01.1`, `US-01.2` = child tasks (Jira stories)

---

## Output order

Always output in this order:
1. Epic ticket
2. Storymap (visual summary)
3. Individual story tickets grouped by activity

---

## Notes

- Acceptance criteria belong here, not in the PRD — the PRD stays lean
- If the user has Jira connected, offer to create the tickets directly
- If the user wants to adjust MVP scope, re-run the storymap with ⭐ markers and ask for confirmation before generating tickets
- Flag any story where the scope is ambiguous with a ❓ and suggest a clarifying question

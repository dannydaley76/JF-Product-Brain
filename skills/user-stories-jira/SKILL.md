---
name: user-stories-jira
description: Use this skill when the user wants to write user stories, create a storymap, break down a PRD into stories, or export stories to Linear or Jira. Trigger on phrases like "write user stories", "break this down into stories", "create a storymap", "turn this into tickets", "create an epic", "acceptance criteria", or any request to take a feature or PRD and produce structured development work. This skill now outputs to Linear (Just-fix team) by default — JustFix has migrated off Jira — and reads the parent PRD from the Notion PRDs database.
---

# User Stories & Linear Export Skill

## Overview

This skill takes input from a lean PRD (a row in the Notion PRDs database) — or a described feature — and produces:

1. A **storymap** — stories organised by user activity and broken into detail-level tasks (chat-only output for review; not persisted unless the user asks)
2. **Linear-ready tickets** — a parent issue (acts as the epic) + child issues under the **Just-fix** team

> **Migration note**: JustFix used Jira until May 2026; this skill now writes to Linear. If a user explicitly asks for Jira output (e.g. for a legacy ticket), the structure below still applies — just use the Jira MCP instead. The skill directory keeps "jira" in its name for backward compatibility with existing trigger phrases.

---

## Where things live

- **PRDs**: Notion PRDs database, data source `d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5`
- **Tickets**: Linear team **Just-fix**, ID `53e25529-6f66-44b7-80cf-6020401d5bf3`
- **Linear MCP create/update tool**: `mcp__a060d5e1-5a65-485c-b7a5-bdd9e5db93f1__save_issue`
- **Notion read tool**: `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-fetch`

---

## Process

### Step 1 — Confirm input

Check what the user has provided:

- **Notion PRD page URL or row name**: fetch it via `notion-fetch` (pass the URL or page ID) to read the Problem, Solution, and User Stories from the page body.
- **A described feature with no PRD yet**: extract the top-level stories from the description, confirm with the user, then proceed. Offer to also save a PRD via the `lean-prd` skill if the feature is non-trivial.
- **Existing tickets to refine**: this is a different workflow — use the `ticket-refinement` skill instead.

If a PRD is referenced but not yet in Notion, prompt the user to save it via the `lean-prd` skill first so there's a canonical link.

---

### Step 2 — Build the storymap (chat-only by default)

A storymap has two layers:

- **Activities** — the high-level things a user does (maps to top-level PRD stories US-01, US-02, ...)
- **Tasks** — the specific actions within each activity (the stories that become Linear child issues)

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
- Break each activity into 2–5 tasks — these become individual Linear child issues
- If a task is large or complex, flag it as a candidate to split further
- Mark MVP tasks with ⭐ — ask the user if they want MVP scope defined
- Flag any story where scope is ambiguous with ❓

#### Persisting the storymap

By default, the storymap stays in chat for review. JustFix deferred a dedicated storymaps database when migrating from Confluence to Notion.

If the user wants to keep the storymap, save it as a **sub-page of the PRD row** in Notion. Use `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-create-pages` with `parent.type = "page_id"` and the PRD row's page ID. Title: `Storymap`. Icon: `🗺️`.

---

### Step 3 — Generate the Linear ticket structure (chat preview first)

For each task in the storymap, draft a Linear-ready ticket in the following format. Show all tickets in chat first for review before any writes.

```
---
**Parent issue:** [Feature / Initiative Name]  (acts as the epic)
**Story:** [US-XX.X] As a [user], I want [goal] so that [benefit]
**Description:** [1–2 sentences expanding on the story if needed]
**Acceptance Criteria:**
- [ ] Given [context], when [action], then [outcome]
- [ ] Given [context], when [action], then [outcome]
- [ ] [Edge case or error state if relevant]
**Labels:** [Suggested labels e.g. MVP, frontend, backend, blocked]
**Priority:** [Urgent / High / Medium / Low / None]
**Dependencies:** [Issue ID/identifier if applicable — e.g. JUS-1353]
---
```

Guidelines for acceptance criteria:

- Use Given/When/Then (Gherkin-style) for clarity
- Aim for 2–4 criteria per story — enough to verify done, not a full spec
- Always include at least one error or edge case where relevant
- Criteria should be testable — avoid vague statements like "works correctly"

---

### Step 4 — Parent issue (the epic)

Always draft a top-level parent issue first. In Linear, parent issues fulfil what Jira calls an "epic":

```
---
**Type:** Parent issue (epic)
**Title:** [Feature / Initiative Name]
**Description:** [2–3 sentences summarising what this epic delivers and why. Reference the Notion PRD URL.]
**Stories in scope:** [List of story IDs: US-01.1, US-01.2, ...]
---
```

When created in Linear, all child tickets will set this issue's identifier as their `parentId`.

---

### Step 5 — Write to Linear (after user confirmation)

Once the user signs off on the preview, create the issues in Linear:

1. **Create the parent issue first** using `save_issue` with `team: "53e25529-6f66-44b7-80cf-6020401d5bf3"`, the title, description, priority, and labels. Capture the returned identifier (e.g. `JUS-1400`).
2. **Create each child issue** using `save_issue` with the same `team` and `parentId: "JUS-1400"` (the parent's identifier from step 1). Include description, AC (in Linear description as a checklist), labels, priority, and any `blockedBy` relations.
3. **Update the PRD row's `Linked Linear Epic`** field with the URL of the parent issue (use `notion-update-page` with `command: "update_properties"` on the PRD's page ID).

If any save fails, stop and report — do not partially commit then continue. The user may want to manually clean up before retrying.

Linear `priority` mapping: `0=None, 1=Urgent, 2=High, 3=Medium, 4=Low`.

---

## Story ID convention

Use a two-level ID system:

- `US-01` = top-level activity (from the PRD)
- `US-01.1`, `US-01.2` = child tasks (Linear child issues)

Use the US-XX.X prefix in the **title** of each Linear issue (e.g. `US-01.1 — Send WhatsApp at appointment end time`) so they're scannable in the Linear list view.

---

## Output order in chat

Always show in this order before any writes:

1. Parent issue (epic) preview
2. Storymap (visual summary)
3. Individual child issue previews, grouped by activity

Then ask: "Save all of these to Linear (Just-fix), or want to adjust first?"

---

## Notes

- Acceptance criteria belong here, not in the PRD — the PRD stays lean
- If the user wants to adjust MVP scope, re-run the storymap with ⭐ markers and ask for confirmation before writing tickets
- Flag any story where scope is ambiguous with ❓ and suggest a clarifying question
- After tickets are created, suggest using the `ticket-refinement` skill on each one before development picks them up — refinement still produces the highest-value detail per ticket
- If the PRD's `Linked Linear Epic` field already has a value, ask the user whether to overwrite it or add child tickets to the existing epic

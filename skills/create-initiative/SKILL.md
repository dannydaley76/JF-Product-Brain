---
name: create-initiative
description: Create a new Linear initiative for JustFix — from idea to fully structured initiative with goal-based naming, a description, linked existing projects, and suggested new projects. Use this skill whenever the user wants to create a new initiative, says "new initiative", "I want to add an initiative", "let's create an initiative for X", or has a strategic idea they want to formalise in Linear. Also trigger when the user has a Discovery-stage idea that has been validated and is ready to be structured as an initiative. This skill guides the user through goal-based framing, creates the initiative in Linear, links relevant existing projects, and updates the product brain.
---

# Create Initiative

Turn a strategic idea into a properly structured Linear initiative — with a goal-based name, a clear description, and the right projects underneath it.

## Why this matters

An initiative should answer "what outcome are we working toward?" not "what are we building?". The name and description need to be goal-based, not solution-based — so that projects can change and the initiative still makes sense. A well-structured initiative also makes it obvious which existing work already belongs under it and what new projects might be needed.

---

## Step 1: Is this an initiative or a project?

Before doing anything, quickly sense-check whether the idea belongs at initiative level. An initiative is a **strategic goal** that could be delivered by multiple projects over time. A project is a **specific piece of work** with a defined scope and end state.

Ask the user: *"Before we set this up, I want to make sure initiative is the right level — is this a strategic goal that multiple projects could contribute to, or is it a specific thing you want to build?"*

If it's a project, say so clearly and offer to create it as a project linked to an existing initiative instead. Stop here if so.

---

## Step 2: Intake questions

Ask these questions to understand the initiative. Don't ask them all at once — have a brief conversation. Two or three turns is fine.

1. **What outcome are you trying to achieve?** (Not what you're building — what changes for customers, fixers, or the business?)
2. **Who benefits and how?** (customers, fixers, ops team, the business?)
3. **Why now?** (what's changed, what's the opportunity or pressure?)
4. **What does success look like — is there a metric we could track?** (e.g. ops time saved, conversion rate, repeat bookings, fixer retention. Even a rough proxy is useful. This gets recorded so we can compare pre/post launch.)
5. **Any early thoughts on what projects might sit under this?** (don't worry if unsure)

The goal is to understand the strategic intent well enough to write a good description and name. Don't proceed until you have a clear sense of the outcome and at least one success metric.

---

## Step 3: Propose goal-based names

Generate 3–4 name options. Names must be:
- **Outcome-focused** — describe what success looks like, not what you're building
- **Memorable** — short enough to say in a sentence
- **Not solution-specific** — so the initiative survives if the approach changes

Good examples: "Zero-touch fixer payments", "Always send the best fixer", "Book a job from a photo or video"
Bad examples: "Implement self-billing", "Matching algorithm improvements", "Live video feature"

Present the options and ask the user to pick one or suggest their own. Don't proceed until the name is confirmed.

---

## Step 4: Draft the initiative description

Write a description with four sections:

1. **The problem or opportunity** — what's happening today that makes this worth doing?
2. **The goal** — what does success look like? (one clear sentence)
3. **Why it matters** — what does this unlock for the business, customers, or fixers?
4. **Success metric** — the specific metric (or proxy) that will tell us if this initiative worked. Format: `**Success metric:** [metric] — [current baseline if known] → [target]`

Example: `**Success metric:** % of reschedules handled without ops involvement — currently 0% → target 80%`

If the user doesn't know the current baseline, record the metric and note "baseline TBC". Keep it concise — this is a strategic framing document, not a spec. Show the draft to the user and refine if needed.

---

## Step 5: Find existing projects to link

Query Linear for all active projects using the Linear MCP:

```
list_projects (no filters, include all active)
```

Review the list and identify projects that clearly belong under this initiative — where the project is a delivery vehicle for the same goal. Be selective: only suggest projects where the connection is clear. Don't force weak fits.

Present your suggestions to the user: *"These existing projects look like they belong under this initiative: [list]. Does that seem right? Anything I've missed or shouldn't include?"*

---

## Step 6: Suggest new projects

Based on the initiative goal and what the user has said, suggest 1–3 new projects that don't exist yet but would logically sit under this initiative. A project is a bounded piece of work with a clear deliverable — not a vague area.

For each suggestion, give a project name and one sentence on what it covers. These are suggestions, not commitments — the user can take, modify, or ignore them.

---

## Step 7: Confirm before creating

Show a summary before touching anything:

```
Initiative: [name]
Description: [draft]
Success metric: [metric] — [baseline] → [target]

Existing projects to link:
- [project 1]
- [project 2]

New projects to create:
- [project name] — [one line description]
```

Ask: *"Happy to go ahead with this?"*

---

## Step 8: Create in Linear

Once confirmed:

1. **Create the initiative** using `save_initiative`:
   - name: confirmed name
   - description: confirmed description
   - status: "Planned"
   - owner: "me"

2. **Link existing projects** using `save_project` with `addInitiatives` for each. Use the initiative ID (not name) to avoid the name-matching bug — fetch the ID from the `save_initiative` response.

3. **Create new projects** using `save_project` for each agreed new project:
   - name: agreed project name
   - description: brief description
   - addTeams: ["Just-fix"]
   - addInitiatives: [initiative ID]

---

## Step 9: Update the product brain

1. **Update `context/initiatives.md`** — add the new initiative in the appropriate thematic group (Customer experience, Fixer supply, Operations & platform, or a new group if it doesn't fit). Use the same format as existing entries: initiative name as heading, one-paragraph description, a Projects line, and a **Success metric** line.

2. **Git push**:
```bash
cd [product brain path]
git add context/initiatives.md
git commit -m "Add [initiative name] initiative"
git push
```

---

## Tips

- If the user struggles to articulate the outcome (keeps describing features instead of goals), ask: *"If this initiative is a success in 12 months, what's different? What can customers/fixers/ops do that they can't do today?"*
- Don't over-link existing projects. A project loosely related to the theme isn't the same as a project delivering against the goal.
- If the user wants to link a project that already belongs to another initiative, that's fine — projects can belong to multiple initiatives.
- The "why now" answer often becomes the third paragraph of the description.

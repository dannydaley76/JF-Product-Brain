---
name: backlog-grooming
description: Run a backlog grooming session — pull tickets from Linear, prioritise them, flag stale items for removal, and apply basic sizing. Use this skill when the user wants to triage their backlog, prepare for an upcoming sprint, clear out dead tickets, find small tickets for a specific developer, or get a prioritised view of outstanding work. Trigger on phrases like "groom the backlog", "triage backlog", "clean up tickets", "what should we work on next", "find small tickets", "what's stale in the backlog", or any request to prioritise or size outstanding work.
---

# Backlog Grooming

Help the team maintain a healthy, actionable backlog by prioritising work, removing stale tickets, and applying rough sizing.

## What this session produces

1. **Prioritised list** — tickets ranked by urgency, business value, and dependencies
2. **Stale ticket flags** — tickets to archive or delete, with a reason
3. **Basic sizing** — S / M / L applied to each ticket

## Workflow reference

The full idea-to-execution workflow for Just-fix is:

| Status | Type | Meaning |
|--------|------|---------|
| Parking Lot | Backlog | Idea captured, RICA scored, not being actively explored |
| Discovery | Backlog | Actively being explored — problem definition, solution thinking |
| Backlog | Backlog | General backlog (pre-PRD work, bugs, improvements) |
| Ready for Refinement | Backlog | Stories written, awaiting refinement session |
| Ready for Execution | Unstarted | Fully refined, A/C agreed, sized — sprint-ready |
| Todo | Unstarted | Committed to current sprint |
| In Progress → Staging Review → Done | Started/Completed | Active dev workflow |

When grooming, the goal is to move tickets forward through this chain — or flag them for removal if they're stale.

## Step 1: Pull the backlog

Use the Linear MCP to fetch all tickets across all pre-sprint statuses (Parking Lot, Discovery, Backlog, Ready for Refinement, Ready for Execution, Todo) for the Just-fix team. Include title, description, labels, status, created date, and last updated date.

If the user has a specific focus (e.g. "find small tickets for Anthony", "just the voice agent work"), filter accordingly — but default to the full backlog unless told otherwise.

## Step 2: Triage for staleness

Flag a ticket as **stale** if any of these are true:
- Last updated more than 60 days ago with no meaningful activity
- References a feature that appears to have already shipped (check description for clues)
- Marked `needs-refinement` or `refinement-required` and hasn't been touched in 30+ days
- Duplicate of another ticket (note which one)
- Vague to the point of being unactionable (no description, no acceptance criteria, no context)

For each stale ticket, suggest one of: **Archive** (worth keeping for reference), **Delete** (genuinely dead), or **Resurrect** (still relevant, needs a refresh).

## Step 3: Size remaining tickets

Apply a simple T-shirt size to each non-stale ticket:

| Size | What it means |
|------|--------------|
| S | Single session (half day or less), no dependencies, one person, self-contained |
| M | 1–3 days, may have one dependency or need a small design decision |
| L | Multi-day, involves multiple people or significant complexity |

Base sizing on the description and any labels (e.g. `spike`, `frontend`, `backend`). If a ticket is too vague to size, flag it as **Needs refinement** rather than guessing.

## Step 4: Prioritise and recommend next status

For each ticket, recommend the appropriate next status move:

- **Parking Lot → Discovery**: RICA scored, problem is worth actively exploring now
- **Discovery → Ready for Refinement**: PRD written in Notion, stories created in Linear
- **Ready for Refinement → Ready for Execution**: Refinement session completed, A/C agreed, sized
- **Ready for Execution → Todo**: Committed to upcoming sprint
- **Any status → Cancelled**: Stale, duplicate, or no longer relevant

Rank non-stale tickets using these factors:

- **RICA score** — if scored, use (Reach × Impact × Confidence) / Effort as primary signal
- **Business alignment** — big jobs, subscriptions, MCP/agentic bookings are the current strategic focus
- **Urgency** — is something blocked on this, or is there a deadline?
- **Unblocks other work** — tickets that unlock downstream work rank higher
- **Effort vs impact** — prefer high-impact S/M tickets over L tickets unless the L is critical

Output three tiers:
- **Do next** — P0, should be in the upcoming sprint
- **Soon** — P1, queue for the sprint after
- **Later / deprioritise** — P2, can wait or needs more thinking

## Step 5: Output the grooming report

Present findings in this structure:

---

## Backlog Grooming — [Date]

### Stale tickets ([N] found)
| Ticket | Age | Recommendation | Reason |
|--------|-----|----------------|--------|

### Sized backlog
| Ticket | Size | Priority | Notes |
|--------|------|----------|-------|

### Do next (P0)
- [ticket id] **[title]** · S/M/L · [one line rationale]

### Soon (P1)
- [ticket id] **[title]** · S/M/L · [one line rationale]

### Later (P2)
- [ticket id] **[title]** · S/M/L · [one line rationale]

### Recommended actions
- [specific things to do in Linear — archive X, assign Y, move Z to sprint]

---

## Step 6: Apply changes (with permission)

After presenting the report, ask: "Want me to apply any of these changes in Linear?"

If yes, use the Linear MCP to:
- Archive or update stale tickets (add a comment explaining why, then set status to Cancelled)
- Add size labels (S, M, L) to tickets — create the labels if they don't exist
- Update priorities on tickets where the user agrees

Always confirm before making bulk changes. Make individual changes without re-confirming if the user has given a clear go-ahead.

## Tips

- If the user says "find small tickets for [name]", focus the Do Next section on S-sized tickets and note which ones are suitable for that person based on their apparent skill area (frontend/backend/etc.)
- Keep the report scannable — one line per ticket where possible
- If you're unsure about a priority call, surface it as a question rather than guessing

---
name: ticket-refinement
description: Use this skill when the user wants to refine a Linear (or legacy Jira) ticket, prepare a story for development, run a refinement session, or get a ticket ready for an engineer to pick up. Trigger on phrases like "refine this ticket", "let's groom this story", "is this ready for dev?", "review this ticket with me", "let's prep this for build", or any request to take a draft ticket and improve it. This skill assumes the ticket is in user-stories-jira format with a parent PRD available in the Notion PRDs database, and produces a proposed update for human review before any write-back to Linear.
---

# Ticket Refinement Skill

## Overview

This skill takes **one ticket at a time** from Linear (Just-fix team), the **parent PRD** from the Notion PRDs database, and optional context from sibling tickets in the same epic. It runs a structured refinement conversation that produces a proposed update for the human to approve.

The bar for "refined" is **human-ready, agent-friendly**: an experienced engineer can pick the ticket up confidently, and an AI planning agent has enough to draft an execution plan (though it may still ask follow-ups).

This skill does NOT write to Linear directly without confirmation. It outputs a side-by-side proposal in the chat. The human approves changes before any write-back happens.

> JustFix migrated off Jira in May 2026. Refinements now target Linear. The skill name keeps "Jira" terminology only in trigger phrases and as the source format reference.

---

## Where things live

- **PRDs**: Notion PRDs database, data source `d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5`. Fetch via `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-fetch`.
- **Tickets**: Linear team **Just-fix**, ID `53e25529-6f66-44b7-80cf-6020401d5bf3`. Fetch via `mcp__a060d5e1-5a65-485c-b7a5-bdd9e5db93f1__get_issue`. Update via `mcp__a060d5e1-5a65-485c-b7a5-bdd9e5db93f1__save_issue` (pass the issue `id` to update an existing issue).
- **Sibling tickets**: list via `mcp__a060d5e1-5a65-485c-b7a5-bdd9e5db93f1__list_issues` filtered by `parent`.

---

## Input expected

- **One Linear ticket identifier** (e.g. `JUS-1353`) or URL
- **The parent PRD**: either pull it from the ticket's parent issue's `Linked Linear Epic` reverse lookup on the Notion PRDs database, or ask the user to paste the Notion PRD URL/page ID
- **Optional context**: other child tickets in the same parent issue. Useful for spotting dependencies and avoiding overlap. Pull via `list_issues` with the parent's identifier.

If the PRD or the ticket isn't provided, ask once before proceeding. Do not invent context.

---

## Process

### Step 1: Fetch context

1. `get_issue` on the target Linear ticket. Capture title, description, labels, priority, parent, blockedBy, current status.
2. `notion-fetch` on the parent PRD. Capture Problem, Solution, User Stories, Open Questions, and `Linked Linear Epic` property.
3. If sibling context will help, `list_issues` filtered by the same `parent` to see neighbouring tickets.

---

### Step 2: Silent prep pass

Read the ticket and the PRD. Privately assess the ticket against these categories. Do NOT show this assessment to the user. It shapes which questions to ask.

- **Scope clarity**: is it clear what's in and what's out of this ticket?
- **User experience**: are flows, states, and error paths described?
- **AC testability and coverage**: is every AC in Given/When/Then? Is there an edge/error case? Could a tester verify each one?
- **Dependencies**: are upstream/downstream tickets, systems, or data declared?
- **Edge cases**: what's the weirdest valid input? What happens on failure?
- **Technical context**: only relevant if criteria in Step 5 are met. Do not pre-fill this for ordinary tickets.

Use the assessment to pick the most useful **3 to 5 anchor questions** for Round 1. A tight ticket may only need 2 to 3 anchors. A vague one may need all 5. Do not ask a question whose answer is already in the ticket or PRD.

---

### Step 3: Round 1 anchor questions (batch)

Ask 3 to 5 questions at once. The five categories:

1. **Scope**: what's in, what's out, what's a separate ticket
2. **User experience**: flows, states, errors, success
3. **Dependencies**: other tickets, systems, data, teams
4. **Edge cases**: weird inputs, scale, interruptions, user types
5. **Validation**: how we'd verify done, test approach, evidence

Format as a numbered list so answers can come back numbered. See the anchor question library at the bottom of this file.

---

### Step 4: Round 2 conversational drill-down

For each Round 1 answer that exposed something interesting, follow up **one question at a time**. Pull on the thread until it's resolved, then move to the next.

Skip Round 2 entirely if Round 1 came back clean and everything is now clear.

Signals to drill down:

- The team disagreed with each other
- The answer revealed a new dependency or scope question
- The answer is vague ("usually", "I think", "probably", "depends")
- The answer implies a constraint or behaviour not yet in the ticket
- The answer references something not in the PRD

When the user gives a clear, decisive answer with no new threads, move on. Do not over-refine.

---

### Step 5: Decide on a Technical Context section

Add a Technical Context section to the proposed ticket ONLY if at least one of these is true:

- The work clearly spans multiple files or services
- A non-obvious technical approach came up during refinement
- There are specific constraints an agent wouldn't infer from the story alone (performance, security, accessibility, data integrity, migration, auth)
- The team explicitly discussed implementation during the session

If none apply, leave it out. When included, format as:

```
**Technical Context:**
- Areas/files/services likely involved: ...
- Approach considerations: ...
- Constraints: ...
```

Keep it terse. Bullet points, not paragraphs.

---

### Step 6: Round 3 read-back and approval

Produce the proposed update as a side-by-side diff in the chat:

```markdown
## Proposed update for [JUS-XXXX]

### Current
**Title:** [original]
**Description:** [original, body only, not the rendered preview]
**Acceptance Criteria:** [original ACs]
**Labels:** [original]
**Priority:** [original]
**Dependencies:** [original: blockedBy / parent]

### Proposed
**Title:** [revised, often unchanged]
**Description:** [revised]
**Out of scope:** [single line or short bullet list. Include only if scope ambiguity warranted it]
**Acceptance Criteria:**
- [ ] [revised AC 1]
- [ ] [revised AC 2]
- [ ] [new edge case AC] *(new)*
**Labels:** [revised]
**Priority:** [revised]
**Dependencies:** [revised]
**Technical Context:** [only if Step 5 criteria met]
- ...

### What changed and why
- [one line per change, citing the refinement answer that drove it]
- ...
```

Then ask: **"Anything missing, wrong, or worth pushing back on?"**

If the user has changes, apply them and reproduce the Proposed block. Loop until they sign off.

---

### Step 7: Sign-off and write-back to Linear

When the user signs off:

1. Produce a final clean ticket in the format below.
2. Confirm one more time: "Write this back to JUS-XXXX in Linear?"
3. On confirmation, call `save_issue` with the issue `id`, updated `title`, `description` (markdown, Linear renders it), `priority`, `labels`, and `blockedBy` as needed.
4. Return the Linear URL to the user.

Final clean output format:

```
---
**Parent:** [JUS-YYYY: Feature / Initiative Name]
**Story:** [US-XX.X] As a [user], I want [goal] so that [benefit]
**Description:** [refined]
**Out of scope:** [if applicable]
**Acceptance Criteria:**
- [ ] Given [context], when [action], then [outcome]
- [ ] ...
**Labels:** [refined]
**Priority:** [refined: Urgent/High/Medium/Low/None]
**Dependencies:** [refined]
**Technical Context:** [if applicable]
- ...
---
```

Linear's description field is markdown. The AC checklist renders natively as actionable checkboxes.

---

## Ticket format additions

This skill extends the base ticket format with two optional additions:

1. **Out of scope line**: when scope ambiguity warranted explicit boundaries. Single line or short bullet list.

   Example:
   ```
   **Out of scope:** Bulk editing, mobile layout (separate ticket), admin permissions
   ```

2. **Technical Context section**: optional, criteria in Step 5.

Everything else (story format, Gherkin AC, labels, priority, dependencies) stays identical.

---

## Definition of done for refinement

A ticket is "refined" when ALL of these are true:

- Every acceptance criterion is in Given/When/Then and is testable
- At least one AC covers an error, edge case, or unhappy path
- Scope boundary is explicit when ambiguity existed (Out of scope line if needed)
- Dependencies on other tickets/systems are declared (or explicitly noted as none)
- The user has signed off on the proposed update
- The update has been written back to Linear (or the user has explicitly chosen to defer the write)

If any of these is missing, the skill is not done. Continue refining.

---

## Style

When writing copy for the user (the proposed update diff, the final ticket, anything that ends up in Linear or chat), follow these rules:

- **No em-dashes (—).** Em-dashes are a strong tell-tale sign of LLM-written prose. Use commas, parentheses, colons, full stops, or rephrase the sentence. Replace "X — Y" with "X, Y", "X (Y)", "X: Y", or two sentences.
- **En-dashes (–) are acceptable in numeric ranges** ("5–10 years", "9am–5pm") since they're standard typography. Avoid them in prose.
- **Avoid the words "genuinely", "honestly", "straightforward"**. These tend to read as filler or hedging.
- **Don't overuse bold or italics.** Bold a term once when it's first introduced as a defined concept, but don't bold for emphasis on every other sentence.
- **Write like a person**, not a documentation generator. Short sentences are fine. Conversational asides are fine. Read it aloud once to check.

These rules apply to the proposed ticket content that gets written to Linear AND to the chat copy shown during the diff/approval steps. If the user provides input that contains em-dashes, preserve their wording when quoting them directly but don't introduce new ones.

---

## Anchor question library

Reference list for picking Round 1 questions. Pick the best 3 to 5 per ticket, rephrased to fit. Not exhaustive. Add new ones as they prove useful.

**Scope**

- What's explicitly NOT in this ticket?
- Is there a smaller version of this that still delivers value?
- Could any part of this be a separate ticket?
- Where does this ticket end and the next one begin?

**User experience**

- What does the user see when [the most likely failure] happens?
- What states does the UI move through during this?
- Are there empty / loading / error states to design for?
- What does success look like to the user?
- What's the very first thing the user does that triggers this?

**Dependencies**

- What other tickets does this depend on or block?
- What external systems, APIs, or data does this touch?
- Are there any cross-team handoffs?
- Is anything here behind a feature flag or migration?

**Edge cases**

- What's the weirdest valid input here?
- What happens at scale, lots of records, slow network, concurrent users?
- What happens if the user navigates away mid-flow?
- What does this look like for an admin, a power user, a brand-new user?
- What if the user has never done this before? What if they've done it a thousand times?

**Validation**

- How would you verify this is done in five minutes?
- What's the test approach: unit, integration, manual, E2E?
- What evidence would prove this works in production?
- What would you want to see in a demo of this?

---

## When to stop and escalate

Refinement is the wrong move in these cases. Surface and stop:

- **The ticket should be split.** If refinement reveals the ticket is two or more meaningfully separate pieces of work, say so and recommend a split. Splitting is a separate workflow.
- **The PRD is the problem.** If the ticket is misaligned with the PRD, or the PRD itself is ambiguous on the relevant point, surface that. It may be a PRD update (use the `lean-prd` skill), not a ticket update.
- **The ticket shouldn't exist.** If refinement reveals the work isn't needed, or is already covered by another ticket, recommend closing or merging.

---

## Notes

- Refine **one ticket per session**. If the user gestures at the whole epic, ask which one to start with.
- This skill does NOT size (poker) or split. Those are separate concerns.
- The skill should feel like a thoughtful refinement partner, not a checklist.
- Read the PRD before reading the ticket. Context first, then detail.
- If a Round 1 batch comes back with mostly "I don't know" answers, the ticket isn't ready for refinement. It needs a stakeholder conversation first. Surface that.
- After write-back, if the ticket gained a `parent` link and the PRD's `Linked Linear Epic` is empty, offer to populate it.

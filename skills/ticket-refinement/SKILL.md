---
name: ticket-refinement
description: Use this skill when the user wants to refine a Jira ticket, prepare a story for development, run a refinement session, or get a ticket ready for an engineer to pick up. Trigger on phrases like "refine this ticket", "let's groom this story", "is this ready for dev?", "review this ticket with me", "let's prep this for build", or any request to take a draft ticket and improve it. This skill assumes the ticket is in user-stories-jira format with a parent PRD available, and produces a proposed update for human review before any write-back to Jira.
---

# Ticket Refinement Skill

## Overview

This skill takes one ticket at a time (in `user-stories-jira` format), the parent PRD (in `lean-prd` format), and optional context from sibling tickets in the same epic, and runs a structured refinement conversation that produces a proposed update for the human to approve.

The bar for "refined" is **human-ready, agent-friendly**: an experienced engineer can pick the ticket up confidently, and an AI planning agent has enough to draft an execution plan (though it may still ask follow-ups).

This skill does NOT write to Jira directly. It outputs a side-by-side proposal in the chat. The human approves changes before any write-back happens. Writing to Jira is the calling app's responsibility.

---

## Input expected

- **One ticket** in user-stories-jira format (Story, Description, Acceptance Criteria, Labels, Dependencies)
- **The parent PRD** in lean-prd format (Problem, Solution, User Stories, Open Questions)
- **Optional context**: other tickets in the same epic — useful for spotting dependencies and avoiding overlap

If the PRD or the ticket isn't provided, ask once before proceeding. Do not invent context.

---

## Process

### Step 1 — Silent prep pass

Read the ticket and the PRD. Privately assess the ticket against these categories. Do NOT show this assessment to the user — it shapes which questions to ask.

- **Scope clarity** — is it clear what's in and what's out of this ticket?
- **User experience** — are flows, states, and error paths described?
- **AC testability and coverage** — is every AC in Given/When/Then? Is there an edge/error case? Could a tester verify each one?
- **Dependencies** — are upstream/downstream tickets, systems, or data declared?
- **Edge cases** — what's the weirdest valid input? What happens on failure?
- **Technical context** — only relevant if criteria in Step 4 are met; do not pre-fill this for ordinary tickets

Use the assessment to pick the most useful **3–5 anchor questions** for Round 1. A tight ticket may only need 2–3 anchors; a vague one may need all 5. Do not ask a question whose answer is already in the ticket or PRD.

---

### Step 2 — Round 1: anchor questions (batch)

Ask 3–5 questions at once. These are the prompts that surface team knowledge best as a group — one person's answer triggers another's memory. Choose from the categories below, phrased to fit the specific ticket. Use the anchor question library at the bottom of this file for inspiration.

The five categories:

1. **Scope** — what's in, what's out, what's a separate ticket
2. **User experience** — flows, states, errors, success
3. **Dependencies** — other tickets, systems, data, teams
4. **Edge cases** — weird inputs, scale, interruptions, user types
5. **Validation** — how we'd verify done, test approach, evidence

Format the questions as a numbered list so answers can come back numbered.

---

### Step 3 — Round 2: conversational drill-down

For each Round 1 answer that exposed something interesting, follow up **one question at a time**. Pull on the thread until it's resolved, then move to the next.

Skip Round 2 entirely if Round 1 came back clean and everything is now clear.

Signals to drill down:

- The team disagreed with each other
- The answer revealed a new dependency or scope question
- The answer is vague ("usually", "I think", "probably", "depends")
- The answer implies a constraint or behavior not yet in the ticket
- The answer references something not in the PRD

When the user gives a clear, decisive answer with no new threads, move on. Do not over-refine — there's a cost to dragging out a session.

---

### Step 4 — Decide on a Technical Context section

Add a Technical Context section to the proposed ticket ONLY if at least one of these is true:

- The work clearly spans multiple files or services
- A non-obvious technical approach came up during refinement
- There are specific constraints an agent wouldn't infer from the story alone (performance, security, accessibility, data integrity, migration, auth)
- The team explicitly discussed implementation during the session

If none apply, leave it out. The Technical Context section is for priming the engineer's downstream AI agent, not for documenting the obvious.

When included, format as:

```
**Technical Context:**
- Areas/files/services likely involved: ...
- Approach considerations: ...
- Constraints: ...
```

Keep it terse — bullet points, not paragraphs. The engineer's agent will expand from this; over-specifying is worse than under-specifying.

---

### Step 5 — Round 3: read-back and approval

Produce the proposed update as a side-by-side diff in the chat. Use this exact format so the future app can parse it consistently:

```markdown
## Proposed update for [TICKET-ID]

### Current
**Story:** [original]
**Description:** [original]
**Acceptance Criteria:**
- [ ] [original AC 1]
- [ ] [original AC 2]
**Labels:** [original]
**Dependencies:** [original]

### Proposed
**Story:** [revised — often unchanged]
**Description:** [revised]
**Out of scope:** [single line or short bullet list — include only if scope ambiguity warranted it]
**Acceptance Criteria:**
- [ ] [revised AC 1]
- [ ] [revised AC 2]
- [ ] [new edge case AC] *(new)*
**Labels:** [revised]
**Dependencies:** [revised]
**Technical Context:** [only if Step 4 criteria met]
- ...

### What changed and why
- [one line per change, citing the refinement answer that drove it]
- ...
```

Then ask: **"Anything missing, wrong, or worth pushing back on?"**

If the user has changes, apply them and reproduce the Proposed block. Loop until they sign off.

---

### Step 6 — Sign-off and handoff

When the user signs off, produce a final clean ticket in user-stories-jira format (with the optional additions from this skill), ready to be picked up by the calling app and written to Jira. Do not write to Jira from this skill.

Final clean output format:

```
---
**Epic:** [Epic name]
**Story:** [US-XX.X] As a [user], I want [goal] so that [benefit]
**Description:** [refined]
**Out of scope:** [if applicable]
**Acceptance Criteria:**
- [ ] Given [context], when [action], then [outcome]
- [ ] ...
**Labels:** [refined]
**Dependencies:** [refined]
**Technical Context:** [if applicable]
- ...
---
```

---

## Ticket format additions

This skill extends `user-stories-jira` with two optional additions. Everything else (story format, Gherkin AC, labels, dependencies) stays identical.

1. **Out of scope line** — `**Out of scope:**` under Description, when scope ambiguity warranted explicit boundaries. Single line or short bullet list.

2. **Technical Context section** — optional, criteria in Step 4.

---

## Definition of done for refinement

A ticket is "refined" when ALL of these are true:

- Every acceptance criterion is in Given/When/Then and is testable
- At least one AC covers an error, edge case, or unhappy path
- Scope boundary is explicit when ambiguity existed (Out of scope line if needed)
- Dependencies on other tickets/systems are declared (or explicitly noted as none)
- The user has signed off on the proposed update

If any of these is missing, the skill is not done. Continue refining.

---

## Anchor question library

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

**Dependencies**
- What other tickets does this depend on or block?
- What external systems, APIs, or data does this touch?
- Is anything here behind a feature flag or migration?

**Edge cases**
- What's the weirdest valid input here?
- What happens if the user navigates away mid-flow?
- What does this look like for a brand-new user vs a returning user?

**Validation**
- How would you verify this is done in five minutes?
- What evidence would prove this works in production?

---

## When to stop and escalate

- **The ticket should be split** — surface and recommend a split
- **The PRD is the problem** — flag and surface for PRD update first
- **The ticket shouldn't exist** — recommend closing or merging

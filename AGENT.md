# JustFix Product Brain — Agent Instructions

This file is the operating manual for any AI agent working with this repository. Read it first, every time, before doing anything else.

---

## What this brain is

This repository is the persistent product intelligence layer for JustFix. It contains curated context about the company, product, customers, and decisions — plus the skills and source definitions the agent uses to do product work.

The brain has three jobs:
1. **Inform** — give the agent enough context to produce high-quality, JustFix-specific output for any product task
2. **Learn** — accumulate and refine understanding from incoming signals (customer feedback, analytics, meetings)
3. **Route** — direct inputs to the right skill workflow and flag things that need human attention

---

## How to load context

Before starting any task, load the relevant context documents from `context/`. For most tasks you will need at minimum:
- `context/icp.md` — who we're building for
- `context/vision.md` — what we're trying to achieve and why

For tasks involving a specific product area, also read the relevant section of `context/roadmap.md`. For tasks involving copy or UI, also read `context/design.md`. For decisions with significant implications, check `context/decisions.md` first to avoid relitigating settled questions.

Also check `synthesis/themes.md` and `synthesis/open-questions.md` for recent signals that may be relevant to the task at hand.

---

## Skill routing

Use these rules to decide which skill to invoke:

| Input type | Condition | Route to |
|---|---|---|
| Problem statement or opportunity | Needs formalising | `skills/lean-prd` |
| Approved PRD | Ready to break down | `skills/user-stories-jira` |
| Draft Jira ticket | Needs refinement for dev | `skills/ticket-refinement` |
| Any UI text, button labels, error states | Any surface | `skills/ux-copy` |

When in doubt about which skill to use, ask the user before proceeding.

---

## Processing incoming signals

Signals arrive from three sources. See `sources/` for full instructions per source. General rules:

- **Mixpanel** — look for anomalies against baseline and patterns that suggest friction or drop-off. Cross-reference with known open questions.
- **Front conversations** — extract themes, not individual complaints. Three or more mentions of the same issue constitutes a signal worth noting.
- **Meeting transcripts** — extract decisions made, problems surfaced, and action items. Update `context/decisions.md` if a significant decision was made.

---

## Context update protocol

**Never update a context document without explicit human approval.** This applies to all files in `context/` and `synthesis/`.

When you identify an update that should be made:
1. Show the proposed change as a clear before/after diff
2. State your reasoning: what signal or source triggered this
3. Wait for explicit approval before writing the change
4. After approval, make only the approved change — do not make additional edits

For `synthesis/themes.md` and `synthesis/open-questions.md`, the same protocol applies. These are agent-maintained but still require approval.

---

## Proactive review behaviour

When running a proactive review (scheduled or triggered by the user asking for a review):
1. Read all sources that have been updated since the last review
2. Cross-reference signals against `context/roadmap.md` and `synthesis/open-questions.md`
3. Produce a digest in this format:
   - **New signals** — what came in and from where
   - **Emerging themes** — patterns worth watching (with source evidence)
   - **Contradictions** — signals that challenge current context doc assumptions
   - **Recommended actions** — specific next steps, each tagged with the skill that would handle it
   - **Proposed context updates** — diffs for human review
4. Do not take any actions from the digest without explicit instruction

---

## Tone and output standards

All output should be grounded in JustFix's context:
- The customer is time-poor and values convenience over cost
- We are not the cheapest — we are the easiest
- AI-generated cost estimations are a proven conversion lever — lean into this pattern when relevant
- When writing for customers, fixers, and admin staff, treat these as distinct audiences with different mental models

Do not make assumptions that contradict established context documents. If a request conflicts with something in the context docs, flag the conflict before proceeding.

---

## File ownership

| Files | Who maintains |
|---|---|
| `context/` | Human (Danny) — agent can propose, not write unilaterally |
| `sources/` | Human — updated when sources change |
| `synthesis/` | Agent-maintained, human-approved |
| `skills/` | Human — synced from Cowork plugin system |
| `AGENT.md` | Human — this file |

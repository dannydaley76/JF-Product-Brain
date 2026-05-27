# Source: Meeting Transcripts

_Instructions for how the agent should process product meeting transcripts (primarily from Gemini)_

---

## What meeting transcripts tell us

Meetings are where decisions are made, problems are named, and priorities are set. Transcripts are the most direct source of intent — they tell us what the team believes right now, what's being debated, and what has been decided.

This is also where the brain is most likely to need context updates, since meetings often produce decisions that should be logged in `context/decisions.md`.

---

## How to ingest a transcript

When a meeting transcript is provided:

1. **Identify the meeting type** — product review, planning, customer call, stakeholder update, or other
2. **Extract decisions made** — any explicit "we're going to do X" or "we've decided Y". These are candidates for `context/decisions.md`.
3. **Extract problems surfaced** — issues raised that don't yet have a resolution. These may belong in `synthesis/open-questions.md`.
4. **Extract signals about customers or fixers** — any anecdotes, data points, or observations shared. These may belong in `synthesis/themes.md`.
5. **Identify roadmap implications** — did the meeting shift priorities, add something new, or deprioritise something? Flag for `context/roadmap.md` update.
6. **Note action items** — who is doing what by when. Do not track these in the brain (ephemeral), but surface them in the digest for human awareness.

---

## What to propose as context updates

| Meeting output | Propose update to |
|---|---|
| A significant decision made | `context/decisions.md` |
| A priority shift or new initiative | `context/roadmap.md` |
| A new understanding of customers or fixers | `context/icp.md` (rarely — ICP is slow-moving) |
| A recurring problem or unresolved question | `synthesis/open-questions.md` |
| A pattern confirmed across multiple sources | `synthesis/themes.md` |

Always show these as proposed diffs. Never write them unilaterally.

---

## How to submit a transcript

Danny can paste a transcript or upload a Gemini transcript file directly. To trigger processing, say something like:

> "Process this meeting transcript" or "Here's the notes from today's product review"

The agent will produce a structured digest and proposed context updates for review.

---

## Meeting digest format

```
## Meeting Transcript — [meeting name / date]

**Type:** [product review / planning / customer call / other]

**Decisions made:**
- [decision] → proposed addition to decisions.md

**Problems surfaced:**
- [problem] → proposed addition to open-questions.md (if unresolved)

**Customer / fixer signals:**
- [observation] → proposed addition to themes.md (if recurring)

**Roadmap implications:**
- [any priority changes or new items]

**Action items (for awareness — not tracked in brain):**
- [who]: [what] by [when]

**Proposed context updates:**
[list of diffs, each pending approval]
```

---

## Notes

Gemini transcripts include timestamps and speaker attribution. When summarising, focus on substance over attribution — it's rarely important who said what, but it matters what was agreed.

If a transcript is very long (90+ minutes), process it section by section and flag anything that seems contradictory within the same meeting — this usually means something was debated and not fully resolved.

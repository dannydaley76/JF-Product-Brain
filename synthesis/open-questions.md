# Open Questions

_Agent-maintained. All updates require human approval before being written._

This file tracks unresolved questions that are significant enough to warrant tracking — things the team hasn't decided yet, hypotheses that need validation, or tensions between what we're seeing and what we currently believe.

Open questions are distinct from themes: a theme is a pattern we've observed; an open question is something we don't yet know the answer to that matters for product direction.

---

## Active questions

### Can ElevenLabs extract structured responses (yes/no) from voice calls to drive conditional logic?
**Priority:** High
**Raised:** 2026-06-02
**Source:** Standup — Paul blocked on this during platform exploration

This is the critical technical blocker for the voice agent. Without it, the agent can't branch on fixer responses (confirm/decline/reschedule).

**What we'd need to answer it:**
- ElevenLabs docs / support on conditional actions
- Paul's exploration findings this week

**Linked roadmap item:** ElevenLabs voice agent

---

### Will fixers accept an AI voice agent for confirmation calls, or is a manual fallback needed long-term?
**Priority:** Medium
**Raised:** 2026-06-03
**Source:** "voice agent update and standup" — tested voices sound "too human"/uncanny; team noted adoption will take time, especially with builders, and manual option remains available

This affects how much we invest in the voice agent vs. keeping/maintaining manual confirmation paths.

**What we'd need to answer it:**
- Fixer reactions to a live MVP call
- Adoption / opt-out rates once deployed

**Linked roadmap item:** ElevenLabs voice agent

---

## Question format

```
### [Question]
**Priority:** High / Medium / Low
**Raised:** [date]
**Source:** [what triggered this question]

[1-2 sentence framing of why this matters]

**What we'd need to answer it:**
- [data source / experiment / conversation]

**Linked theme:** [[theme-slug]] (if applicable)
**Linked roadmap item:** [item name] (if applicable)
```

---

## Resolved questions

_Questions move here once answered, with a brief note on the answer and what changed._

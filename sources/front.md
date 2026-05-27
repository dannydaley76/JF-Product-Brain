# Source: Front (Customer Conversations)

_Instructions for how the agent should process customer conversations from Front_

---

## What Front tells us

Front is our customer communication tool. Conversations here are a direct signal from customers — what's confusing them, what's breaking, what they're frustrated about, and occasionally what they love. Unlike Mixpanel, Front reveals the *why* behind behaviour.

---

## How to ingest Front conversations

When Front data is provided (exports, conversation batches, or manual pastes), process in this order:

1. **Categorise by surface** — which product area is the conversation about? (Web booking, estimation, fixer attendance, post-job, billing, other)
2. **Identify sentiment** — is this a complaint, a question, a compliment, or a feature request?
3. **Extract the underlying job to be done** — what was the customer actually trying to do when they hit this problem?
4. **Look for repetition** — a single complaint is noise. Three or more mentions of the same issue in a given period is a signal.
5. **Cross-reference with Mixpanel** — does this feedback match a drop-off pattern we can see in the funnel?
6. **Cross-reference with open questions** — does this confirm, refute, or add nuance to anything in `synthesis/open-questions.md`?

---

## Signal thresholds

| Frequency | Action |
|---|---|
| 1-2 mentions | Note in raw synthesis. Do not escalate. |
| 3+ mentions in a week | Add to `synthesis/themes.md` as an emerging signal (pending approval) |
| 5+ mentions or a high-severity single issue | Flag for immediate human review |
| Recurring over 3+ weeks | Escalate to `synthesis/open-questions.md` as a candidate for roadmap consideration |

---

## Tags to apply when categorising

_To be updated as Front's actual tag taxonomy is confirmed._

- `estimation` — feedback about cost estimates
- `booking-flow` — friction in the booking journey
- `fixer-quality` — concerns about the fixer (lateness, quality of work)
- `fixer-no-show` — fixer did not attend
- `comms` — communication issues (didn't receive confirmation, unclear next steps)
- `billing` — pricing or payment issues
- `bug` — product broke in some way
- `positive` — compliment or praise

---

## How to format a Front digest

```
## Front Conversations — [date range]

**Volume:** [N conversations reviewed]

**By category:**
- [category]: [N conversations], [dominant sentiment]

**Emerging themes (3+ mentions):**
- [theme]: [N mentions], [representative quote if available]

**High priority signals:**
- [anything requiring immediate attention]

**Proposed synthesis updates:**
- [changes to themes.md or open-questions.md, shown as diffs, pending approval]
```

---

## Notes

Front data is qualitative — it represents customers who reached out, not all customers. Weight it against Mixpanel data where possible. A complaint that doesn't show up in funnel data may be a niche case; a complaint that aligns with a Mixpanel drop-off is a confirmed problem.

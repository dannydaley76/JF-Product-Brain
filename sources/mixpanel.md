# Source: Mixpanel

_Instructions for how the agent should process Mixpanel analytics data_

---

## What Mixpanel tells us

Mixpanel is our primary source for understanding what customers actually do — as opposed to what they say they do. It reveals friction points, drop-off, and behavioural patterns that Front conversations and meetings often lag behind.

---

## How to ingest Mixpanel data

When Mixpanel data is provided (as an export, paste, or screenshot), process it in this order:

1. **Identify the time window** — note the date range. Trends need a baseline to be meaningful.
2. **Look for anomalies** — significant drops or spikes vs the previous period. These are the first things to surface.
3. **Map to known funnels** — where does this data sit in the customer journey? (Discovery → estimation → booking → post-booking)
4. **Cross-reference with open questions** — check `synthesis/open-questions.md` for known hypotheses this data might answer or complicate.
5. **Cross-reference with roadmap** — does this data validate or challenge current priorities in `context/roadmap.md`?

---

## Key funnels to watch

_To be populated with actual Mixpanel event names as they are confirmed._

### Customer booking funnel
- [ ] Landing / entry event
- [ ] Estimation started
- [ ] Estimation viewed
- [ ] Booking started
- [ ] Booking confirmed

### AI estimation path vs traditional path
- This is the most important funnel split to track. Current known result: AI estimation path converts 2x vs hourly rate. Track whether this holds over time and across service categories.

### Fixer funnel (if tracked)
- [ ] SMS received (or WhatsApp message received)
- [ ] Link opened
- [ ] Job accepted / declined

---

## Signals worth escalating

Flag these for immediate human review rather than routine synthesis:

- Conversion rate on the AI estimation path drops more than 10% week-over-week
- A specific service category shows anomalous drop-off (could indicate estimation accuracy issue)
- Any funnel step drops to near-zero (likely a bug)
- Significant increase in time-to-book (suggests new friction somewhere)

---

## How to format a Mixpanel digest

When synthesising Mixpanel data for the weekly review or in response to a request, use this format:

```
## Mixpanel — [date range]

**Key metrics**
- [metric]: [value] ([change vs prior period])

**Anomalies**
- [describe any significant deviations]

**Funnel health**
- [AI estimation path]: [conversion rate]
- [Traditional path]: [conversion rate]

**Signals for review**
- [anything requiring human attention]

**Proposed context updates**
- [any changes to themes.md or roadmap.md, shown as diffs, pending approval]
```

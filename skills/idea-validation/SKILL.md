---
name: idea-validation
description: Score a product idea using a consistent RICA framework (Reach, Impact, Confidence, Effort) with weighted scoring, and write the result back to the Linear ticket. Use this skill whenever the user wants to validate, score, or prioritise an idea, review a Parking Lot ticket, decide whether to move an idea to Discovery, or run a scoring session on ideas. Trigger on phrases like "validate this idea", "score this idea", "should we pursue this", "RICA score", "is this worth building", "review the parking lot", or any request to evaluate or prioritise a product idea.
---

# Idea Validation

Score a product idea consistently using RICA, apply weighted scoring, and recommend whether it's ready to move from Parking Lot to Discovery.

## Why consistency matters

The only way RICA scores are useful is if they're comparable across ideas. That means the same questions, same scales, and same formula every time. Don't let the user shortcut the questions — even for ideas that feel obvious. A low-confidence gut feeling that something is high impact is exactly what RICA is designed to challenge.

## The scoring model

### Dimensions and scale (all 1–5)

**Reach** — How many customers will this affect?
| Score | Meaning |
|-------|---------|
| 1 | Very few — niche edge case, <5% of active customers |
| 2 | Small segment — 5–20% of customers |
| 3 | Moderate — 20–40% of customers |
| 4 | Most customers — 40–70% |
| 5 | Almost everyone — >70% of active customers |

**Impact** — How significantly does it affect them?
| Score | Meaning |
|-------|---------|
| 1 | Minimal — nice to have, wouldn't be missed |
| 2 | Minor — small improvement to an existing experience |
| 3 | Notable — meaningful time or frustration saving |
| 4 | Significant — removes a painful friction point or unlocks a clear job to be done |
| 5 | Transformative — changes how they use the product or enables something previously impossible |

**Confidence** — How confident are we in these estimates?
| Score | Meaning |
|-------|---------|
| 1 | Pure guess — no data or feedback |
| 2 | Anecdotal — one or two mentions, informal observation |
| 3 | Supported — customer feedback, qualitative research, or pattern across multiple sources |
| 4 | Confirmed — data or research directly validates the need |
| 5 | Validated — tested with real users or evidenced by clear behaviour |

**Effort** — How much work is this? (higher = worse)
| Score | Meaning |
|-------|---------|
| 1 | Trivial — hours to a day, one person |
| 2 | Small — a few days |
| 3 | Medium — one to two weeks |
| 4 | Large — three to four weeks, may need multiple people |
| 5 | Very large — over a month, significant complexity or dependencies |

### Formula

```
RICA Score = (Reach × 1.0  +  Impact × 2.0  +  Confidence × 1.5)  ÷  Effort
```

Impact is weighted 2x because it's the dimension most likely to differentiate ideas that feel similar. Confidence is weighted 1.5x to reward ideas grounded in real signals. Effort is a straight divisor — it doesn't reduce quality, it reduces throughput.

Score interpretation:
- **< 3** — Low priority. Revisit if Reach or Impact changes.
- **3–6** — Keep in Parking Lot. Worth watching, not yet ready.
- **6–10** — Good candidate for Discovery. Discuss timing.
- **> 10** — Strong candidate. Move to Discovery soon.

These thresholds are a guide, not a gate. A score of 5 with a specific strategic reason can outrank a score of 9 that doesn't align with current focus.

---

## Process

### Step 1: Identify the idea

Ask the user for the Linear ticket ID if not already given. Fetch the ticket using the Linear MCP to load the title and any existing description. Read it before starting so your calibrating questions are grounded in what's already known.

If the user wants to score multiple ideas in one session, do them one at a time — finish and write back one before starting the next.

### Step 2: Score each dimension

Work through the four dimensions in order: Reach, Impact, Confidence, Effort. For each one:

1. Ask the two calibrating questions below (same every time)
2. Based on the answers, propose a score with a one-line reason
3. Let the user confirm or push back — they can adjust, but ask them to explain why so the reasoning is captured

**Reach questions:**
- "How many of your active customers encounter this problem or would use this feature?"
- "Is this something all customers need eventually, or specific to a segment? Which one?"

**Impact questions:**
- "When a customer hits this problem today, what happens? What's the cost to them?"
- "If we built this, what would change for them in concrete terms?"

**Confidence questions:**
- "What evidence do we have that this is actually a problem? Where did the idea come from?"
- "Have we heard this from customers directly, or is it an internal assumption?"

**Effort questions:**
- "What's involved technically? New data model, new UI, third-party integration, or simpler?"
- "Does this depend on anything else being built first?"

### Step 3: Calculate and show the score

Once all four scores are confirmed, calculate:

```
Score = (Reach × 1.0 + Impact × 2.0 + Confidence × 1.5) ÷ Effort
```

Show the result clearly:

```
## RICA Score — [Idea title]

| Dimension  | Score | Weight | Weighted |
|------------|-------|--------|---------|
| Reach      | [R]   | ×1.0   | [R]     |
| Impact     | [I]   | ×2.0   | [I×2]   |
| Confidence | [C]   | ×1.5   | [C×1.5] |
| Effort     | [E]   | ÷      | —       |

**RICA Score: [score] / ~22 max**

Interpretation: [one sentence — e.g. "Good candidate for Discovery, particularly strong on impact."]
```

Then give a recommendation: stay in Parking Lot, move to Discovery, or deprioritise.

### Step 4: Write back to Linear

Update the Linear ticket description with the full scoring record. Preserve any existing content — append the RICA block at the top so it's immediately visible:

```markdown
## RICA Validation — [date]

| Dimension | Score | Reasoning |
|-----------|-------|-----------|
| Reach | [R]/5 | [one line] |
| Impact | [I]/5 | [one line] |
| Confidence | [C]/5 | [one line] |
| Effort | [E]/5 | [one line] |

**RICA Score: [score]** — [interpretation]

**Recommendation:** [Stay in Parking Lot / Move to Discovery / Deprioritise]
```

Use the Linear MCP `save_issue` tool to update the description.

### Step 5: Update status if moving to Discovery

If the recommendation is to move to Discovery and the user agrees, update the ticket status to `Discovery` using the Linear MCP.

If deprioritising, ask whether to move to `Won't do` or leave in Parking Lot for a later review.

---

## Tips

- If the user hasn't thought through an idea enough to answer the calibrating questions, that's a signal in itself — low confidence, probably stays in Parking Lot.
- Don't let high Impact override very low Confidence without flagging it. An exciting idea with no evidence is a bet, not a plan.
- Strategic alignment isn't in the formula but it matters. If an idea scores 8 but has nothing to do with big jobs, subscriptions, or the MCP — flag that before recommending Discovery.
- The goal isn't a perfect score. It's a consistent, defensible reason to prioritise one idea over another.

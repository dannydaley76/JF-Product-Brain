# Design Principles

_Last updated: 2026-05-27_

---

## Core principles

### 1. Effort is the enemy

Every additional tap, field, decision, or piece of information we ask the customer to process is a risk. Default to removing steps, not adding them. If something can be inferred, infer it. If a decision can be deferred, defer it.

### 2. Certainty builds trust

Ambiguity — about cost, timing, who's coming, what happens next — creates anxiety. Great JustFix experiences replace ambiguity with concrete information at the right moment. AI cost estimates are the clearest expression of this principle.

### 3. Different surfaces, consistent experience

We have three customer surfaces (native app, web app, ChatGPT app, Claude app) that should feel like the same product. The native app is currently the biggest gap. New features should be designed for consistency across surfaces from the start.

### 4. Fixers are professionals, not afterthoughts

The fixer experience deserves the same design rigour as the customer experience. A fixer who can't easily accept a job or log their work is a fixer who doesn't show up reliably — which breaks the customer experience downstream.

---

## Audience voice guide

### Customers (homeowners)

Tone: **Calm, capable, clear.** The customer is stressed about their problem. JustFix should feel like a competent friend who's handled this before. Not overly casual, not corporate. Reassuring without being patronising.

- Say: "We'll match you with a vetted plumber" — not "Our algorithm will optimise your booking"
- Say: "Here's an estimate based on your job" — not "AI-powered cost prediction"
- Avoid jargon. Avoid exclamation marks in functional contexts.

### Fixers (tradespeople)

Tone: **Direct, respectful, efficient.** Fixers are professionals. Give them the information they need with no fluff. They're reading on their phone, probably between jobs.

- Lead with the most important information (job type, location, time)
- Clear action labels — "Accept job", "I'm on my way", "Job complete"
- No marketing language

### Admin / ops team

Tone: **Functional and precise.** These are internal users who need to act quickly and accurately. Prioritise clarity over polish. Status labels, action labels, and error messages should be unambiguous.

---

## UI patterns to maintain

_To be populated as patterns are established. Examples:_

- AI estimation UI: show estimate range, not a single number. Always explain what's included.
- Booking confirmation: always state next step explicitly ("Your plumber will call to confirm timing")
- Error states: always tell the user what to do next, not just that something went wrong

---

## Notes for the agent

When writing UX copy or reviewing designs, apply the ux-copy skill (`skills/ux-copy`) and reference this document for tone and principle alignment. Flag anything that increases customer effort or introduces ambiguity about cost, timing, or next steps.

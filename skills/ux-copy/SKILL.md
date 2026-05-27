---
name: ux-copy
description: Use this skill when writing or reviewing UI copy for JustFix. Triggers include requests for button labels, error messages, empty states, tooltips, onboarding text, notifications, form labels, confirmation messages, success/failure states, or any in-product text. Also use when asked to review existing copy for tone or consistency. This skill covers three distinct audiences: customers (homeowners), fixers (tradespeople), and admin/internal staff.
---

# JustFix UX Copy Skill

## Brand Voice

JustFix copy is:

**Clear** — No jargon, no over-explaining. Customers don't need to understand boiler mechanics. Get to the point, solve the problem, let them get on with their day.

**Supportive** — A leaking tap might be minor to us, but it's been stressing someone out for weeks. We're the reliable, reassuring voice in a market where trust doesn't come easy.

**Grounded in Material Design principles:**
- Active voice, present tense
- Every word earns its place — cut anything that doesn't add meaning
- Be specific about what happens next
- Buttons are verb-led actions
- Errors explain what went wrong and what to do

---

## Audience Guidelines

JustFix has three distinct audiences. Write differently for each.

### Customers (Homeowners)
- Warm, reassuring, jargon-free
- Assume no technical knowledge
- Focus on outcomes, not process ("Your boiler's being looked at" not "A diagnostic check is in progress")
- Acknowledge the inconvenience — validate the frustration briefly, then move to resolution
- Use "you" and "your" — keep it personal

### Fixers (Tradespeople)
- Direct, respectful, peer-to-peer
- They're professionals — don't over-explain or patronise
- Focus on the job, the details, and what they need to do next
- Use plain trade language where appropriate (e.g. "job", "appointment", "callout")
- Efficiency matters — they're on the tools, not reading essays

### Admin / Internal Staff
- Functional and precise — they need to act quickly
- No need for warmth — clarity is the priority
- Use consistent terminology that matches the data model (e.g. "discount code", "job record", "fixer")
- Labels and actions should be unambiguous — no clever copy in admin interfaces

---

## Core Writing Rules

### Voice and tone
- Active voice: "We've booked your fixer" not "Your fixer has been booked"
- Present tense where possible: "Your code is applied" not "Your code has been applied"
- Contractions are fine for customers and fixers: "We'll", "You've", "Don't"
- Avoid contractions in admin interfaces — keep it formal and precise

### Length
- Titles and labels: as short as possible — 1–4 words
- Body copy: 1–2 sentences maximum for UI messages
- If you need more than 2 sentences, the UX probably needs rethinking

### Punctuation
- No full stops on standalone labels, buttons, or titles
- Full stops on multi-sentence body copy only
- No exclamation marks — supportive doesn't mean excitable
- Use sentence case for everything except proper nouns and product names

### Buttons and CTAs
- Always verb-led: "Book now", "Apply code", "View job", "Save changes"
- Never use: "Submit", "Click here", "OK", "Yes" on its own
- Pair destructive actions with clear consequence: "Delete code" not just "Delete"
- Confirmation buttons should restate the action: "Yes, delete code" / "Cancel"

---

## UI Patterns

### Success messages
- Customer: "Your discount's been applied — you're saving £20"
- Fixer: "Job confirmed. We'll send the customer's details shortly"
- Admin: "Code created successfully"

### Error messages
- Say what went wrong (specifically) + what to do next. Never blame the user.
- Customer: "That code isn't valid for this service. Check the code and try again, or contact us if you need help"
- Fixer: "We couldn't save your feedback. Try again or call us on [number]"
- Admin: "This code already exists. Enter a different code name"

### Empty states
- Customer: "No bookings yet. Ready when you are — book your first job"
- Fixer: "No upcoming jobs. We'll notify you when a new job comes in"
- Admin: "No discount codes. Create your first code to get started"

### Validation (inline form errors)
- Appear on field blur, not on submit. Short, specific, actionable.
- "Enter a valid email address" not "Email is invalid"

### Tooltips and helper text
- One sentence maximum. Only add if genuinely needed.
- Customer: "Your code will be applied to the cost of work, not the call-out fee"
- Admin: "Leave blank to allow unlimited uses"

### Notifications and alerts (WhatsApp / SMS)
- Fixers: "New job available: Boiler repair in Hackney. Tap to view details"
- Customers: "Good news — your fixer is confirmed for tomorrow at 10am"

### Confirmation prompts (destructive actions)
- Admin: "Delete this code? This can't be undone. Existing redemption records will be kept"

---

## Terminology — use these consistently

| Concept | Customer-facing | Fixer-facing | Admin-facing |
|---|---|---|---|
| The person doing the work | Fixer | — (self) | Fixer |
| The person booking | You / Customer | Customer | Customer |
| The booking | Job | Job | Job / Booking |
| A visit where no work is done | Call-out | Callout | Call-out |
| Discount code | Discount code | — | Discount code |
| The app/platform | JustFix | JustFix | — |

---

## What to avoid

- **Jargon:** "initiate", "utilise", "leverage" — use plain English
- **Vagueness:** "Something went wrong" — always be specific
- **Over-apologising:** one acknowledgement is enough
- **Padding:** "Please note that..." — just say the thing
- **Passive voice:** flip it
- **Exclamation marks:** ever
- **Ellipsis for loading states:** "Loading your jobs" or a spinner, not "Loading..."

---

## Process

When asked to write UI copy:
1. Identify the audience — customer, fixer, or admin
2. Identify the UI pattern — success, error, empty state, button, notification, etc.
3. Draft the copy following the rules above
4. Provide 2–3 variants if tone could reasonably vary
5. Flag if the requested copy suggests a UX problem

When asked to review existing copy:
1. Check against voice, tone, and rules above
2. Flag specific issues with a suggested fix
3. Don't rewrite everything — only flag what genuinely needs changing

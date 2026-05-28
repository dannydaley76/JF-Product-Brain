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

_Last updated from Mixpanel Production project (project ID: 3303016) — 56 events as of May 2026._

### Customer booking funnel (FE events)
1. `FE - Book Now clicked in top nav`
2. `FE - Booking - Service selected`
3. `FE - Booking - Date Selected` _(73 vol — canonical; `FE - Booking - Select date` at 51 vol may overlap)_
4. `FE - Booking - Address selected` _(lowercase s, 50 vol — canonical; capital-S variant is dead)_
5. `FE - Booking - Field changed` _(captures description, name, email, phone fields — replaces dedicated field events)_
6. `FE - Booking - Login modal opened` / `FE - Booking - Signup modal opened` _(login modal has volume; signup modal shows 0 — investigate)_
7. `FE - Booking - Signup success` / `FE - Booking - Login success` _(login success shows 0 vol in Lexicon — investigate)_
8. `FE - Booking - Confirm button clicked`
9. `FE - Booking - Booking requested` _(carries `booking` object)_
10. `FE - Booking - Success` _(carries `bookingId`, `bookingValue`)_
11. `FE - Booking - Visited booking complete page` _(0 vol in Lexicon — may be broken or renamed)_

### AI estimation path (key split to watch)
1. `FE - AI Estimator Clicked`
2. `FE - AI Estimator - Get Estimate Clicked`
3. `FE - AI Estimator - Booking Suggestion Clicked`
→ then merges into booking funnel above

This is the most important funnel split. Current known result: AI estimation path converts 2x vs hourly rate. Track whether this holds over time and across service categories.

### Server-side booking lifecycle (Admin-tagged events)
1. `Booking Created`
2. `Booking Fixer Match Started`
3. `Booking Fixer Matched` / `Booking No Fixers Matched`
4. `Booking Fixer Accepted` / `Booking Fixer Accepted With Conditions` / `Booking Fixer Rejected`
5. `Booking Payment Successful`
6. `Booking Balance Paid`
7. `Booking Cancelled` / `Booking Abandoned`
8. `Appointment Fixer No Show`

### Fixer funnel
_Not yet instrumented in Mixpanel. WhatsApp Flows tracking TBD._

---

## Event taxonomy

All events from the Production project, organised by category.

### AI Estimator
| Event | Tag | Notes |
|-------|-----|-------|
| `FE - AI Estimator Clicked` | | Entry point to AI estimation flow |
| `FE - AI Estimator - Get Estimate Clicked` | | User requests estimate |
| `FE - AI Estimator - Booking Suggestion Clicked` | | User acts on AI suggestion |

### Booking flow (frontend)
| Event | Tag | Notes |
|-------|-----|-------|
| `FE - Book Now clicked in top nav` | | Top-of-funnel entry |
| `FE - Booking - Service selected` | FrontEnd | |
| `FE - Booking - Date Selected` | FrontEnd | |
| `FE - Booking - Select date` | | Possible duplicate of above — check |
| `FE - Booking - Address Selected` | FrontEnd | |
| `FE - Booking - Address selected` | | Possible duplicate — note casing difference |
| `FE - Booking - Address entry mode changed` | | |
| `FE - Booking - Autoselected default property` | FrontEnd | |
| `FE - Booking - Changed property selection` | FrontEnd | |
| `FE - Booking - Job Description Entered` | FrontEnd | |
| `FE - Booking - Urgent selected` | FrontEnd | |
| `FE - Booking - Field changed` | | Generic field interaction |
| `FE - Booking - Next button clicked` | | Step progression |
| `FE - Booking - Back arrow click` | | Step regression |
| `FE - Booking - Change service clicked` | | |
| `FE - Booking - Upload image` | FrontEnd | |
| `FE - Booking - Validation Error` | | Friction signal |
| `FE - Booking - Terms and Conditions changed` | | |
| `FE - Booking - Add card modal opened` | FrontEnd | Payment step |
| `FE - Booking - Confirm button clicked` | | Pre-submission |
| `FE - Booking - Booking requested` | | Submission |
| `FE - Booking - Success` | FrontEnd | Confirmed |
| `FE - Booking - Visited booking complete page` | FrontEnd | |

### Auth within booking flow
| Event | Tag | Notes |
|-------|-----|-------|
| `FE - Booking - Signup modal opened` | FrontEnd | |
| `FE - Booking - Signup success` | FrontEnd | |
| `FE - Booking - Login modal opened` | FrontEnd | |
| `FE - Booking - Login success` | FrontEnd | |
| `FE - Booking - Email already exists, open login modal` | | |
| `FE - Booking - OTP successfully entered` | | |
| `FE - Booking - Forgot password modal opened` | FrontEnd | |

### General auth & account
| Event | Tag | Notes |
|-------|-----|-------|
| `FE - Sign Up` | FrontEnd | Outside booking flow |
| `FE - Login` | FrontEnd | Outside booking flow |
| `FE - payment method added` | FrontEnd | |
| `FE - property added` | FrontEnd | |
| `FE - booking cancelled` | FrontEnd | Customer-initiated cancellation |
| `FE - My Bookings clicked in top nav` | | |

### Server-side / admin
| Event | Tag | Notes |
|-------|-----|-------|
| `Booking Created` | Admin | |
| `Booking Fixer Match Started` | Admin | |
| `Booking Fixer Matched` | Admin | |
| `Booking No Fixers Matched` | Admin | Watch for spikes — supply gap signal |
| `Booking Fixer Accepted` | Admin | |
| `Booking Fixer Accepted With Conditions` | Admin | |
| `Booking Fixer Rejected` | Admin | |
| `Booking Payment Successful` | Admin | |
| `Booking Balance Paid` | Admin | |
| `Booking Cancelled` | Admin | |
| `Booking Abandoned` | Admin | |
| `Appointment Fixer No Show` | | High-priority signal |

### AI / aggregate
| Event | Tag | Notes |
|-------|-----|-------|
| `All AI` | | Any event where the booking flow was initiated via the AI estimator. Used to segment AI-path bookings vs traditional path. |

### Mixpanel system events (exclude from funnels)
| Event | Notes |
|-------|-------|
| `$mp_web_page_view` | Auto page view |
| `$mp_session_record` | Session replay — hidden |
| `$session_start` | Virtual session event |
| `$session_end` | Virtual session event |

---

## Key event properties

Top-level properties confirmed via Lexicon CSV export (May 2026).

| Event | Property | Type | Notes |
|-------|----------|------|-------|
| `FE - Booking - Success` | `bookingId` | number | Confirmed booking ID |
| `FE - Booking - Success` | `bookingValue` | number | Monetary value of booking |
| `FE - Booking - Booking requested` | `booking` | object | Full booking object — opaque in Lexicon, sub-fields not exposed. Ask engineering for booking model schema or inspect via session replay. |
| `FE - Booking - Validation Error` | `errors` | object | Validation errors at point of failure |
| `FE - Booking - Field changed` | `field` | string | Field name: `details`, `email`, `first_name`, `last_name`, `phone` |
| `FE - Booking - Autoselected default property` | _(address fields)_ | mixed | Full address object sent flattened: `address_line_1`, `address_line_2`, `address_line_3`, `address_line_4`, `company_name`, `county`, `created_at`, `deleted_at`, `floor`, `id`, `last_indexed`, `lat`, `lift_access`, `lon`, `name`, `offstreet_parking`, `permitted_parking`, `postcode`, `property_type`, `town_or_city`, `type`, `updated_at` |
| `Booking Payment Successful` | `amount` | number | Payment amount |
| `FE - Booking - Service selected` | `serviceName` | string | Service category selected |

> **Note on `booking` object**: Mixpanel Lexicon treats this as an opaque blob — internal fields are not enumerable via MCP or CSV export. To map the full schema, either inspect a specific event via session replay, or ask engineering to document the booking model that gets serialised here.

---

## Known data quality issues

_Volume data from Lexicon CSV export, 28 May 2026._

- **Duplicate address events — CONFIRMED**: `FE - Booking - Address selected` (lowercase s) has **50 volume** — this is the active event. `FE - Booking - Address Selected` (capital S, tagged FrontEnd) has **0 volume** — dead duplicate. Recommend hiding/deleting the capital-S version in Lexicon and confirming with engineering that it's no longer fired.
- **Duplicate date events — UNCONFIRMED**: `FE - Booking - Date Selected` (73 volume, FrontEnd tagged) appears to be primary. `FE - Booking - Select date` (51 volume) may overlap or cover a different surface — verify with engineering before deprecating.
- **Zero-volume events** (effectively dead — confirm before removing):
  - `FE - Booking - Login success` (0) — login success may now fire under a different name
  - `FE - Booking - Job Description Entered` (0) — check if replaced by `FE - Booking - Field changed`
  - `FE - Booking - Urgent selected` (0) — feature may have changed
  - `FE - Booking - Visited booking complete page` (0)
  - `FE - Booking - Signup modal opened` (0) — check against `FE - Booking - Login modal opened` (which has volume)
  - `FE - Booking - Changed property selection` (0)
  - `Booking Fixer Match Started` (0)
  - `Booking Fixer Matched` (0)
  - `Booking No Fixers Matched` (0) — **flag**: this should not be zero if fixer supply gaps exist; investigate whether server-side event is broken or if all bookings are matching
- **Fixer Onboarding, KPIs, Marketing, Admin projects**: Only session start/end events present — either not instrumented or data is routed elsewhere.
- **`All AI` event**: Custom aggregate segment — fires on any booking initiated via the AI estimator. Description added to Lexicon (pending).

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

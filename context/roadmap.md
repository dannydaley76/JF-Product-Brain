# Roadmap

_Last updated: 2026-06-03_

This document tracks current and near-term product priorities across all surfaces. It should be updated as priorities shift. It is a working document, not a polished plan.

---

## Active / in progress

### AI estimation — web app
Pushing AI-generated cost estimations on the web app as the primary conversion path. This has proven 2x conversion vs the traditional hourly rate path. Current focus: refining accuracy and expanding coverage.

### Fixer WhatsApp Flows migration
Moving fixer interactions (accept booking, attendance check, feedback/invoice submission) from SMS + simple web app to WhatsApp Flows. Goal: more reliable, app-like experience that reduces no-shows and friction.

**WhatsApp admin relay** (Anthony) — relay app between Twilio and Front now built. Contact relay through admin working; template sending added; 24-hour window auto-checking implemented; problem reports now generate replies (not comments). Deployed to staging 5 Jun; inactive until QA passes. Full production deploy planned for Monday 8 Jun. Admin team to be notified about new relay channel; phone numbers (Twilio + Meta) still need documenting in Notion. Anthony away from 6 Jun for 3 weeks.

**Discount codes** — admin backend ready for production. Deploying admin features only; frontend and mobile on hold until QA complete (in progress 4 Jun afternoon).

### ElevenLabs voice agent
Exploring an AI voice agent (via ElevenLabs) to handle outbound fixer confirmation calls. ElevenLabs integration now mostly complete with the questions framework established — MVP nearly ready for testing. Next: call triggering and information passing once the MVP is complete. Open risks: voices tested so far sound "too human" (uncanny valley) — voice selection still open; yes/no response extraction / conditional action logic not yet resolved.

---

## Near term

### Big Jobs MCP server — boiler estimator
AI-powered estimation for bigger jobs (boiler services, fixes, installs). Approach: decision tree (not fully conversational), replicating the iHeat/BuildTrade estimation model. Prototype expected ~2 weeks after kickoff.

### Claude app — discovery
The Claude app is not yet in discovery. It should provide the same AI estimation experience as the ChatGPT app and web app. Discovery should identify what Claude-specific capabilities can add unique value beyond estimation.

### ChatGPT app — AI estimation
The ChatGPT app provides an estimation-like experience. Status of feature parity with web app to be confirmed.

---

## Backlog / known issues

### Native app — web app parity
The native app is significantly behind the web app in feature parity and UX consistency. Given the occasional-use nature of the product, this may not be the highest priority — but the gap creates inconsistency for customers who do use it.

### Admin system improvements
The ops team manages bookings through an admin system. Known workflow friction points to be documented here as they are surfaced.

---

## Surfaces at a glance

| Surface | Audience | Status | Priority |
|---|---|---|---|
| Web app | Customers | Active development | High |
| WhatsApp Flows | Fixers | In progress | High |
| ChatGPT app | Customers | Live | Medium |
| Claude app | Customers | Pre-discovery | Medium |
| Admin system | Ops team | Maintained | Medium |
| Native app | Customers | Lagging | Low |

---

_Note: This roadmap reflects what was known as of the last update date. For the most current priorities, verify with Danny directly or check recent meeting synthesis in `synthesis/`._

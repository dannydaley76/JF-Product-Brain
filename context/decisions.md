# Key Decisions

_A running log of significant product decisions, why they were made, and what they constrain._

This document exists to prevent good decisions from being relitigated and bad ones from being forgotten. When in doubt, check here before proposing something that might conflict with an established direction.

---

## 2026

### AI estimation as primary conversion path — web app
**Decision:** Invest in AI-generated cost estimations as the primary customer conversion path, replacing the traditional hourly rate presentation.
**Evidence:** AI estimation path converts at 2x the rate of the hourly rate path.
**Rationale:** Our ICP is time-poor and convenience-oriented. A concrete cost estimate upfront removes the single biggest source of friction and anxiety in the booking journey.
**Constraints this creates:**
- New customer-facing features should consider how they interact with the estimation UX
- The estimation model's accuracy is now a trust signal — errors undermine the whole value proposition
- This pattern should be extended to new surfaces (Claude app, ChatGPT app) rather than reinvented per surface

---

_Add new decisions here as they are made. Format: heading with date, decision, evidence/rationale, constraints created._

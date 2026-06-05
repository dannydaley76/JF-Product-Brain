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

### Atlassian → Linear + Notion (tooling migration)
**Decision:** Switch from Atlassian (Jira/Confluence) to Linear + Notion for product and engineering tooling.
**Evidence:** Atlassian MCP server too slow (60s delays, excessive token use); Linear offers better workflow fit.
**Rationale:** Speed and integration quality — Linear + Notion aligns better with current team tooling and AI-assisted workflows.
**Constraints this creates:**
- Jira references in existing skills/docs should be updated to Linear over time
- Atlassian MCP is legacy — do not invest further in it

---

### WhatsApp relay app as Twilio ↔ Front intermediary
**Decision:** Use a purpose-built relay app (built by Anthony/Tol) as the intermediary between Twilio and Front, rather than a direct integration.
**Evidence:** Twilio and Front could not be used simultaneously with the previous direct setup.
**Rationale:** The relay keeps Twilio for send/receive while preserving Front display, gives better visibility into message flow, and tracks all activity through the admin system.
**Constraints this creates:**
- New WhatsApp messaging features should route through the relay, not a direct Twilio↔Front integration
- Feature activation is controlled via an admin toggle / DB settings (not UI)

---

### Discount codes — deploy admin-only first, hold frontend until QA
**Decision:** Deploy discount codes admin backend to production immediately; hold frontend and mobile deploys until QA is complete.
**Evidence:** Admin backend was tested and ready on 4 Jun; frontend and mobile had not yet been QA'd.
**Rationale:** Ship only what's tested. Avoids releasing broken UI while getting admin value into prod quickly.
**Constraints this creates:**
- Discount code frontend/mobile should not go to prod until QA passes
- Sets a pattern: split backend/frontend deploys are acceptable when one side is ready and the other isn't

---

### Big Jobs: decision tree approach for boiler estimator MCP server
**Decision:** Use a decision tree (not a fully conversational AI) as the initial approach for the Big Jobs MCP server, replicating the iHeat/BuildTrade estimation model.
**Evidence:** Discussed in Roadmap catchup, 4 Jun 2026. Decision tree seen as faster to prototype and easier to control than open-ended conversation.
**Rationale:** Gets a working boiler estimator to prototype stage faster; can evolve toward conversational later once the core model is validated.
**Constraints this creates:**
- Initial MCP server should not attempt free-form conversation — structured decision tree only
- Design should accommodate future evolution toward more conversational flows

---

### Ship to production behind toggles to avoid long-lived branches
**Decision:** Push code to production ASAP — even when not actively used — gated behind an admin toggle.
**Rationale:** Avoids long-lived branches and merge conflicts.
**Constraints this creates:**
- New features should ship dark behind a toggle rather than waiting on a feature branch

---

_Add new decisions here as they are made. Format: heading with date, decision, evidence/rationale, constraints created._

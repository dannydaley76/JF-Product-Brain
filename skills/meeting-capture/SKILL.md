---
name: meeting-capture
description: Use this skill when the user wants to process meeting notes, capture a meeting in the Product Brain, summarise a Slack-posted meeting transcript, or extract action items from a recent conversation. Trigger on phrases like "process this meeting", "capture today's standup", "summarise this refinement", "what did we agree", "extract action items", or any request to turn a Granola/Slack meeting note into a structured Notion entry. Pulls source from Slack (#meeting-notes channel and DMs to self), creates a row in the Meetings database, and proposes Linear tickets for action items.
---

# Meeting Capture Skill

## Where things live

JustFix records meetings as structured rows in the **Meetings database** inside the **Product Brain** Notion workspace. Granola captures live meeting notes (free tier, deletable). The user copies finalised notes to Slack, which is the durable holding pen. This skill turns those Slack posts into Notion rows and Linear tickets.

- **Notion workspace home**: Product Brain
- **Meetings database data source ID**: `c182a256-83d3-4148-9d1a-9c6be13ecaa4`
- **PRDs database data source ID** (for cross-linking): `d8c21fd9-00ce-4aa3-a845-49dc36b2d9c5`
- **Linear team**: Just-fix, ID `53e25529-6f66-44b7-80cf-6020401d5bf3`
- **Slack sources to scan**:
  - `#meeting-notes` (public, channel ID `C0B72UDPVEG`): standups, refinements, retros, team meetings, ops feedback when non-sensitive
  - DM to self (`@danny`, user ID `U06E4TK5RSQ`): strategic chats with CEO, sensitive ops feedback
- **Processing marker**: a ✅ emoji reaction on a Slack message means the note has been processed. Only process messages without it.

## Database fields

- `Title` (TITLE): short descriptive name, e.g. "Standup 2026-05-28" or "Refinement: Bigger Job Estimator"
- `Date` (DATE): when the meeting actually happened, not when notes were posted
- `Type` (SELECT): Stand-up / Refinement / Strategic / Ops feedback / Customer / Other
- `Attendees` (RICH_TEXT): free text list of attendees by name
- `Linked PRDs` (RELATION to PRDs database): any PRDs referenced or affected
- `Linked Linear` (URL): Linear issue or project this meeting relates to
- `Source` (URL): permalink to the Slack message holding the original note
- `Status` (SELECT): Captured / Processed / Actioned
- `Last Edited` (LAST_EDITED_TIME): auto-managed

Page body holds: summary, key decisions, action items with owners and Linear ticket links, notable quotes, anything worth extracting from the raw notes.

## Process

### Step 1: Identify the source

If the user pasted notes directly in chat, use those and skip Slack scanning. Proceed to Step 2.

Otherwise, read recent Slack messages from the configured sources:

1. `slack_read_channel` on `C0B72UDPVEG` (#meeting-notes) for recent messages
2. `slack_read_channel` on the current user's own user_id (DM to self) for sensitive notes

Filter out any message that already has a ✅ reaction. Use `slack_get_reactions` to check before processing.

If multiple unprocessed meetings are found, ask the user which to process (or "all of them" if they want a batch).

### Step 2: Classify the meeting

From the note content and the channel it was posted in, classify the meeting type:

- Standups usually have a "Yesterday / Today / Blockers" structure
- Refinements reference specific tickets or PRDs
- Strategic conversations name strategy, direction, or vision topics
- Ops feedback names ops processes, fixer/customer experience, support themes
- If unclear, ask the user

### Step 3: Extract structured data

From the note, identify:

- **Title**: short descriptive name. For standups, include the date. For other types, include the topic.
- **Date**: from the note, or today if not stated.
- **Attendees**: any names mentioned. Free text.
- **Summary**: 2 to 4 sentences capturing what was discussed and any conclusions.
- **Key decisions**: bullet list of explicit decisions made.
- **Action items**: bullet list with owner and clarity rating. Format each as: `[clear|ambiguous] [Owner] action description`.
- **Linked PRDs**: any PRD names mentioned. Look them up in the Notion PRDs database via `notion-search` to confirm page IDs and URLs.
- **Linked Linear**: any Linear issue identifiers (JUS-NNNN) or project URLs mentioned.

An action item is **clear** when it has an unambiguous owner, a verb, and an object ("Anthony updates the Worcester boiler list by Friday"). It's **ambiguous** when any of those is missing ("Someone should look into pricing").

### Step 4: Preview to user

Show the drafted Notion entry in chat with all extracted fields and body content. For action items, mark each one as **clear** (auto-create Linear ticket) or **ambiguous** (proposed but needs user approval).

Format:

```
**New Meetings row:**

- Title: [name]
- Date: [date]
- Type: [type]
- Attendees: [list]
- Linked PRDs: [list of PRD names with URLs]
- Linked Linear: [URLs]
- Source: [Slack permalink]
- Status: Captured

**Body:**
[summary / decisions / action items / quotes]

**Action items to create as Linear tickets:**
- [CLEAR] [Owner] does [thing] (will auto-create)
- [AMBIGUOUS] Someone should look into [thing] (proposed: assign to ? with description "...")
```

Then ask: "Save the Notion entry and create the clear-action tickets? Want me to revise any ambiguous ones first?"

### Step 5: Write to Notion

On approval, create the Notion page using `mcp__ebc1696e-54ae-4c1b-9d1e-ac120cbd538c__notion-create-pages` with:

```json
{
  "parent": {
    "type": "data_source_id",
    "data_source_id": "c182a256-83d3-4148-9d1a-9c6be13ecaa4"
  },
  "pages": [{
    "properties": {
      "Title": "[name]",
      "Type": "[type]",
      "Attendees": "[free text]",
      "Linked Linear": "[URL]",
      "Source": "[Slack permalink]",
      "Status": "Captured",
      "date:Date:start": "[YYYY-MM-DD]"
    },
    "icon": "🗣️",
    "content": "[body markdown]"
  }]
}
```

For "Linked PRDs" relation, set it via `notion-update-page` with `command: "update_properties"` after the page is created. The relation column accepts a JSON array of PRD page URLs.

### Step 6: Create Linear tickets for clear action items

For each action item marked **clear**:

- Call `mcp__a060d5e1-5a65-485c-b7a5-bdd9e5db93f1__save_issue` with `team: "53e25529-6f66-44b7-80cf-6020401d5bf3"`, title (verb + object), description (1 to 2 sentences plus a link to the Notion meeting row), assignee (owner if known and mentionable), priority (default Medium / 3).
- Capture the returned Linear identifier.

For each **ambiguous** action item, follow up in chat with a proposed ticket draft. Wait for user approval before creating.

After all tickets are created:

- Update the Notion entry's `Linked Linear` field with a comma-separated list of created issue URLs, OR extend the body's action items list with the Linear identifiers inline.
- Update the Notion entry's `Status` from `Captured` to `Actioned`.

### Step 7: Mark the Slack source as processed

Add a ✅ reaction to the Slack message that held the original note, so the next sweep skips it. Use `slack_add_reaction` with `name: "white_check_mark"` and the channel/timestamp parsed from the Source permalink.

## Style

When writing copy for the user (Notion entry body, Linear ticket descriptions, action items, anything persisted or shown in chat), follow these rules:

- **No em-dashes (—).** Em-dashes are a strong tell-tale sign of LLM-written prose. Use commas, parentheses, colons, full stops, or rephrase. Replace "X — Y" with "X, Y", "X (Y)", "X: Y", or two sentences.
- **En-dashes (–) are acceptable in numeric ranges** ("5–10 years", "9am–5pm"). Avoid them in prose.
- **Avoid the words "genuinely", "honestly", "straightforward"**. They read as filler or hedging.
- **Don't overuse bold or italics.** Bold a term once when first introduced as a defined concept, but not for emphasis on every other sentence.
- **Write like a person**, not a documentation generator. Short sentences are fine. Conversational asides are fine.

If the user provides input that contains em-dashes (e.g. inside a Granola transcript), preserve their wording when quoting them directly but don't introduce new ones.

## Notes

- One meeting per Notion row. If a Slack post contains notes from two different meetings, ask the user to split.
- The existing `#stand-ups` channel is for individual async daily updates. Those are NOT meetings. Don't ingest them through this skill.
- Action items without a clear owner default to assigning to the user (Danny). Flag this in the preview so they can reassign.
- The user can paste a Granola transcript directly in chat without going via Slack. Handle that path too: skip Steps 1 and 7, proceed from Step 2.
- Sensitive content (CEO chats, ops feedback about specific people) should never be quoted in a Linear ticket description visible to the wider team. When in doubt, ask the user before writing back.
- This skill works alongside `lean-prd`, `user-stories-jira`, and `ticket-refinement`. A refinement meeting note often produces both a Notion meeting row AND ticket updates. After capturing the meeting, suggest the user run `ticket-refinement` on any tickets the meeting touched.

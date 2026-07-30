---
name: submission-triage
version: 0.1.0
description: >
  Scores incoming insurance submissions and quote requests by premium size,
  win-likelihood, and carrier appetite fit, producing a "quote these first"
  list with what's missing before it can be submitted. Use when the user
  asks to prioritize submissions, which quotes to work first, or about their
  new-business pipeline. Falls back to a CSV/spreadsheet export when no AMS
  connector is live.
compatibility: "Requires an AMS/CRM connector, or a CSV export of open submissions (fallback). See reference/csv-format.md for the expected columns."
---

# Submission Triage

## Quick start

Pull open submissions, score them, and surface a ranked work list showing what's
missing before each can go out to carriers. Drafts producer follow-ups and
proposes calendar holds for underwriter calls — never sends or submits without
owner approval.

```
User: "what should I quote first today?"
→ Pull open submissions: stage = New or In Review, not yet bound or declined
→ Score each on premium size, win-likelihood, and appetite fit
→ Return ranked work list (size adapts to volume) with what's missing
→ Offer to draft producer follow-ups and propose underwriter call slots
```

## Workflow

1. **Pull open submissions.** Fetch records where status is `New` or `In Review`
   and not already `Bound` or `Declined`. Use the field list in
   [reference/submission-fields.md](reference/submission-fields.md) — line of
   business, target premium, effective date, named insured, carrier(s) being
   marketed.

   **If no AMS/CRM connector is live**, ask whether a CSV/spreadsheet export
   of open submissions is available instead: *"No AMS connector is
   connected — do you have a submissions export (CSV or spreadsheet) I can
   work from?"* If yes, read it per
   [reference/csv-format.md](reference/csv-format.md) and proceed with the
   same scoring workflow, noting in the output that this run is based on an
   uploaded export rather than live data (so any submission bound or
   declined since the export was pulled won't be reflected). If no connector
   and no file is available, stop: *"No submission data available — connect
   your AMS/CRM or upload a submissions export to run this."*

2. **Clarify if the trigger is ambiguous.** If the user said only "pipeline"
   without a qualifier, ask: *"Quick pipeline overview (submissions by stage
   and total premium) or a prioritized work list for today?"* — then route
   accordingly. Do not score submissions on a bare "pipeline."

3. **Score each submission.** Apply the four-dimension model in
   [reference/submission-fields.md](reference/submission-fields.md):
   - **Premium size** — larger accounts weighted higher, scaled to the agency's typical book (default: $5K–$50K annual premium band; ask if the agency runs larger commercial accounts)
   - **Carrier appetite fit** — does the line of business, class code, and account size match a carrier already being marketed to, or does this need new markets
   - **Win-likelihood** — incumbent relationship, exclusivity, whether it's a rewrite of an expiring account vs. new business, broker-of-record status
   - **Urgency** — days to effective date; anything inside 15 days gets an urgency bump regardless of premium

4. **Build the ranked work list.** Sort descending by composite score. Adapt
   list size to volume:
   - ≤10 submissions → show all
   - 11–30 submissions → show top 5
   - >30 submissions → show top 8

   For each submission: named insured, line of business, target premium,
   effective date, score, and **what's missing to quote** (e.g. "loss runs
   not yet received," "no SOV for the property schedule," "need signed
   application"). If effective date has already passed, flag it clearly
   rather than burying it in the ranking.

5. **Offer follow-up drafts.** Ask: *"Draft follow-ups for any of these?"* If
   yes, write one email or message per selected submission — to the producer
   or the retail broker — requesting the specific missing item. Show draft;
   do not send.

6. **Offer calendar slots.** Ask: *"Propose underwriter call slots for any of
   these?"* If yes, check the calendar for open windows in the next two
   business days. Propose two options per submission. Do not create events —
   the owner books.

## Approval gates

- **Never send an email or message.** Draft only; owner sends.
- **Never create calendar events.** Propose times; owner books.
- **Never change a submission's stage, bind coverage, or mark a submission Declined** unless the owner explicitly asks.
- **Never quote or bind on carrier appetite that hasn't been confirmed** — if appetite fit is uncertain, say so rather than assuming a market will take it.
- **If zero submissions match the filter**, explain why and offer to check what stages are in use — do not fabricate a list.

## Reference

- [reference/submission-fields.md](reference/submission-fields.md) — field names, scoring weights, and appetite-fit defaults
- [reference/csv-format.md](reference/csv-format.md) — expected columns and export instructions for the no-connector fallback
- [reference/examples/happy-path-triage.md](reference/examples/happy-path-triage.md) — worked output for an 8-submission work list including a 12-truck fleet commercial auto submission

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

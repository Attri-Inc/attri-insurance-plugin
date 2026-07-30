---
name: agency-monday-brief
description: >
  Produces a one-page Monday snapshot for an insurance agency owner:
  renewals due this week, open submissions, claims status, and
  receivables — pulled from every connected tool (AMS/CRM, billing,
  calendar, email) and synthesized into the single most important thing to
  act on today. Gracefully scopes to whatever is connected. Use when the
  user asks how the agency is doing, wants a snapshot, a weekly summary, a
  Monday brief, or says anything like "what am I missing" or "catch me up."
---

# Agency Monday Brief

One prompt, one page. Pull live data from every connected tool, synthesize
it into a single scannable brief, and surface the single most important
thing to act on today. Do the work — don't ask the user to help find the data.

## Step 1 — Pull data in parallel

**Dispatch all connector calls in a single parallel batch** — see
`reference/data_sources.md` for the exact tool-to-metric mapping. Do not
pull serially; latency turns a 30-second skill into a painful wait.

Connectors to attempt simultaneously:

- **AMS/CRM** — renewals due in the next 7 and 30 days, open submissions by stage, cross-sell flags
- **Billing/accounting** — receivables aging, past-due premium finance accounts
- **Claims system or claims notes in the AMS** — open claims, status changes, anything reopened
- **Google/Outlook Calendar** — key meetings, underwriter calls, client deliverable deadlines this week
- **Gmail/Outlook Mail** — threads flagged urgent, FNOLs received, carrier non-renewal notices
- **Slack/Teams** — urgent internal signals, threads needing owner attention (if connected)

If a connector errors or returns no data, record it internally and move on.
Never block the brief on a single bad integration.

**AMS/CRM fallback**: if the connector returns an unexpected state (not
connected, sync pending, empty response), mark the relevant section "n/a —
[system] unavailable" and proceed. Do not retry or ask the user to reconnect.

**Mail fallback**: if the mail connector errors, skip the Watch List section
silently and note "Mail unavailable" in the appendix — do not surface an
error mid-brief.

## Step 2 — Compute metrics

Read `reference/thresholds.md` for red/yellow/green cutoffs. Compute:

- **Renewal exposure** — total premium renewing in the next 7 and 30 days, and how much of it is flagged (non-renewal notice, missing loss runs, prior claim activity)
- **Submission pipeline health** — open submissions by stage, weighted by likely win, vs. any monthly new-business target
- **Receivables aging** — past-due premium finance or direct-bill accounts, grouped 0–30 / 31–60 / 61+ days

Assign a 🟢/🟡/🔴 status to each section. If a source returned nothing, mark
the metric "n/a" and note it in the appendix.

## Step 3 — Flag risks proactively

Scan for actionable items. Every risk entry must name a specific record and
a next step — "some renewals need attention" is useless; "Riverside
Logistics, $41,200 premium, renews 8/12, carrier issued non-renewal — needs
re-marketing" is actionable.

- Renewals inside 30 days with a flag (non-renewal notice, missing loss runs, unresolved claim)
- Submissions with no activity in 7+ days, or an effective date already passed
- Claims reopened or escalated in the window
- Past-due receivables > 30 days, named account and amount
- Email threads containing FNOL language, "cancel," "non-renewal," "complaint"

## Step 4 — Compose the output

Use the exact template in `reference/output_template.md`. Include only
sections where real data exists — omit headers for connectors that weren't
available. Adapt depth to context: a casual "how's the agency doing" gets a
fuller report; "quick snapshot before a call" gets a tighter one.

Cross-connector synthesis is where this skill earns its keep. If an email
thread connects to a stalled submission, surface that link in the #1
Priority section.

Writing rules:
- Numbers lead, words follow. Never write "renewals look fine" — write "$142k in premium renewing in the next 30 days, one flagged for re-marketing" and let the owner judge.
- Every number carries a delta vs. the prior period where available.
- Names and dollars, not adjectives. "$41,200 renewal at Riverside Logistics, non-renewal notice" beats "some concerning renewals."
- No filler. If a section has nothing worth reporting, write "No material changes" and move on.

## Step 5 — Export and share (once)

After presenting the brief, offer once:
- "Want me to save this as a file?"
- "Should I post the top items to Slack/Teams?" (only if connected, and only with explicit confirmation — posting requires approval)

If they say yes, do it. If they say no or don't respond, move on — don't ask again.

## Scope variants

The owner may ask for a narrower cut:

- **"Just renewals"** → Renewals section + related risks only
- **"Submission pipeline only"** → Submission pipeline section + stalled-submission risks
- **"Claims status"** → Claims section only
- **"Anything urgent"** → Watch list + all risks, no metric sections
- **"Quick snapshot before a call"** → TL;DR + #1 Priority only

## What not to do

- **Do not ask permission before pulling data.** If the skill was invoked, run it.
- **Do not invent or estimate numbers.** If a source returned nothing, say "n/a" explicitly.
- **Do not skip the delta.** If there's no prior-period baseline, say "(no prior baseline)" rather than omitting the field.
- **Do not surface connector errors mid-brief.** Log them to the appendix.
- **Never bind, re-market, or submit a claim from this skill.** This is a reporting skill — anything actionable routes to submission-triage, policy-comparison, book-of-business, or policyholder-desk, each of which has its own approval gate.

## Approval gates

- **Saving the file is auto.** No approval needed — it's the owner's own drive.
- **Posting to Slack/Teams requires confirmation.** Show the post draft and wait for explicit approval before publishing.
- **Never post if the brief surfaces sensitive numbers** (a large receivables gap, a reopened claim) without explicitly asking the owner first — the channel may have members beyond leadership.

## Reference files

- `reference/data_sources.md` — exact connector tool → metric mapping with fallbacks
- `reference/thresholds.md` — 🟢/🟡/🔴 cutoffs, tunable per agency
- `reference/output_template.md` — exact markdown structure; do not deviate

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

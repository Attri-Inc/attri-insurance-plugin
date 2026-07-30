---
name: retention-watch
version: 0.1.0
description: >
  Spots at-risk policyholder accounts — non-payment, complaints, or no
  recent contact — and suggests a retention action for each. Aggregates
  AMS/CRM notes, billing status, and email sentiment into a ranked
  watch-list. Use when the user asks which accounts are at risk, about
  retention, non-payment, or what's likely to lapse for reasons other than a
  scheduled renewal.
compatibility: "Runs on any subset of: billing/accounting connector, AMS/CRM connector, Mail. Falls back to a CSV export for the AMS/CRM piece — see reference/csv-format.md."
---

# Retention Watch

## Quick start

Ask: *"Which accounts are at risk this month?"*

Claude pulls billing status, AMS/CRM notes, and recent email threads for the
last 30 days, flags accounts showing risk signals, and delivers a ranked
watch-list with a suggested retention action per account.

## Workflow

1. **Set the date window.** Default: last 30 days. If the user specifies a range, use it.

2. **Pull billing/payment status.** Fetch policies with a non-payment flag,
   past-due premium, or a payment plan in default. If the billing source is
   unavailable, skip and note it in the Sources section — do not error.

3. **Pull AMS/CRM notes and recent activity.** Fetch notes and activity
   timestamps for active accounts. Flag any account with no logged contact
   in the window — this is often the earliest and quietest retention
   signal, well before a complaint shows up.

   **If no AMS/CRM connector is live**, ask whether a book or activity
   export is available per [reference/csv-format.md](reference/csv-format.md).
   If neither a connector nor a file is available, skip this source, note it
   in the Sources section, and score remaining accounts on billing + email
   signals only.

4. **Pull email threads.** Search for threads in the window containing
   language suggesting dissatisfaction: `cancel, switch, shopping around,
   too expensive, unhappy, disappointed, non-renewal, considering other
   options`. Extract subject lines and 1–2 sentence excerpts per thread.

5. **Accept pasted feedback (optional).** If the user pastes a review or
   complaint text, include it in the source pool tagged `[Pasted]`.

6. **Score each flagged account.** Combine signals into a risk level:
   - **High** — non-payment/past-due AND (no recent contact OR complaint language)
   - **Medium** — one clear risk signal (non-payment alone, or complaint alone, or 60+ days no contact)
   - **Low** — minor signal only (e.g. one unanswered email, otherwise healthy)

   Do not assume every non-payment is a retention risk — a payment plan
   in good standing with one missed cycle is different from a pattern of
   defaults. Check history before scoring high.

7. **Build the ranked watch-list.** Sort by risk level, then premium size.
   For each account: named insured, line(s) of business, premium, risk
   level, the specific signal(s) found (with a verbatim excerpt where
   applicable), and **one suggested retention action** — e.g. "call before
   the payment plan lapses," "address the pricing complaint directly," "a
   simple check-in call, since there's been no contact in 90 days."

8. **Offer outreach drafts.** Ask: *"Draft outreach for any of these?"* If
   yes, draft one message per selected account, matching agency tone. Show
   draft; do not send.

9. **Deliver the report.** Structure the output with these sections in order:
   - **Header** — "Retention Watch" and the date range.
   - **Sources pulled** — bullet list with counts per source, noting any
     source that was unavailable and skipped.
   - **Watch-list** — each account with risk level, signal(s), verbatim
     evidence where applicable, and the suggested action.
   - **Top 3 to act on this week** — the three highest-risk, highest-premium
     accounts, each with a one-line reason.

## Approval gates

- **This skill is read-only for scoring and reporting** — no records are modified.
- **Never send outreach emails.** Draft only; owner sends.
- **Never mark an account for cancellation or change billing status.**
- **Do not score an account High risk on a single ambiguous signal** — require at least two signals, or one severe signal (explicit cancellation language), before flagging High.

## Reference

- [reference/gotchas.md](reference/gotchas.md) — payment-plan false positives, verbatim quote requirement, no-contact vs. genuinely satisfied accounts
- [reference/examples/example-report.md](reference/examples/example-report.md) — full worked example output
- [reference/csv-format.md](reference/csv-format.md) — expected columns for the no-connector AMS/CRM fallback

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

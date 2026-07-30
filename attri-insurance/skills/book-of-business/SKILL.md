---
name: book-of-business
description: >
  Keeps the agency's book clean: flags upcoming renewals, lapsed or
  non-renewed policies, and cross-sell openings, and produces a renewal
  pipeline view. Logs renewal outreach and stale-record flags to the
  AMS/CRM — never renews, cancels, or re-markets a policy without approval.
  Use when the user asks about renewals, wants to clean up the book, asks
  what's lapsing, or wants to see cross-sell opportunities. Falls back to a
  CSV/spreadsheet export of the book when no AMS connector is live.
compatibility: "Requires an AMS/CRM connector, or a CSV export of the active book (fallback). Log-to-AMS step (7) requires a live connector — skipped if running on CSV. See reference/csv-format.md."
---

# Book of Business

## Quick start

Pull the active book from the AMS/CRM, flag what needs attention, and surface
a renewal pipeline view. Logs outreach notes and stale-record flags — never
renews, cancels, quotes a cross-sell, or edits policy data without approval.

```
User: "what's coming up for renewal?"
→ Pull active policies with renewal dates in the requested window
→ Flag: renewals due, lapsed/non-renewed policies, cross-sell openings
→ Return a renewal pipeline view sorted by date and premium
→ Offer to draft renewal outreach and log notes to the AMS/CRM
```

## Workflow

1. **Identify intent.** Decide which of three paths applies:
   - **Renewal pipeline** — "what's renewing," "upcoming renewals," "renewal pipeline"
   - **Lapsed/non-renewed check** — "what's lapsed," "who didn't renew," "cancellations"
   - **Cross-sell scan** — "cross-sell opportunities," "who should we bundle," "account rounding"

   If the request is ambiguous ("clean up the book"), ask which of the three, or offer all three as a combined view.

2. **Pull the book.** Fetch active policies from the AMS/CRM with renewal
   date, line of business, premium, carrier, and named insured. Use the
   field list in [reference/book-fields.md](reference/book-fields.md).

   **If no AMS/CRM connector is live**, ask whether a book/policy export is
   available: *"No AMS connector is connected — do you have a book export
   (CSV or spreadsheet) I can work from?"* If yes, read it per
   [reference/csv-format.md](reference/csv-format.md) and proceed. Note in
   the output that this is a point-in-time snapshot, and skip step 7
   (logging to AMS) since there's no live system to write to. If neither a
   connector nor a file is available, stop: *"No book data available —
   connect your AMS/CRM or upload a book export to run this."*

3. **Renewal pipeline path.** Filter to policies renewing in the requested
   window (default: next 60 days if unspecified). Sort by renewal date, then
   premium. For each: named insured, line of business, premium, renewal
   date, and a flag for anything needing attention before renewal (missing
   updated loss runs, carrier issuing a non-renewal notice, prior claim
   activity that could affect terms).

4. **Lapsed/non-renewed path.** Identify policies that lapsed, cancelled, or
   were non-renewed in the lookback window (default: last 90 days). For
   each: named insured, reason if known (non-payment, carrier non-renewal,
   client-initiated), last premium, and whether a win-back outreach makes
   sense. Do not assume a lapse was intentional — flag ones that look like
   they may have fallen through the cracks (no documented reason, no
   outreach logged) separately from clear client-initiated cancellations.

5. **Cross-sell scan.** For active policyholders, check which common lines
   they don't currently hold with the agency (e.g. a commercial auto client
   with no umbrella, a homeowners client with no auto). Use
   [reference/cross-sell-patterns.md](reference/cross-sell-patterns.md) for
   common bundle patterns by account type. Flag account-rounding
   opportunities, not just any missing line — prioritize by premium
   potential and account tenure.

6. **Offer outreach drafts.** Ask: *"Draft renewal outreach or win-back
   emails for any of these?"* If yes, draft one message per selected
   policyholder, matching agency tone. Show draft; do not send.

7. **Log to AMS/CRM.** After the owner reviews, offer to log a note (renewal
   flagged, outreach sent, cross-sell identified) on the relevant record.
   Announce before writing.

## Approval gates

- **Never renew, re-market, cancel, or bind a policy.** This skill surfaces information and drafts outreach only.
- **Never send outreach emails.** Draft only; owner sends.
- **Never delete or overwrite policy records.** Flag stale or conflicting data; let the owner decide.
- **Never assume a lapse was client-initiated** without documentation — flag ambiguous lapses separately so they don't get missed.
- **Announce before logging any note to the AMS/CRM.**

## Reference

- [reference/book-fields.md](reference/book-fields.md) — AMS/CRM field names used for renewal, lapse, and cross-sell checks
- [reference/cross-sell-patterns.md](reference/cross-sell-patterns.md) — common line-of-business bundle patterns by account type
- [reference/csv-format.md](reference/csv-format.md) — expected columns and export instructions for the no-connector fallback
- [reference/examples/renewal-pipeline-view.md](reference/examples/renewal-pipeline-view.md) — worked example: 60-day renewal pipeline with a flagged non-renewal

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

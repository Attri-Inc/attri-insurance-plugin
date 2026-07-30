# Attri for Insurance Agencies

*An Attri plugin — built on Anthropic's open-source [Small Business plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/small-business), customized for insurance agencies. Learn more at [attri.ai](https://attri.ai).*

Nine skills that handle the day-to-day work of running an insurance agency —
scoring submissions, comparing policies, handling policyholder requests,
tracking the renewal book, watching for at-risk accounts, staying on top of
receivables and month-end close, and a one-page Monday brief that pulls it
all together.

**Every skill keeps an audit trail — what it read, what it changed — and
none of them send, file, bind, or submit anything without your explicit
approval first.**

## Skills

| Skill | What it does |
|---|---|
| `submission-triage` | Scores open submissions by premium, appetite fit, and win-likelihood; flags what's missing to quote |
| `policy-comparison` | Compares policies/quotes across carriers or renewal terms; flags coverage gaps and changed limits |
| `policyholder-desk` | Handles FNOL intake, certificate of insurance requests, and endorsement questions |
| `book-of-business` | Flags upcoming renewals, lapsed policies, and cross-sell openings |
| `retention-watch` | Spots at-risk accounts (non-payment, complaints, no recent contact) and suggests a retention action |
| `agency-monday-brief` | One-page Monday snapshot: renewals due, open submissions, claims status, receivables |
| `cash-flow-snapshot` | 30/60/90-day cash flow forecast from accounting and billing data |
| `invoice-chase` | Drafts overdue-premium reminder emails, matched to policyholder payment history |
| `month-end-prep` | Reconciles accounting against settlements, flags gaps, writes a plain-English close narrative |

## Setup

Connect the tools your agency already uses — QuickBooks (or your accounting
system), your CRM/AMS, Gmail/Outlook, Google/Outlook Calendar, and DocuSign
if you use it for policy documents. Each skill gracefully scopes to whatever
is connected — you don't need every connector for the plugin to be useful.

**Note:** these skills reference a generic AMS/CRM. If your agency runs
Applied Epic, HawkSoft, EZLynx, or another AMS, the field names in each
skill's `reference/` folder may need light adjustment to match your system's
actual schema — ask Claude to walk through this with you on first use.

## Governance

This is Attri's differentiator: nothing in this plugin sends an email,
issues a certificate, submits an FNOL, binds coverage, or posts to a
channel without you reviewing and approving it first. Every skill's
approval gates are documented at the bottom of its `SKILL.md`.

## License

Apache License 2.0 — inherited from Anthropic's open-source Small Business
plugin. See `LICENSE`.

---
Built by Attri AI · [attri.ai](https://attri.ai)

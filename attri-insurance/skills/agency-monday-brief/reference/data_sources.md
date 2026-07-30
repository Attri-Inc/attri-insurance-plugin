# Data Sources — agency-monday-brief

Connector-to-metric mapping. Map generic names below to the agency's actual
connected systems (Applied Epic, HawkSoft, EZLynx, QuickBooks, Gmail/Outlook,
Google/Outlook Calendar, Slack/Teams).

| Connector | Metric pulled | Fallback if unavailable |
|---|---|---|
| AMS/CRM | Renewals due (7d, 30d), submission pipeline by stage, claims status | If not connected, ask for a book/submissions CSV export (see the csv-format.md in submission-triage and book-of-business). If neither exists, "n/a — AMS unavailable," proceed |
| Billing/accounting | Receivables aging, past-due premium finance accounts | "n/a — billing unavailable," proceed |
| Calendar | This week's underwriter calls, client deliverable deadlines | Skip Week Ahead section, note in appendix |
| Mail | FNOL mentions, non-renewal notices, urgent threads | Skip Watch List silently, note "Mail unavailable" in appendix |
| Slack/Teams | Urgent internal signals (if connected) | Skip silently if not connected |

Dispatch all available connectors in parallel — do not pull serially.

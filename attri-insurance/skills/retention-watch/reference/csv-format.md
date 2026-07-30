# CSV Fallback Format — retention-watch

Covers the AMS/CRM piece only (notes, last-contact date, account tenure).
Billing and email signals still need their own connectors — this fallback
doesn't substitute for those.

## Expected columns

| Expected data | Common column name variations |
|---|---|
| Named insured | `Insured Name`, `Client` |
| Last contact date | `Last Activity`, `Last Contact`, `Last Note Date` |
| Account tenure | `Client Since`, `Start Date` |
| Line(s) of business | `LOB`, `Lines` |
| Annual premium | `Premium` |

## Limitations vs. a live connector

- No live note search — the export is a point-in-time snapshot. Say so.
- If last-contact date isn't tracked in the export, skip the no-contact
  signal for this run rather than guessing — note it as "not available
  from this export" in the Sources section.

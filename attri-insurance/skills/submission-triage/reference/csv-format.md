# CSV Fallback Format — submission-triage

Use when no AMS/CRM connector is live. Most AMS platforms (Applied Epic,
HawkSoft, EZLynx, Vertafore) can export an open-submissions or new-business
report to CSV or Excel — ask the agency to pull that report and upload it.

## Expected columns

The file doesn't need exact column names — Claude will map reasonable
variations (e.g. "Insured Name" or "Client" both map to `named_insured`) —
but should contain, at minimum:

| Expected data | Common column name variations |
|---|---|
| Named insured | `Insured Name`, `Client`, `Account Name` |
| Line of business | `LOB`, `Line`, `Coverage Type` |
| Target/quoted premium | `Premium`, `Target Premium`, `Quoted Premium` |
| Effective date | `Eff Date`, `Effective Date`, `Policy Start` |
| Submission stage | `Status`, `Stage` |
| Carrier(s) being marketed | `Carrier`, `Markets`, `Submitted To` |
| Missing items (if tracked) | `Missing Docs`, `Outstanding Items`, `Notes` |

## If a column is missing

- **No stage/status column**: assume all rows in the export are open (the
  agency likely filtered before exporting) — confirm this assumption with
  the user rather than silently guessing.
- **No missing-items column**: skip that part of the output for this run
  rather than fabricating what might be missing — say "not tracked in this
  export" instead of inventing a gap.
- **No carrier column**: appetite-fit scoring degrades to premium + urgency
  only — note this in the output.

## What changes vs. a live connector

- The data is a point-in-time snapshot — flag this in the output so the
  owner knows a submission may have already moved since the export was
  pulled.
- Nothing changes about the approval gates — this is a read/score/report
  workflow either way, so the CSV fallback doesn't reduce any of the
  guardrails.

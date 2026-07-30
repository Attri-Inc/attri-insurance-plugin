# CSV Fallback Format — book-of-business

Use when no AMS/CRM connector is live. Most AMS platforms can export a full
book-of-business or policy list report — ask the agency to pull that and
upload it.

## Expected columns

| Expected data | Common column name variations |
|---|---|
| Named insured | `Insured Name`, `Client`, `Account Name` |
| Policy number | `Policy #`, `Policy Number` |
| Line of business | `LOB`, `Line`, `Coverage Type` |
| Carrier | `Carrier`, `Company` |
| Renewal/expiration date | `Renewal Date`, `Exp Date`, `Expiration` |
| Annual premium | `Premium`, `Annual Premium` |
| Policy status | `Status` (Active, Lapsed, Non-Renewed, Cancelled) |
| Last claim date (if tracked) | `Last Claim`, `Claim Date` |

## If a column is missing

- **No status column**: assume all rows are active unless a renewal/expiration
  date has already passed — flag this assumption to the user.
- **No distinction between "Non-Renewed" and "Cancelled"**: ask the user how
  their AMS codes carrier-initiated vs. client-initiated exits before
  assuming — these need different follow-up.
- **No claim history**: skip the claim-activity renewal flag for this run
  rather than guessing.

## What changes vs. a live connector

- Treat the data as a point-in-time snapshot — say so in the output.
- **Step 7 (log to AMS) is skipped entirely** when running on CSV — there's
  no live system to write a note to. Say this explicitly rather than
  silently dropping the step.
- All other approval gates apply exactly as they do with a live connector.

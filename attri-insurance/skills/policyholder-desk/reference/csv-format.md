# CSV Fallback Format — policyholder-desk

Use for policy lookup when no AMS/CRM connector is live. This is read-only —
enough to answer "what does this policyholder's policy say," not to log
anything back.

## Expected columns

| Expected data | Common column name variations |
|---|---|
| Named insured | `Insured Name`, `Client` |
| Policy number | `Policy #`, `Policy Number` |
| Carrier | `Carrier`, `Company` |
| Line of business | `LOB`, `Line` |
| Coverage limits | `Limits`, `Coverage Limits` |
| Effective / expiration dates | `Eff Date`, `Exp Date` |
| Endorsements (if tracked) | `Endorsements`, `Forms` |

## Limitations vs. a live connector

- No note-logging — step 7 (log to AMS/CRM) is skipped and should be stated
  explicitly rather than silently dropped.
- If the export doesn't include endorsement-level detail, say so rather
  than guessing at what's covered — route the policyholder's question to a
  producer if the file doesn't have enough detail to answer confidently.

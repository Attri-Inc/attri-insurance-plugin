# Book Fields — book-of-business

Field names used for renewal, lapse, and cross-sell checks. Map to the
connected AMS/CRM's actual schema.

| Field | Used for |
|---|---|
| `named_insured` | Display |
| `policy_number` | Display + lookup |
| `line_of_business` | Cross-sell scan, filtering |
| `carrier` | Display, non-renewal tracking |
| `renewal_date` / `expiration_date` | Renewal pipeline filtering and sorting |
| `annual_premium` | Sorting, cross-sell prioritization |
| `policy_status` | Filter (Active, Lapsed, Non-Renewed, Cancelled) |
| `last_claim_date` | Renewal-attention flag |
| `account_tenure` | Cross-sell prioritization (longer tenure = higher trust for bundling) |
| `outreach_logged` | Used to separate documented cancellations from unexplained lapses |

## Defaults

- Renewal pipeline window: next 60 days unless the user specifies otherwise.
- Lapsed/non-renewed lookback: last 90 days unless specified.
- If the AMS/CRM doesn't distinguish "Non-Renewed" (carrier-initiated) from
  "Cancelled" (client-initiated) as separate statuses, ask the user how
  their system codes this before assuming.

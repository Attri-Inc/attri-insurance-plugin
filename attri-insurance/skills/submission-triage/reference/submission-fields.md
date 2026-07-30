# Submission Fields — submission-triage

Field names, scoring weights, and appetite-fit defaults. Field names below use
generic AMS/CRM terms — map to the connected system's actual schema
(Applied Epic, HawkSoft, EZLynx, or a CRM standing in for an AMS).

---

## Fields to pull

| Field | Used for |
|---|---|
| `named_insured` | Display |
| `line_of_business` | Appetite fit + display (e.g. Commercial Auto, GL, Property, BOP, Workers Comp) |
| `target_premium` | Premium-size scoring |
| `effective_date` | Urgency |
| `submission_stage` | Filter (keep New, In Review) |
| `carrier_marketed` | Appetite fit — carrier(s) submission is out to |
| `incumbent_carrier` | Win-likelihood — rewrite vs. new business |
| `broker_of_record` | Win-likelihood — exclusivity signal |
| `missing_items` | What's-missing flag (loss runs, SOV, signed application, etc.) |
| `class_code` | Appetite fit — industry/class match |
| `account_size` | Appetite fit — revenue, payroll, or unit count depending on line |

---

## Scoring model (0–100 composite)

Four dimensions, each 0–25. Sum for composite.

**Premium size (0–25):** scaled against the agency's typical book, not an
absolute dollar figure — ask the owner what "large" means for their book if
unclear.

**Carrier appetite fit (0–25):** full points if the line, class, and account
size clearly match a carrier's known appetite; partial if it's a stretch;
low if new markets need to be sourced.

**Win-likelihood (0–25):** full points for a rewrite with broker-of-record
already secured; lower for new business competing against an incumbent with
no relationship.

**Urgency (0–25):** full points inside 15 days of effective date; scale down
linearly beyond that. An expired effective date should be flagged separately,
not just scored low.

## Appetite defaults

Default assumption: standard commercial lines (BOP, GL, Commercial Auto,
Workers Comp) in the $5K–$50K premium band. If the agency writes larger
programs, surplus lines, or personal lines predominantly, ask before applying
these defaults — the scoring weights should reflect the actual book.

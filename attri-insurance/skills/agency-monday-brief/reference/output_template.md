# Output Template — agency-monday-brief

This is the exact structure every brief must follow. Do not reorder
sections. Omit a section only if its connector returned no data — never
leave an empty header.

Variables in `{{double braces}}` are placeholders. Arrow convention: ▲ up,
▼ down, ▬ flat (<1% change). Always show the delta value after the arrow.

---

```markdown
# Agency Monday Brief — {{Day, Month Date, Year}}

**Overall: {{🟢|🟡|🔴}} {{one-line status, e.g. "Renewals on track, one non-renewal notice needs re-marketing."}}**

## TL;DR

- {{Most important number-backed fact, e.g. "$142k in premium renewing in the next 30 days — one flagged for re-marketing."}}
- {{Second most important, e.g. "Riverside Logistics, $41,200 Commercial Auto, non-renewal notice from carrier — needs a submission-triage pass this week."}}
- {{Third, e.g. "Submission pipeline at 72% of monthly target."}}

---

## 📋 Renewals — {{🟢|🟡|🔴}}

- **Due in 7 days**: {{N}} policies, ${{PREMIUM}} total premium
- **Due in 30 days**: {{N}} policies, ${{PREMIUM}} total premium
- **Flagged**: {{N}} — {{list named insured + reason, e.g. "Riverside Logistics — carrier non-renewal notice"}}

## 📥 Submission Pipeline — {{🟢|🟡|🔴}}

- **Open submissions**: {{N}}, ${{PREMIUM}} total target premium
- **Stalled (7+ days no activity)**: {{N}}
- **New this week**: {{N}}

## 🩹 Claims — {{🟢|🟡|🔴}}

- **Open claims**: {{N}}
- **Reopened/escalated this week**: {{N}} — {{name + reason if any}}
- **No status update in 14+ days**: {{N}}

## 💰 Receivables — {{🟢|🟡|🔴}}

- **Total outstanding**: ${{AR_TOTAL}}
- **0–30 days**: ${{AR_0_30}}
- **31–60 days**: ${{AR_31_60}}
- **61+ days**: ${{AR_61_PLUS}} {{🟡/🔴 if nonzero}}

## 📅 Week Ahead

- {{Tue 10am}} — {{Underwriter call: Harborline Manufacturing renewal}}
- {{Thu EOD}} — {{COI due to GC for Cedar Point Retail}}
- ...

## 👀 Watch List

- {{FNOL received, needs same-day carrier submission}}
- {{Email thread flagged non-renewal or complaint language}}

## Three things that need you today

1. {{Highest-leverage action with one-line why}}
2. {{...}}
3. {{...}}

---
*Appendix: {{note any unavailable connectors, e.g. "Mail unavailable — Watch List may be incomplete."}}*
```

---
name: policy-comparison
description: >
  Compares policy documents and endorsements across carriers or renewal terms,
  flags coverage gaps, changed limits, and non-standard exclusions, and
  produces a plain-English summary of what changed. Reads policies from local
  files, Gmail attachments, or DocuSign envelopes. Use when the user says
  "compare these quotes," "what changed on renewal," "check coverage gaps,"
  "flag any exclusions," or uploads/forwards a policy, quote, or endorsement.
---

# Policy Comparison

## Quick start

Attach two or more policy/quote documents, forward the email containing them,
or paste the text directly.

```
User: "Compare these two commercial property quotes and flag anything I should push on."
→ Skill reads both documents, identifies named insured / carrier / term,
  compares coverages line by line, returns a severity-tiered summary of
  gaps and changes, and exports a side-by-side comparison DOCX.
```

## Workflow

1. **Get the documents** — Pull from one of three sources, in order of preference:
   - **Gmail**: search recent emails with policy or quote attachments (see `reference/gmail-fetch.md`)
   - **DocuSign**: fetch the envelope by ID (see `reference/docusign-fetch.md`)
   - **Local file or paste**: read the PDF (chunked via `pages` parameter for long policies) or DOCX. If pasted directly, work with what's provided.

   Read the full document, including endorsements and exclusion schedules —
   the meaningful changes are frequently buried there, not in the declarations page.

2. **Identify the basics** — named insured, carrier(s), line of business,
   policy term / effective dates, and whether this is a renewal comparison
   (same account, prior term vs. new term) or a multi-carrier quote comparison
   (same account, competing carriers).

3. **Compare across coverage categories** — work line by line. Categories
   below are ordered by typical materiality; use judgment for context.

   **Limits and sublimits**
   - Per-occurrence and aggregate limits — flag any decrease
   - Sublimits on named perils (water damage, cyber, EPLI, etc.) — these are frequently reduced quietly on renewal
   - Deductible / retention changes, including any move to a percentage deductible (e.g. wind/hail)

   **Coverage gaps and exclusions**
   - New or broadened exclusions vs. prior term or the competing quote
   - Named-peril vs. all-risk / open-peril form changes
   - Additional insured and waiver-of-subrogation requirements — present or dropped
   - Occurrence vs. claims-made trigger, and retroactive date on claims-made forms

   **Endorsements**
   - Endorsements present on one document and missing on the other
   - Manuscript endorsements that alter standard ISO/AAIS language — flag for a plain-English read

   **Pricing and terms**
   - Premium change and what's driving it (rate, exposure, schedule credit/debit changes)
   - Payment plan and audit provisions (for audited lines like Workers Comp and GL)
   - Cancellation and non-renewal notice periods

4. **Present flagged summary** — organize by severity:

   **🔴 Coverage gaps (flag before binding)** — quote the exact language, explain the exposure it leaves uncovered, and what to ask the carrier or wholesaler to fix.

   **🟡 Changed terms (worth a conversation with the client)** — quote the change, explain the practical effect on a claim.

   **🟢 Key terms to note (awareness only)** — limits, deductibles, renewal date, additional insureds, payment plan.

   **📋 Plain-English summary** — what's covered, for how much, under what conditions, and how this term compares to the prior one or the competing quote.

5. **Export comparison DOCX** — offer to export a side-by-side comparison
   document using the `docx` skill. Ask: *"Want a side-by-side comparison
   DOCX you can attach to the client's renewal email?"*

## Approval gates

- Never characterize this as legal or coverage advice — always recommend the licensed producer or agency principal confirm before advising the client on binding decisions.
- Quote actual policy language, not paraphrases — producers need exact wording for carrier and client conversations.
- Flag what's missing, not just what's there. A renewal silent on a sublimit that existed last term is often more important than a clause that's merely changed.
- Do not flag standard ISO/AAIS boilerplate that hasn't changed. The producer wants signal, not a full policy restatement.
- Never send the comparison document to the client or carrier without explicit user confirmation.

## Reference

- `reference/gotchas.md` — edge cases in policy comparison (manuscript endorsements, claims-made retro dates, admitted vs. non-admitted carriers)
- `reference/docusign-fetch.md` — pulling envelopes from DocuSign
- `reference/gmail-fetch.md` — finding policy/quote attachments in Gmail
- `reference/examples/flagged-summary-property.md` — worked example: commercial property renewal comparison

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

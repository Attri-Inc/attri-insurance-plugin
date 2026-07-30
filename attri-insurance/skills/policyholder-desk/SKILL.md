---
name: policyholder-desk
description: >
  Reads a forwarded policyholder email or ticket, identifies whether it's a
  first notice of loss (FNOL), a certificate of insurance (COI) request, or
  an endorsement question, pulls policy details from the AMS/CRM, drafts a
  tone-matched reply in the agency's voice, and routes anything requiring
  carrier action. Use when the user says "answer this policyholder," "draft a
  response," "I need a COI," "file a claim," or forwards a client email.
compatibility: "Requires AMS/CRM connector, Mail. Falls back to a policy-list CSV export for lookup (read-only) if no AMS connector is live — see reference/csv-format.md. Optional: claims portal, e-signature."
---

# Policyholder Desk

## Quick start

Forward or paste a policyholder message — Claude identifies the request type,
pulls the policy from the AMS/CRM, and drafts a reply in the agency's voice.
Anything that touches the carrier (filing a claim, issuing a certificate)
is staged for the owner's approval before it goes out.

```
User: "answer this policyholder" [forwards email]
→ Identify request type: FNOL, COI request, or endorsement question
→ Pull policy record from AMS/CRM
→ Draft reply in the agency's voice
→ Owner approves draft → send or stage
→ If claim or COI action needed: approval prompt → owner confirms → route
```

## Workflow

1. **Read the policyholder message.** Accept a forwarded email thread or
   pasted text. Extract: policyholder name, policy number (if present), and
   the core request. Classify it as one of three types:
   - **FNOL (first notice of loss)** — anything describing an incident, damage, injury, or accident, even if the word "claim" isn't used
   - **Certificate of insurance (COI) request** — a request for proof of coverage, usually for a landlord, lender, or general contractor
   - **Endorsement question** — a request to add/remove a vehicle, driver, location, or coverage, or a general question about what's covered

   If the message doesn't clearly fit one type, ask the owner to confirm before proceeding — do not guess on an FNOL, since timing matters for claim reporting.

2. **Pull the policy record.** Search the AMS/CRM by policyholder name or
   policy number. Capture: policy number, carrier, line of business,
   coverage limits, effective/expiration dates, and named insureds.

   **If no AMS/CRM connector is live**, ask whether a policy-list export is
   available: *"No AMS connector is connected — do you have a policy list I
   can search instead?"* If yes, read it per
   [reference/csv-format.md](reference/csv-format.md) and search it the same
   way. A CSV is read-only here — it's fine for looking up a policy, but
   the step 7 AMS/CRM note-logging step is skipped since there's nothing
   live to write to.

   If neither a connector nor a file is available, note it in the draft and
   continue. If no policy matches (with either source), flag it — do not
   guess at a match.

3. **Handle by request type:**

   **FNOL** — Capture what's known: date/time of loss, location, description,
   any injuries, other parties involved, photos if attached. Do not assess
   coverage or estimate a payout. Draft an acknowledgment to the policyholder
   confirming receipt and next steps, and stage the FNOL details for the
   owner to submit to the carrier's claims system. Flag urgency if there's
   any indication of injury, third-party involvement, or ongoing loss (active
   water leak, fire).

   **COI request** — Identify who's requesting the certificate and why
   (landlord, lender, general contractor requiring additional insured
   status). Check whether the requester needs to be added as an additional
   insured or certificate holder only — these are different asks and
   confusing them creates E&O exposure. Draft the certificate request for
   the owner to issue through the AMS or carrier portal. Do not generate or
   sign a COI directly — most agencies require it to be issued through their
   AMS's certificate module for audit trail purposes.

   **Endorsement question** — Answer what's clearly stated in the policy
   record. If the question requires a coverage interpretation beyond what's
   in the file, say so and route to the producer or CSR rather than
   guessing. Draft a reply; do not bind or request an endorsement change
   without the owner confirming.

4. **Draft the reply.** Write in the agency's voice. Adjust tone to the
   request type — FNOL replies should be calm and reassuring; COI replies
   factual and quick; endorsement replies clear about what's confirmed vs.
   what needs follow-up. Flag any data gaps inline with a bracketed note
   (e.g. *[Note: no policy record found — verify policy number before
   sending]*). See [reference/examples/respond-fnol.md](reference/examples/respond-fnol.md)
   for a worked example and [reference/gotchas.md](reference/gotchas.md) for
   common pitfalls.

5. **Approval gate — owner reviews the draft.** Present the full draft. Do
   not send or stage it until the owner approves. The owner may edit freely.

6. **Approval gate — carrier-facing action.** If the request requires
   submitting an FNOL to the carrier, issuing a COI, or requesting an
   endorsement, surface a dedicated confirmation prompt after the draft is
   approved:

   > *"Submit FNOL for [policyholder] ([policy number]) to [carrier] claims? Reply Y to proceed."*

   Wait for explicit confirmation. If the reply is anything other than a
   clear yes, stop and ask what they'd like to do instead.

7. **Send or stage the reply.** After draft approval, ask: send via email
   now, or save as a draft? Execute their choice. Then log the interaction
   as a note on the policyholder's AMS/CRM record.

8. **Report.** One short paragraph: reply sent or staged, carrier action
   taken or staged for the owner, AMS/CRM note logged.

## Approval gates

- **Never submit an FNOL to a carrier without explicit owner confirmation** — always show policyholder, policy number, carrier, and loss details before executing.
- **Never issue a certificate of insurance directly.** Stage the request for the owner to issue through the AMS or carrier portal — this preserves the agency's audit trail and E&O protection.
- **Never send the reply without owner review.** Always present the full draft first.
- **Never offer a coverage opinion beyond what's documented in the policy file.** Route ambiguous coverage questions to a licensed producer.
- **Never fabricate policy details.** If the AMS/CRM has no record, say so inline in the draft — do not invent coverage or limits.

## Reference

- [reference/gotchas.md](reference/gotchas.md) — patterns for FNOL urgency triage, COI vs. additional insured confusion, and ambiguous coverage questions
- [reference/examples/respond-fnol.md](reference/examples/respond-fnol.md) — worked example: FNOL intake with policy found
- [reference/csv-format.md](reference/csv-format.md) — expected columns for the no-connector policy lookup fallback

---
*Part of Attri for Insurance Agencies · [attri.ai](https://attri.ai)*

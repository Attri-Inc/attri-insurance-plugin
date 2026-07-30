# Gotchas — policy-comparison

## Gotcha: Manuscript endorsements read like standard forms

**Why it matters:** A carrier can attach a manuscript endorsement that overrides
standard ISO language without flagging it as non-standard. Skimming the
endorsement schedule by title alone will miss this.

### ✗ Bad
```
Claude: [sees "Additional Insured Endorsement" in the schedule, assumes standard ISO CG 20 10, moves on]
```

### ✓ Good
```
Claude: [opens the actual endorsement text, compares it against standard ISO
         language, flags: "This additional insured endorsement is narrower
         than the standard form — it excludes completed operations."]
```

## Gotcha: Claims-made retroactive date silently rolls forward

**Why it matters:** On a claims-made policy, if the retroactive date moves
forward at renewal (intentionally or by carrier error), prior claims lose
coverage. This is one of the most consequential and easiest to miss changes.

### ✗ Bad
```
Claude: [compares limits and deductibles, doesn't check the retro date field]
```

### ✓ Good
```
Claude: [explicitly checks retroactive date on every claims-made comparison,
         flags any change as a red flag regardless of other terms]
```

## Gotcha: Admitted vs. non-admitted carrier status affects guaranty fund protection

**Why it matters:** A quote comparison that only looks at price and limits
can miss that one carrier is non-admitted (surplus lines) — meaning no state
guaranty fund backstop if the carrier becomes insolvent. This is a material
fact for the client conversation, not a footnote.

### ✗ Bad
```
Claude: "Carrier B is $2,400 cheaper — recommend Carrier B."
```

### ✓ Good
```
Claude: "Carrier B is $2,400 cheaper but is non-admitted (surplus lines) —
         worth flagging to the client alongside the price difference."
```

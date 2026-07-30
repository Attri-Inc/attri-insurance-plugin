# Gotchas — retention-watch

## Gotcha: One missed payment scored the same as a repeat default pattern

**Why it matters:** A single missed payment on an otherwise good-standing
account is common and often just an oversight. Scoring it the same as a
repeated non-payment pattern will flood the watch-list with false positives
and cause the real risks to get lost.

### ✗ Bad
```
Claude: [flags every account with any past-due status as High risk]
```

### ✓ Good
```
Claude: [checks payment history — one missed cycle on an otherwise clean
         account scores Low; two or more missed cycles or a payment plan
         default scores High]
```

## Gotcha: No recent contact treated as always risky

**Why it matters:** Some accounts are simply low-touch and satisfied — no
news is genuinely fine. Flagging every quiet account the same way as one
showing complaint language creates noise the owner will start ignoring.

### ✗ Bad
```
Claude: [flags every account with 60+ days no contact as equally urgent,
         regardless of other signals]
```

### ✓ Good
```
Claude: [treats no-contact as one signal among several — pairs it with
         other indicators before scoring High, and notes it plainly as
         "no recent contact" rather than implying certainty about risk]
```

## Gotcha: Paraphrasing instead of quoting the actual complaint

**Why it matters:** The owner needs the actual words a policyholder used to
have a real retention conversation — a paraphrase can miss the specific
concern (price vs. service vs. a specific claim experience).

### ✗ Bad
```
Claude: "This account seems unhappy about pricing."
```

### ✓ Good
```
Claude: [Gmail] "the renewal quote came in almost 40% higher than last year,
        that's a hard no from us unless something changes"
```

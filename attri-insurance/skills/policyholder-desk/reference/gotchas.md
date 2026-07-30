# Gotchas — policyholder-desk

## Gotcha: Treating an FNOL like a general complaint

**Why it matters:** Some policyholders describe a loss without using the
word "claim" — "my basement flooded last night" is an FNOL, not a service
question. Timing matters for claim reporting requirements in most policies.

### ✗ Bad
```
Policyholder: "Hey, just a heads up my basement flooded last night, what a mess."
Claude: [drafts a sympathetic reply, no FNOL flagged]
```

### ✓ Good
```
Policyholder: "Hey, just a heads up my basement flooded last night, what a mess."
Claude: [classifies as FNOL, captures date/location/description, flags for
         same-day carrier submission given active water damage]
```

## Gotcha: Certificate holder vs. additional insured confusion

**Why it matters:** A certificate holder just receives proof of coverage. An
additional insured actually gets added to the policy's coverage. Issuing the
wrong one creates a coverage gap the requesting party is relying on — and
E&O exposure for the agency.

### ✗ Bad
```
Claude: [drafts a standard certificate without checking whether the general
         contractor needs additional insured status per the contract]
```

### ✓ Good
```
Claude: "The GC's contract typically requires additional insured status, not
         just a certificate holder listing — worth confirming with the
         policyholder before issuing, since these aren't the same thing."
```

## Gotcha: Answering a coverage question from memory instead of the file

**Why it matters:** Coverage varies by endorsement and carrier form. A
general answer about what a "typical" policy covers can be wrong for this
specific policyholder's actual endorsements.

### ✗ Bad
```
Policyholder: "Is water damage from a burst pipe covered?"
Claude: "Yes, that's typically covered under a standard homeowners policy."
```

### ✓ Good
```
Policyholder: "Is water damage from a burst pipe covered?"
Claude: [checks the actual policy file for water damage endorsements/exclusions]
        "Based on the file, sudden and accidental discharge is covered, but
         there's a $2,500 sublimit — worth routing to your producer to
         confirm details before responding."
```

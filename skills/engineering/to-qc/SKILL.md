---
name: to-qc
description: "Turn a release candidate into a QC plan. Use before verifying a release, or when the user asks for a QC document."
disable-model-invocation: true
---

Write `QC_<date>.md` — the plan the `run-qc` skill executes.

It is written for the agent that will run it, not for a person. Every item states one action and
one result comparable enough that two readers cannot disagree about it.

Explore, then interview, then write.

## Explore

Work out what changed between what runs in production and the candidate. Date production by
probing it — a ticket that is Done is merged, not deployed.

Past the commits, what decides the plan: migrations, dependency and image changes, new
environment variables, new outbound destinations, new scopes, new scheduled work, and anything
that writes into a system you do not own.

## Interview

Use the `grilling` skill. Look the facts up yourself; these are the user's to answer:

- What worries them most in this release.
- What must not be touched, and what it costs to skip it.
- What cannot be tested in this window — no credential, no data, no way to force the failure.
- What ships inert, and what would switch it on.

## Write

<qc-template>

# QC <date>

## Candidate

Per repo: production commit, candidate commit. The migration chain. Whether the image must be
rebuilt.

## Out of bounds

What may not be touched, and why. The `run-qc` skill asserts this before it starts.

## Unreachable

What cannot be exercised in this window, with the reason. These are NOT EXERCISED before anybody
starts, so nobody spends time discovering it.

## Blocks

One block per area, independent of the others, each carrying a severity of CRITICAL, HIGH or
MEDIUM — whether one failure blocks the deploy.

Each item: an imperative action, then the expected result. Not "it works" — "answers 422 naming
the parameter".

</qc-template>

## Rules

- Riskiest block first. What reaches a person outside, what two sides shipped separately, what
  changes behaviour with no switch, and what was closed but never deployed.
- A claim of absence carries the control that proves the instrument would see the presence.
  Without one it is not verifiable, and it does not go in the plan.
- What the user refused to have tested belongs in the plan, not left out of it.
- You write the plan. You run nothing, and you write no findings.

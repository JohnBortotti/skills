---
name: to-spec
description: "Turn the current conversation into a spec and publish it to the tracker. No interview — synthesis of what was already discussed, on top of a present you established yourself."
---

Take the conversation and the codebase and produce a spec. **Do NOT interview the user** — synthesise
what you already know. This skill runs after `grilling`; the decisions are already made.

The tracker and the triage label vocabulary should have been provided to you.

## The spec is the only artefact nothing makes fail

Lint fails. The suite fails. The adversarial reviewer rejects the PR. The spec does not — and the
reviewer cannot cover it, because it checks the code *against* the spec and takes the spec as
ground.

So a wrong premise in a card walks through implementation green, review green and adversarial
review green, and surfaces weeks later. Everything below exists to give the spec a half that can
be checked before a line of code exists.

## Establish the present

Read where reading answers it, measure where it does not. **The user answers neither.**

- **Reading** answers what a module does, what its interface is, what a guard covers.
- **Only measuring** answers what production actually runs, what the data looks like, whether the
  thing you are about to build already half-exists, and whether work marked Done ever shipped.
  Date production by probing it — a ticket that is Done is merged, not deployed.
- **Dispatch the sub-agents up front and wait for all of them to report** before you write a single
  pair. Nothing is written against a picture still being redrawn.
- **Show the return.** A measurement in the spec carries what it returned, not the fact that it was
  taken.

## Seams

Sketch the seams at which the feature will be tested. Prefer existing seams to new ones. Use the
highest seam possible. The fewer seams across the codebase, the better — the ideal number is one.
If new seams are needed, propose them at the highest point you can.

**Check with the user that these seams match their expectations.**

Use the project's domain glossary throughout, and respect any ADRs in the area you are touching.

## Write it

Use the template below, then publish to the tracker with the `ready-for-agent` triage label. No
further triage.

No file paths and no code, with one exception: a snippet from a prototype that encodes a decision
more precisely than prose can (state machine, reducer, schema, type shape). Inline it in the
decision it belongs to, note that it came from a prototype, and trim it to the decision-rich part.

<spec-template>

## Problem Statement

The problem the user is facing, from the user's perspective.

## Solution

The solution, from the user's perspective.

## Behaviour

One pair per behaviour this spec changes. Every pair is two halves and a mark:

```
**Today**, when <trigger>, <what actually happens>.  [read | measured | assumed]
**After this spec**, when <trigger>, <what happens instead>.
```

- **The "today" half is a claim about the running system, and it is the half that can be wrong.**
  That is the point of the shape. "As a user I want X so that Y" is true in every possible state of
  the world, so nothing downstream can ever fail on it; a wrong card premise is literally a wrong
  "today" half.
- A trigger that does not exist yet still gets a today half: *"today there is no way to <trigger>"*.
- The mark says how the half was established. `assumed` is permitted and must be visible — an
  unestablished today half is the cheapest defect in this pipeline to catch here and the most
  expensive to find later.
- `measured` carries the return inline, short.

## Implementation Decisions

Modules built or modified, the interfaces that change, technical clarifications, architectural
decisions, schema changes, API contracts, specific interactions.

Mark any decision the user took **against your recommendation**, with your recommendation in one
line. This is not the same as whether it was measured — a decision can be measured and still be
one you argued against.

## Alternatives Rejected

What else was considered, and why it lost. One line each.

Not the same as Out of Scope: scope is what will not be built, an alternative is what was nearly
built instead. It is the first thing lost when a conversation is compressed into imperatives, and
the first thing whoever revisits this decision will look for.

## Testing Decisions

What makes a good test here (external behaviour, not implementation details), which modules will be
tested, and prior art for those tests in the codebase.

## Out of Scope

What this spec does not cover.

## Further Notes

Anything else.

</spec-template>

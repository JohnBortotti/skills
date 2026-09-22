---
name: to-spec
description: "Turn the current conversation into a spec and publish it as a GitHub issue — one issue when the work fits one PR, a parent issue with one sub-issue per delivery when it does not. No interview — synthesis of what was already discussed, on top of a present you established yourself."
---

Take the conversation and the codebase and produce a spec. **Do NOT interview the user** — synthesise
what you already know. The decisions are already made.

## The spec is the only artefact nothing makes fail

Lint fails. The suite fails. The adversarial reviewer rejects the PR. The spec does not — and the
reviewer cannot cover it, because it checks the code *against* the spec and takes the spec as
ground.

So a wrong premise in a spec walks through implementation green, review green and adversarial
review green, and surfaces weeks later. Everything below exists to give the spec a half that can
be checked before a line of code exists.

## Establish the present

Read where reading answers it, measure where it does not. **The user answers neither.**

- **Reading** answers what a module does, what its interface is, what a guard covers.
- **Only measuring** answers what production actually runs, what the data looks like, whether the
  thing you are about to build already half-exists, and whether work marked Done ever shipped.
  Date production by probing it — an issue that is closed is merged, not deployed.
- **Dispatch the sub-agents up front and wait for all of them to report** before you write a single
  pair. Nothing is written against a picture still being redrawn.
- **Show the return.** A measurement in the spec carries what it returned, not the fact that it was
  taken.
- **An `assumed` half is also listed in Unestablished**, with the reason. A mark buried in the body
  is not a signal.

## Seams

Sketch the seams at which the feature will be tested. Prefer existing seams to new ones. Use the
highest seam possible. The fewer seams across the codebase, the better — the ideal number is one.
If new seams are needed, propose them at the highest point you can.

Name the existing seam you are proposing and show it — the test, the helper, the boundary that
already carries this. A seam either exists or it does not, and establishing that is yours, not the
user's to remember.

Take it to the user only when the choice is real: a NEW seam, and then with what it costs — or a
pick between existing seams that are equally good on paper, because they know which boundary is
about to move and the codebase does not.

Use the project's domain glossary throughout, and respect any ADRs in the area you are touching.

## Invariants

A file that already carries several merged behaviours is where a regression is born, and whoever
implements has no way to know which parts are load-bearing. The spec says it.

**A reason survives refactoring; a bare rule does not** — *"the cache key carries the tenant id,
because two tenants sharing a key serve one tenant's rows on the other's page"* works; *"don't touch
the cache key"* does not.

Draw them from the Implementation Decisions and from the today halves the changed behaviour
depends on. Two readers use them: `implement` must not loosen one without arguing it in the PR,
and the adversarial reviewer takes its aim from them. A spec with nothing to protect says so; an
empty section is not the same as a section nobody filled in.

## Deliveries

**One PR per repo the work touches is the default.** A PR lands in one repo, so work that spans two
repos is two deliveries whatever its size, the repo the other depends on first. Work in one repo is
one delivery.

**Cut further only with a number in hand.** A delivery is too big when its diff is too wide for a
reviewer to hold, or it needs more than one fresh context window. Neither is visible before the code
exists, so the only evidence is measured: the size of a merged PR of comparable work in the same
repo. Put that number next to the cut. Without one, do not cut.

The prior to distrust is ticket size — what a card used to hold, what a person ships in a day. An
agent implements this, and every extra delivery adds a merge wait and a review without making any
one of them easier to hold.

<delivery-rules>

- A delivery is demoable or verifiable on its own, and lands as one PR
- Within a repo, a delivery cuts a narrow but COMPLETE path through every layer that repo carries —
  vertical, NOT a horizontal slice of one layer. Across repos, the ordered deliveries complete the
  path together
- Prefactoring rides in the delivery that needs it. It becomes its own first delivery only when it
  passes the measured rule above. "Make the change easy, then make the easy change."
- Deliveries are ordered; the next one starts when the previous one merged

</delivery-rules>

Every "After this spec" half belongs to exactly one delivery — name it against the half. A half no
delivery owns is a promise the spec made and nobody will keep.

**Wide refactors are the exception to vertical slicing.** One mechanical change — rename a column,
retype a shared symbol — whose blast radius fans across the codebase cannot land green as a
vertical slice. Sequence it as **expand–contract**: add the new form beside the old; migrate the
call sites in the fewest batches that stay reviewable, one delivery each; delete the old form once no
caller remains. The After half belongs to the contract delivery.

**Granularity stays with the user.** Whether a delivery is the size they want to see working is
intent, not fact. Show them the cut before you publish: each delivery, its repo, the After halves it
makes true, and the measured number behind any cut beyond one per repo. Ask whether it is right. Do
not offer candidate splits or merges — a menu of cuts to prune turns the default around.

## Write it

Use the template below.

No file paths and no code, with one exception: a snippet from a prototype that encodes a decision
more precisely than prose can (state machine, reducer, schema, type shape). Inline it in the
decision it belongs to, note that it came from a prototype, and trim it to the decision-rich part.

<spec-template>

## Problem Statement

The problem the user is facing, from the user's perspective.

## Solution

The solution, from the user's perspective.

## Unestablished

Every today half that could not be established, with the reason: no access, no data, no way to
force the state. It is declared here, before the pairs, so the count reaches the reader before the
body does.

Empty is the good outcome, and says so: *"none — every today half was read or measured."*

## Behaviour

One pair per behaviour this spec changes. Every pair is two halves and a mark:

```
**Today**, when <trigger>, <what actually happens>.  [read | measured | assumed]
**After this spec**, when <trigger>, <what happens instead>.
```

- **The "today" half is a claim about the running system, and it is the half that can be wrong.**
  That is the point of the shape. "As a user I want X so that Y" is true in every possible state of
  the world, so nothing downstream can ever fail on it; a wrong spec premise is literally a wrong
  "today" half.
- A trigger that does not exist yet still gets a today half: *"today there is no way to <trigger>"*.
- The mark says how the half was established. `assumed` is permitted and must be visible — an
  unestablished today half is the cheapest defect in this pipeline to catch here and the most
  expensive to find later.
- `measured` carries the return inline, short.

## Invariants

What may not loosen when this lands. One per line, each with the reason it exists — or "none", said
so.

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

## Deliveries

Only when the work was cut. Numbered in order: title, the repo its PR lands in, and the After halves
it makes true. A cut beyond one per repo carries the measured size that justified it. Omit the
section when the spec is one PR.

## Out of Scope

What this spec does not cover.

## Further Notes

Anything else.

</spec-template>

## Publish

On GitHub Issues, with no labels. The GitHub CLI must be recent enough to have
`gh issue create --parent`.

**One PR** → one issue, the whole spec as its body, in the repo where the PR will land.

**Deliveries** → the spec is the parent issue, in the repo that carries most of the work. Then one
sub-issue per delivery, **in delivery order**, each in the repo where its own PR will land:

- Same repo as the parent: `gh issue create --parent <parent-number> ...`
- Another repo: `gh issue create --repo <owner>/<repo> --parent <parent-url> ...`. Parent and
  sub-issue must belong to the same owner.

<sub-issue-template>

## Parent

The parent spec, by reference. **Read it in full before starting** — this issue is one delivery of
it, not a summary of it.

## What to deliver

The end-to-end behaviour this delivery makes work, from the user's perspective — not a
layer-by-layer implementation list. Quote the After halves of the parent that it makes true.

## Invariants

The parent's invariants this delivery touches, each with its reason — or "none".

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Closing

This delivery's PR says `Closes <owner>/<repo>#<n>` for this issue. **The last delivery's PR also
closes the parent** — GitHub does not close a parent when its sub-issues close.

</sub-issue-template>

Do NOT close or modify any existing parent issue.

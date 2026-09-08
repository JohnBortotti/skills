---
name: to-tickets
description: "Break a spec, plan, or conversation into tracer-bullet tickets, each declaring its blocking edges and the invariants that may not loosen, published to the configured tracker."
---

# To Tickets

Break a spec, plan, or conversation into **tickets** — tracer-bullet vertical slices, each declaring
the tickets that **block** it.

This runs on the output of `to-spec`. **The ticket is the text a worker executes**: one agent is
dispatched per ticket and told to read it in full. Write it for that reader.

The tracker and the triage label vocabulary should have been provided to you.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec
path, an issue number or URL) as an argument, fetch it and read its full body and comments.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.
Ticket titles and descriptions use the project's domain glossary, and respect any ADRs in the area
you are touching.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change
easy, then make the easy change."

### 3. Draft vertical slices

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests) —
  vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Any prefactoring is done first

</vertical-slice-rules>

Give each ticket its **blocking edges** — the other tickets that must complete before it can start.
A ticket with no blockers can start immediately.

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical
change — rename a column, retype a shared symbol — whose **blast radius** fans across the whole
codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land
green. Do not force it into a tracer bullet; sequence it as **expand–contract**. First expand: add
the new form beside the old so nothing breaks. Then migrate the call sites in batches sized by
blast radius (per package, per directory), each batch its own ticket blocked by the expand, keeping
CI green batch to batch because the old form still exists. Finally contract: delete the old form
once no caller remains, in a ticket blocked by every migrate batch. When even the batches cannot
stay green alone, keep the sequence but let them share an integration branch that all block a final
integrate-and-verify ticket — green is promised only there.

### 4. Check the slices against the spec

Two checks, both against the spec's `## Behaviour` pairs. Run them before you show anything to the
user, and **state the result of each one even when it is clean** — a check that reports only on
failure is indistinguishable from a check that never ran.

**Coverage.** Every "After this spec" half has exactly one **owner**: the ticket that makes that
behaviour true. Name it against each half.

In a normal vertical slice the owner is the slice itself. In an expand–contract sequence it is the
**last** ticket of the sequence — the contract, or the integrate-and-verify where there is one.
The earlier tickets of that sequence own nothing, and their absence from the owner list is the
correct result, not a gap.

A half with no owner is what this check is for: the common failure is not "a ticket forgot
something", it is "no ticket became the owner of a promise the spec made".

**Premise.** Any ticket delivering an "After" half whose "Today" half is marked `assumed` rests on
a fact nobody established. List those tickets with the assumption each one rests on. This is the
last step before an agent writes code against it.

Clean reads as a statement, not as silence: *"coverage: every After half has an owner"*,
*"premise: no ticket rests on an assumed half"*.

A ticket that keeps an assumed premise after the user decides to go ahead **carries that assumption
in its body**, so the worker reads it too.

### 5. Present the breakdown

A numbered list. For each ticket: **Title**, **Blocked by**, **What it delivers**.

**Present every blocking edge with the reason it exists** — what the later ticket needs that the
earlier one produces. The user checks a reason, not their memory of the graph. Do not ask them to
verify edges from memory.

**Granularity stays with the user.** Does the slice size match what they want to see working? That
is intent, not fact. Ask it, and ask whether any tickets should be merged or split.

When two breakdowns are equally good on paper, take it to them — they know what is about to move
and the codebase does not.

Iterate until the user approves the breakdown.

### 6. Publish

Publish the approved tickets. **How** depends on the configured tracker — the tickets are the same
either way, only the shape of the blocking edges changes:

- **Local files** → one file per ticket under `.scratch/<feature-slug>/issues/<NN>-<slug>.md`,
  numbered from `01` in dependency order (blockers first). Each file's "Blocked by" lists the
  numbers/titles it depends on. One ticket per file, never a single combined file.
- **A real issue tracker** → one issue per ticket in dependency order (blockers first), so each
  ticket's blocking edges can reference real identifiers. Use the platform's native blocking /
  sub-issue relationship where it has one; otherwise set each ticket's "Blocked by" to the blocking
  issues. Apply the `ready-for-agent` triage label unless instructed otherwise — the tickets are
  agent-grabbable by construction.

Work the **frontier**: any ticket whose blockers are all done. For a purely linear chain that means
top to bottom.

Do NOT close or modify any parent issue.

<local-ticket-template>

# <NN> — <Ticket title>

**What to build:** the end-to-end behaviour this ticket makes work, from the user's perspective —
not a layer-by-layer implementation list.

**Blocked by:** the numbers/titles of the tickets that gate this one, or "None — can start
immediately".

**Status:** ready-for-agent

**Invariants:**

- <what may not loosen> — because <reason>

- [ ] Acceptance criterion 1
- [ ] Acceptance criterion 2

</local-ticket-template>

<issue-template>

## Parent

A reference to the parent issue on the tracker (if the source was an existing issue, otherwise omit
this section).

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective — not layer-by-layer
implementation.

## Invariants

What may not loosen when this ticket lands. One per line, each with the reason it exists.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Blocked by

- A reference to each blocking ticket, or "None — can start immediately".

</issue-template>

## Invariants

A file that already carries several merged behaviours is where a regression is born, and the worker
has no way to know which parts are load-bearing. **A reason survives refactoring; a bare rule does
not** — *"the cache key carries the tenant id, because two tenants sharing a key serve one tenant's
rows on the other's page"* works; *"don't touch the cache key"* does not.

Draw them from the spec's Implementation Decisions and from the today halves this ticket's behaviour
depends on. A ticket with nothing to protect says so; an empty section is not the same as a section
nobody filled in.

## Paths and snippets

In either form, avoid specific file paths or code snippets — they go stale fast, and a ticket can
sit in the backlog for weeks. Exception: if a prototype produced a snippet that encodes a decision
more precisely than prose can (state machine, reducer, schema, type shape), inline it and note
briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just
the important bits.

Work the frontier one ticket at a time, clearing context between tickets.

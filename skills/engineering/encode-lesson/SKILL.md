---
name: encode-lesson
description: "Turn a correction that keeps coming back into a mechanism (a type, a lint rule, a shared helper, a runtime check) and delete the instruction it replaces. Use when the user says a correction is the second time, 'make this a lint', 'encode this', or when an adversarial review marks a finding as a lint candidate."
disable-model-invocation: true
---

An instruction works only when the agent reads it, remembers it, and obeys it. A mechanism works without its cooperation. Agents also copy the code around them, so a mistake that already sits in three files lands in the fourth whatever the instructions say.

## Name the lesson

One sentence: the wrong thing, the right thing. Then the two occurrences that make it a lesson: the PRs, the `file:line`, or the correction in this session. One occurrence is a one-off, not a lesson. Say so and stop.

Find the instruction that states the rule today, if one exists: `CLAUDE.md`, a skill, a doc, a comment.

## Pick the strongest mechanism

In this order, the first one the situation allows:

1. **A type or structure** that makes the wrong state fail to compile.
2. **A lint rule or a banned API** that fails the repo's gate.
3. **One shared helper**, with every duplicate migrated to it.
4. **A runtime check** that fails loudly and names the problem.

The order matters because a weaker guard becomes the next template. Agents copy code without the guard as readily as code with it.

Use what the repo already runs: its linter's configuration (restricted syntax, restricted imports, banned APIs), a test that scans the tree, the type system. Write a custom plugin only when configuration cannot express the rule.

The error message says what to do instead, and names the helper or pattern to use.

## Prove it

Put the original mistake back and watch the gate fail with that message. Remove it and watch the gate pass.

Fix every existing violation in the same change, or the gate is red on the main branch. When they are too many for one PR, stop and report the count. Do not add a silent ignore list.

## Delete the instruction

Delete the instruction the mechanism replaces. If the rule needs judgment and no mechanism fits, do not force one. Make the instruction more prominent, add an example of the failure it prevents, and say why no mechanism fit.

## Ship it alone

One PR for the lesson, never inside a feature PR. It usually touches code across the repo, and mixing it in makes both harder to review.

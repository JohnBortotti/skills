---
name: open-pr
description: "Write and open a pull request whose description is a briefing a reviewer reads once: why, scope, tradeoffs, blast radius, and how it was verified. Use when opening a PR, or when the user asks to rewrite a PR description."
disable-model-invocation: true
---

The description is a briefing, not a lab notebook. A reviewer who has the diff learns from it why the change exists, what is out of scope, and how you proved it works.

Adapted from the `opening-a-pr` playbook of `poteto-mode` in pstack (MIT, © 2026 Lauren Tan).

## Title

State what is true after the merge. Match the style of the repo's recent merged PRs (`gh pr list --state merged --limit 10`). Name a real symbol when one carries the change. No trailing period.

## Description

These sections in this order. Drop a section with nothing to say.

- **Why.** The problem and the approach, in one or two short paragraphs. From the user's point of view first, then the mechanism.
- **Scope.** Bullets with real symbols and paths. Name both sides of a rename. State what is out of scope only when the boundary matters.
- **Tradeoffs.** Only the rejected alternatives a reviewer would otherwise ask about.
- **Blast radius.** One to three sentences on who or what the change touches, and why it is safe or risky.
- **Verification.** Each real run and its outcome. A performance change reports one number, before and after, with its unit.

Then the evidence. Attach screenshots or a recording when they prove a claim (`gh pr create --attach '<file>#<alt text>'`), and say what each one shows. Open every image before attaching it. A blank screen is a finding, not evidence. When the change touches no screen, say so.

When the PR closes an issue, the last line is `Closes <owner>/<repo>#<n>`.

Keep the whole body under about 40 lines. Leave out full SHAs, file-by-file lists, "Summary" and "Test plan" boilerplate, and the story of how you got there. Write it with the **unslop** skill.

## Open it

Open it ready for review, not as a draft. Check it with `gh pr view` before you report its URL.

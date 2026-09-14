---
name: implement
description: "Implement one issue — a spec, or one delivery of a spec — and open the PR that closes it."
disable-model-invocation: true
---

Implement the work described by the issue the user points you to.

Read the whole issue with its comments (`gh issue view <n> --comments`), and the parent spec in full
when the issue is a delivery of one. A summary of the issue makes you build the summary.

Respect the spec's invariants. **If you disagree with a decision in the spec, argue it in the PR
instead of changing it on your own.** The spec is what the reviewer checks your code against; a
silent change leaves it describing something else, and a regressing spec implemented in silence is
no better. Stopping to ask about an ambiguous instruction is worth more than obeying it.

Use /tdd where possible, at the seams the spec names.

Run typechecking regularly, single test files regularly, and the full test suite once at the end —
in the foreground, waiting for it to finish inside the turn.

Docs the change made outdated are updated in the same PR.

Once done, use /code-review to review the work, and fix the real findings.

Before the first push, name the branch after the issue, renaming the current one in place:
`git branch -m <n>-<short-slug-of-the-title>`, where `<n>` is the number of the issue this PR closes
(e.g. `405-lob-mails-a-letter-a-person-approved`). A
session opened in a worktree sits on a branch named after the session (`worktree-bridge-cse_…`), and
pushed as is, that name is what the PR carries. Rename, don't create a new branch — the worktree is
checked out on this one.

Commit your work to that branch and open the PR. Its description says
`Closes <owner>/<repo>#<n>` for the issue, and also closes the parent spec when this is its last
delivery.

**Evidence.** When the change touches a screen, attach screenshots or a recording to the PR
(`gh pr create --attach '<file>#<alt text>'`, or `gh pr comment --attach`), and say what each one
shows. Open every image before attaching it — a blank screen or a captured error is a finding, not
evidence. What cannot be photographed, like something called zero times or never requested, gets a
count from instrumentation instead. When the change touches no screen, say so explicitly in the PR:
*"this change touches no screen — the evidence is the suite."*

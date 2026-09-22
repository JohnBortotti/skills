---
name: implement
description: "Implement a change, from an issue, a spec, or the user's prompt, and open its PR."
disable-model-invocation: true
---

Implement the work the user points you to: an issue, or the prompt itself.

When there is no issue, the prompt and the conversation are the source. If the user ran /restate, the
corrected restatement is.

When there is an issue, read the whole issue with its comments (`gh issue view <n> --comments`), and
the parent spec in full when the issue is a delivery of one. A summary of the issue makes you build
the summary.

When there is a spec, respect its invariants. **If you disagree with a decision in the spec, argue
it in the PR instead of changing it on your own.** The spec is what the reviewer checks your code
against; a silent change leaves it describing something else, and a regressing spec implemented in
silence is no better. Stopping to ask about an ambiguous instruction is worth more than obeying it.

**No tautological tests.** A test has to be able to fail when the behaviour breaks. Before you keep
one, ask whether it would still pass if every function it imports returned `undefined`; if it would,
it tests nothing. The usual shapes: asserting a string or substring the code itself contains (a
message, a prompt, a constant), an expected value computed by the code under test, an assertion only
that a mock was called, and a check on data the test built itself. Call the code with a concrete
input and assert the literal output or the observable effect, or delete the test.

Run typechecking regularly, single test files regularly, and the full test suite once at the end —
in the foreground, waiting for it to finish inside the turn.

Docs the change made outdated are updated in the same PR.

**Mutate what you changed.** Once the suite is green, break each guard, condition and call site the
diff adds — delete it, invert it, return early — and watch a test fail **on its assertion**; a crash or
an import error proves nothing. Run the repo's own command for this over the diff where it has one. A
survivor that matters gets the behaviour test that kills it; one that does not gets a line in the PR
saying why. **Never kill a mutant with a test coupled to the implementation** — pinning a message,
counting a mock's calls, reading the source: it goes red without holding anything.

Once done, use /code-review to review the work. When there is no spec, first write down what the
change is for and what it touches (the PR's Why and Scope) in a file outside the repo, and pass its
path to /code-review as the spec. Fix the real findings and run /code-review again. Open the PR only
when it comes back with no real finding. A finding you still disagree with after two rounds goes into
the PR description, argued.

Before the first push, rename the current branch in place: `git branch -m <n>-<short-slug>`, where
`<n>` is the number of the issue this PR closes (e.g. `412-export-invoices-as-csv`), or
just `<short-slug>` when it closes none. A
session opened in a worktree sits on a branch named after the session (`worktree-bridge-cse_…`), and
pushed as is, that name is what the PR carries. Rename, don't create a new branch — the worktree is
checked out on this one.

Commit your work to that branch and open the PR. When it closes an issue, its description says
`Closes <owner>/<repo>#<n>` for the issue, and also closes the parent spec when this is its last
delivery.

**Evidence.** When the repo has a `verify-<app>` skill, use it to exercise the change and capture the
evidence. When the change touches a screen, attach screenshots or a recording to the PR
(`gh pr create --attach '<file>#<alt text>'`, or `gh pr comment --attach`), and say what each one
shows. Open every image before attaching it — a blank screen or a captured error is a finding, not
evidence. What cannot be photographed, like something called zero times or never requested, gets a
count from instrumentation instead. When the change touches no screen, say so explicitly in the PR:
*"this change touches no screen — the evidence is the suite."*

**Adversarial review.** Once the PR is open, spawn one subagent whose whole prompt is
`Use the adversarial-review skill on <PR URL>.` Nothing else. No summary of what you did, no hint of
what is fine. It posts its verdict on the PR itself. Do not relay, summarise, or act on the verdict.
Report the PR URL to the user and stop. The user reads the verdict on the PR and decides.

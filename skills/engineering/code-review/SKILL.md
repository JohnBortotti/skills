---
name: code-review
description: "Review a diff along two axes, each in its own subagent: Standards (does the code follow the repo's conventions and the patterns around it?) and Intent (does it do what the spec asks, or what the change says it is for?). Use when implement reaches its review step, or when the user asks to review a branch or work in progress."
---

Review the diff between `HEAD` and a fixed point. You report findings. The caller fixes them.

## 1. Pin the diff

The fixed point is what the caller passed. Otherwise it is the merge-base with the default branch (`git merge-base origin/main HEAD`). Capture `git diff <fixed-point>...HEAD` and `git log <fixed-point>..HEAD --oneline`.

Fail here on a ref that does not resolve or an empty diff, not inside two subagents.

## 2. Find the intent

In this order, the first that exists:

1. A spec path the caller passed.
2. The issue the commits reference (`Closes #n`, `#n`), read with `gh issue view <n> --comments`.
3. The description of the PR open for this branch (`gh pr view --json body`).

When none exists, ask the caller what the change is for. Never skip the Intent axis.

## 3. Find the standards

What the repo writes down: `CLAUDE.md`, `AGENTS.md`, contributing docs, the docs they point to. And what the repo shows: how the files around the diff already do the same kind of thing. Leave out what the linter and the formatter already enforce.

## 4. Run both axes in parallel

One subagent per axis, so neither one's context leaks into the other. Give each the diff command and only what its axis needs.

- **Standards.** A finding names the `file:line`, the convention it breaks, and where that convention is written or shown: a line in a doc, or a neighbouring file that does it right. A preference with no written or shown convention behind it is not a finding.
- **Intent.** For each thing the intent asks, does the diff do it? And what does the diff do that the intent does not ask for?

Each finding is marked **real** (it breaks a convention or misses the intent) or **nit**.

## 5. Report

The two axes side by side. Real findings first, each with its `file:line` and why. Nits after, one line each. "Nothing found" is a valid result for either axis. Do not invent a finding to fill one.

---
name: create-verification-skill
description: "Generate a repo-local verify-<app> skill: a small CLI an agent runs to bring the app up, check it is healthy, drive it the way a user does, and capture evidence. Use when a repo has no scripted way to prove behaviour, or when the way to run and test it is prose an agent re-derives every session."
disable-model-invocation: true
---

Write `.claude/skills/verify-<app>/`: a `SKILL.md` and a CLI. The CLI is the point. A command costs one tool call and a few lines of output. The same step written as prose costs a re-read, a throwaway script, and a page dump every session, and drifts between sessions.

You write for an agent that reads the skill cold, mid-task, and has never seen the app.

## 1. Read the repo, not the user

Answer from the code. Ask the user only what you cannot observe.

- **Surface.** What does a user touch? A web UI, an API, a CLI, a worker. Pick the primary one and note the rest.
- **Run.** How does it start? The repo's own commands first: package scripts, compose files, the README, any skill or doc that explains how to run it. Note env vars, seed data, auth, and services it needs from other repos.
- **Drive.** How can a script interact with it? Existing tooling first: Playwright or Cypress specs, HTTP endpoints, a debug port. Then a generic recipe: Playwright as a library for a web UI, plain HTTP for a service, a PTY for a CLI.
- **Evidence.** What proves a behaviour? Screenshots, response bodies, rows in the database, logs, exit codes.

If the checkout does not build or start as it is, fix that first or report it precisely. A skill written against a broken base teaches wrong steps.

## 2. Write only what the agent cannot find

Agents copy the patterns the repo already has and solve what they see when it happens: a port in use, a stale container, a missing dependency. Do not write rules for those. Write what the agent cannot see in the repo or discover by running:

- where test credentials live, and which seeded user to log in as;
- which services must be up, and from where;
- what counts as proof for this app.

Leave git and worktree handling to Claude Code, and machine setup in a cloud session to the environment's setup script.

## 3. Build the CLI

One executable in the skill directory, in the repo's own language, using dependencies the repo already has. When driving needs a library the repo lacks (Playwright for a web UI), add it as a dev dependency and say so in the PR.

Start with these commands and add more only when a real check needs them:

- **Lifecycle.** `up`, `down`, `doctor`.
- **Gate.** `check` runs the repo's full lint, type, and test gate and exits non-zero on any failure.
- **Drive.** The smallest set that reaches the primary surface, like `open <route>`, `login [--as <user>]`, `click`, `type`, or `call <method> <path>` for an API.
- **Inspect.** `snapshot`, `screenshot <path>`, `logs`.

`doctor` is read-only and answers one question: is this instance worth driving? Processes up, the served code is this checkout's, auth works, required env present. Each failure names the command that fixes it.

Design rules:

- Every input is a flag or an argument. No prompts, no menus.
- Every subcommand has `--help` with real examples from this repo. The top-level help lists commands only.
- Output is JSON on stdout: ids, URLs, ports, paths, durations. Progress and logs go to stderr.
- An error exits non-zero and shows the exact command to run instead.
- Commands are idempotent. `up` twice reuses the running instance, and `down` on nothing is a no-op.
- Anything destructive takes `--dry-run`.
- `snapshot` returns what the next action needs: interactive elements with their role, name, and a stable handle. Never the whole DOM or the whole accessibility tree.
- Wait on an observable state (a port answering, an element present), never on a fixed sleep.

## 4. Write the skill

`SKILL.md` with frontmatter (`name: verify-<app>`, and a description naming the app, the surface, and when to use it) and these sections:

- **Quick start.** `up`, `doctor`, one drive, `screenshot`, `down`, as commands.
- **Commands.** The groups, with `--help` as the reference. Do not restate flags.
- **Proof.** A capture of the app opening is not proof. Exercise the real user path and show the trigger and the resulting state. Run `doctor` first, because evidence from a stale build is not evidence. Check side effects, not only pixels: the response, the row, a reload that shows the state persisted. A mock counts only behind a boundary production also isolates. Name any path you could not reach and what blocked it.
- **Keep it alive.** A change that moves what this skill drives updates the skill and the CLI in the same PR.

When the repo already has a skill or doc that explains how to run and test the app, shrink it to point at `verify-<app>` instead of deleting it. Other skills and `CLAUDE.md` may reference it by name. Move every step the CLI now does out of its prose.

## 5. Prove it before handing it over

Run it end to end once from a clean state: `up`, `doctor`, `check`, one drive with a screenshot, `down`. Then confirm the evidence still exists. Fix what fails, and run `down` after every failed attempt. A skill that was never executed is a draft.

Open a PR with the skill, the CLI, and the evidence from this run.

---
name: maintain-verification-skill
description: "Check that a repo's verify-<app> skill and its CLI still work against the current app, fix what drifted, and ship at most one PR of proven corrections. Use when the user asks to maintain or audit the verification skill, or before a release QC."
disable-model-invocation: true
---

The main path for keeping `verify-<app>` current is the PR that changes what it drives. This pass catches what slipped. One agent, driving serially.

Adapted from `maintain-verification-skill` in pstack (MIT, © 2026 Lauren Tan).

## Outcomes

Pick one and say which:

- **clean.** Every command worked and the skill matches the app. No PR.
- **changed.** One PR of proven corrections.
- **blocked.** The pass could not finish, or a fix could not be proven. Say exactly what blocked it.

## Edit scope

Only the skill's own directory: its `SKILL.md` and its CLI. Never product code. When the app no longer does what the skill describes, decide which it is. Drift means the skill is wrong, so fix it. A regression means the app is wrong, so report it to the user and keep it out of the PR.

## Pass

1. **Locate** `.claude/skills/verify-*/`. None means stop and point at `create-verification-skill`.
2. **Scope.** Read the merges on the main branch since the skill last changed (`git log <last-change>..origin/main`). Note what they did to the surface the skill drives: routes, commands, endpoints, env vars, seed data, dependencies.
3. **Doctor.** `up`, then `doctor`. A failure caused by the skill itself (a renamed env var, a moved command) is drift. Fix it and retry once before calling the pass blocked.
4. **Drive.** Run the quick start, every command at least once, and every part of the surface the merges touched. Run `doctor` again after any failed drive. Keep the evidence.
5. **Triage** each failure:
   - the skill describes it wrong or not at all, so fix the skill;
   - the app works but the CLI cannot drive it, so fix the CLI and drive it again;
   - the app is broken, which is a regression, so report it.
6. **Ship or stop.** For changed, open one PR, re-read every changed file, and attach the evidence. For clean or blocked, report what was driven and what was not.
7. **`down`**, then confirm the evidence is still there.

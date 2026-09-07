---
name: run-qc
description: "Execute a QC plan and consolidate the results into findings. Use when a QC document exists and the user wants it run."
disable-model-invocation: true
---

Execute `QC_<date>.md` and write `FINDINGS_<date>.md`.

A badly written item is a finding about the plan, not an edit of yours. How the run is organised —
one pass or many, delegated or not, foreground or background — is the user's to compose.

## Before starting

**Prove the boundary.** Assert the plan's out-of-bounds section holds — the credential is absent,
not merely forbidden. Some items have you driving a part of the product that carries its own
credentials and tools; a prompt on your side does not reach those, and a missing connection
does. If the boundary cannot be asserted, do not start.

**Prove the instruments.** A blind instrument returns empty, and empty looks like approval. For
every claim of absence in the plan, plant the signal first and confirm the reader sees it. A
control that fails marks its whole class of item unreachable up front rather than stopping the QC.

**Use what is here.** Whatever this environment offers to drive a browser, query logs, run
commands and drive the product itself. A missing capability makes its items NOT EXERCISED,
declared up front rather than discovered mid-run.

Anything you delegate inherits nothing — carry the item numbers, the boundary and the result
format into the prompt.

## Judging an item

Every item ends in PASS, FAIL or NOT EXERCISED.

- **No artefact is not a PASS.** Command output, HTTP status, screenshot path, query result.
- **NOT EXERCISED is cheap and blameless.** If amber costs anything, green gets reported instead.
- **An empty reading is never a finding.** Below the instrument's floor it means retry, then NOT
  EXERCISED with the number recorded.
- **Existence and count are read off the DOM. Appearance is read off the screenshot**, and only on
  a frame the DOM has confirmed is painted.
- A judgement item is graded by an independent second pass that does not see the first verdict.

## Consolidate

`FINDINGS_<date>.md`: the verdict per repo, the findings with action and real result, what was
verified green with its artefact, what was not exercised and why, and the boundary and calibration
results.

A finding that closes during the investigation stays in the file as closed. A false finding
teaches method, and the next QC repeats it if nobody writes it down.

Filing tickets is a step of its own, after the findings exist. The evidence goes on the ticket.

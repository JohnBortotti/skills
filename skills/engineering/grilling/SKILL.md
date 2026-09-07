---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round. Then wait for the user's answers before the next round.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Read the tree ahead and send those sub-agents before the first round. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Hard rule 1: nothing is asked while a sub-agent is still running

Dispatch the sub-agents up front, before the session starts, not round by round. Then **wait for all of them to report before you ask anything**.

A question sent early overlaps two things that must not overlap: the user answering, and facts still arriving. The user decides against a picture you are still redrawing, a sub-agent then contradicts the premise they answered on, and the question has to be asked again. Waiting costs one pause. Not waiting costs the answer.

## Hard rule 2: the shape of a question

Every question in a round is three blocks and a lettered list, in this order:

```
**Q1 — <title>**

**Context.** What is true today. Facts you established, not guesses.

**Problem.** What stays undecided or breaks until this is settled.

**Recommendation.** The answer you would pick, and why it beats the others.

- **A.** <option>
- **B.** <option>
- **C.** <option>
- **D. Other** — <what you would need the user to fill in>
```

As many lettered options as the decision actually has. The last one may be **Other**, for the user to complete, whenever you are not confident the list is exhaustive.

## Throughout

Follow the `wait-what` skill for the whole grilling session — every question, every summary, everything you send.

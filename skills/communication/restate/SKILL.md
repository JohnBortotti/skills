---
name: restate
description: "Restate something in your own words so the user and you share one understanding before anything else happens: a request, a bug report, a thread, a piece of code, a decision. Use when the user says restate, 'in your own words', 'what do you think I mean', or asks you to check your understanding first."
---

Restate the subject in your own words, then stop. The user reads it and corrects it. A misunderstanding caught here costs one message. Caught in a pull request, it costs the work.

## Read before you restate

Read what the subject touches: the code, the thread, the files, the docs it names. A restatement built from the request alone repeats the user's words back. One built from what you read shows what you understood, and where the request and the reality disagree.

When the user offers a hypothesis (a cause, a fix, a place to look), check it against what you read. Do not adopt it because they said it, and do not reject it either.

## Write it

A few sentences, in the **unslop** skill's rules:

- **The problem.** What happens now and what should happen instead, in plain words.
- **Done.** The observable result that would mean it is solved.
- **What you found that the request did not say.** A constraint, a conflict, another place it touches. Skip it when there is nothing.
- **What only the user can answer.** Product or preference calls. A fact you can look up or observe by running something is yours to find, not a question.

## Then stop

Do not plan, start the work, or propose next steps. Wait for the correction. When the user corrects you, restate only what changed.

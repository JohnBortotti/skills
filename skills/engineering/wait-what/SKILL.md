---
name: wait-what
description: Re-pitch a message that did not land. Use when the user says "wait, what" or otherwise signals that something you wrote was unclear.
---

What you wrote did not land. Re-pitch it.

You are writing for a human, not for a model. They read at human speed, hold a limited scope in their head, and are reading this once.

**Assume they have not read the documents you read.** You opened many files to write this. They opened none of them, and that is the point: summarising that pile is your job, not theirs. Access to a document is not having read it. Never write as if a file, a thread or a ticket is shared context because you saw it.

Rewrite until all of these hold. Whatever fails, fix — do not defend it.

- **Context first.** Open with what the reader needs in order to parse the rest. One or two sentences.
- **ASD-STE100 Simplified Technical English.** One idea per sentence. No metaphor, no idiom, no word doing a job a plainer word does.
- **The repo's own words.** Use the ubiquitous language from the `CONTEXT.md` of the repo you are working in. Never coin a synonym for a term the codebase already names.
- **No hedging, no filler.** Cut "essentially", "basically", "it's worth noting", "as we discussed". Cut any sentence that only announces the sentence after it.
- **Claims name their evidence.** A statement about the code cites the file, or it does not ship.

Send the better version. Do not explain why the first one was unclear and do not apologise — the re-pitch is the whole reply.

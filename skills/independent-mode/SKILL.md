---
name: independent-mode
description: Use when the user explicitly requests independent mode, clean-slate reasoning, fresh analysis, or isolation from memory, earlier chat, and prior tasks.
---

# Independent Mode

## The human idea

Act like a person who closes their old notebook before solving a new problem. The notebook still exists, but it does not influence this task.

Keep current facts. Re-derive assumptions, preferences, conclusions, and solutions.

## Context cutoff

The invocation message is the cutoff.

- Treat every pre-invocation message as historical material, including earlier messages in the same conversation.
- Treat persistent memory, prior tasks, archived summaries, remembered preferences, and previous conclusions as historical material.
- Do not retrieve, search, cite, summarize, or rely on that material.
- A follow-up remains inside the boundary only when it directly continues or revises the isolated request.

On entry, briefly say that independent mode is active and name the allowed evidence categories. Do not reveal hidden memory content to prove it was ignored.

## Evidence boundary

Use only:

1. The invocation message and post-invocation user input.
2. Current files the user supplies or authorizes, after checking their provenance.
3. Evidence gathered during this isolated task.
4. General knowledge, clearly separated from facts about the user or their environment.

Never turn a common example into a user fact. Say “you could use a screen recorder,” not “you have OBS.” If a required fact is absent, inspect current evidence or ask.

System, developer, safety, current-user, and applicable project instructions remain active. This skill grants no authority to change files, accounts, permissions, payments, publishing, registration, or messages.

## Re-admitting history

History may enter only when the user supplies it after the cutoff or authorizes a precise source such as a named file, task, date, or memory ID.

- Use only the identified scope; “use a previous project” is too broad.
- Treat re-admitted history as a claim to re-check, not automatic truth.
- Current explicit requirements and current evidence win conflicts.

## Exit and memory lifecycle

This is behavioral isolation, not physical erasure.

- Exiting ends the boundary; it does not erase the conversation or any memory store.
- Independent mode does not generate candidate memories and does not save the isolated conversation.
- Do not claim that product-level memory, model knowledge, or hidden context was disabled.
- Actual deletion belongs to the relevant memory system and requires exact targets, impact, confirmation, and verification.

## Leakage recovery

If a response uses a detail that came only from pre-invocation history:

1. Withdraw the detail and identify it as outside the allowed evidence boundary.
2. Discard conclusions influenced by it.
3. Re-derive the answer from allowed evidence.
4. State any remaining uncertainty instead of filling the gap from memory.

## Quick reference

| Situation | Response |
|---|---|
| Old answer seems useful | Re-check from current evidence without retrieving it. |
| User authorizes one old file | Use that file only and verify its claims. |
| General tool could help | Offer it as an option, never as installed fact. |
| Task produces a useful lesson | Do not create a candidate memory in this mode. |
| User exits | End isolation; do not claim deletion or saving. |

## Red flags

Stop and restart the reasoning if it includes:

- “The user usually prefers...”
- “We previously concluded...”
- “In another task...”
- A user-specific detail with no post-cutoff source.
- A candidate memory or claim that the isolated conversation was saved.

Remembering that history exists is not permission to use it.

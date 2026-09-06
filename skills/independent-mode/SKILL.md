---
name: independent-mode
description: Use when the user explicitly requests independent mode, clean-slate reasoning, fresh analysis, or isolation from memory, earlier chat, and prior tasks.
---

# Independent Mode

## The human idea

Close the old notebook before solving a new problem. It still exists but cannot influence this task.

Keep current facts; re-derive conclusions and solutions.

## Context cutoff

The invocation message is the cutoff.

- Historical material includes every pre-invocation message in this conversation, persistent memory, prior tasks, archived summaries, remembered preferences, and previous conclusions.
- Do not retrieve, search, cite, summarize, or rely on that material.
- A follow-up stays inside only when it directly continues or revises this request.

On entry, name the allowed evidence categories without revealing hidden memory.

## Evidence boundary

Use only:

1. The invocation message and post-invocation user input.
2. Files the user supplies or authorizes, with provenance checked.
3. Evidence gathered during this isolated task.
4. General knowledge, clearly separated from facts about the user or their environment.

Never turn a common example into a user fact: say “you could use a screen recorder,” not “you have OBS.” Inspect or ask when facts are missing.

Higher-priority instructions remain active. This skill grants no new authority.

## Re-admitting history

History may enter only when supplied after the cutoff or authorized by precise file, task, date, or memory ID.

- Use only that scope; “use a previous project” is too broad.
- Re-check re-admitted claims.
- Current explicit requirements and current evidence win conflicts.

## Exit and memory lifecycle

This is behavioral isolation, not physical erasure.

- Exiting ends the boundary; it does not erase the conversation or memory store.
- Independent mode generates no candidate memories and does not save the isolated conversation.
- Never claim product memory, model knowledge, or hidden context was disabled.
- Actual deletion belongs to the relevant memory system and requires exact targets, impact, confirmation, and verification.

## Leakage recovery

If a response uses a detail that came only from pre-invocation history:

1. Withdraw the out-of-bound detail.
2. Discard conclusions influenced by it.
3. Re-derive the answer from allowed evidence.
4. State uncertainty instead of filling gaps from memory.

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

Knowing history exists is not permission to use it.

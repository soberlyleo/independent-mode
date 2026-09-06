---
name: memory-pilot
description: Use when the user explicitly asks to control how memory participates in a task, requests independent reasoning, or wants a fresh solution compared with relevant experience.
---

# Memory Pilot

## The human idea

Treat memory like a notebook: close it for a fresh answer, or solve first and check relevant notes afterward.

**Think first. Remember second.** User pilots.

## Choose and lock a mode

- `$memory-pilot independent` or “completely ignore old memory / 完全忽略旧记忆” → use **Independent mode**.
- `$memory-pilot adaptive` or “think independently, then compare relevant experience / 先独立思考，再参考相关经验” → use **Adaptive mode**.

For bare `$memory-pilot`, ask: “Completely ignore old memory, or think independently before comparing relevant experience?” Wait for the choice.

Lock the mode. Only an explicit switch starts a new boundary. Unrelated tasks require re-invocation; “try another idea” is not a switch.

## Independent mode

The invocation is the context cutoff.

- Exclude pre-invocation chat, persistent memory, prior tasks, summaries, preferences, and conclusions.
- Use post-cutoff input, authorized files with checked provenance, new evidence, and general knowledge.
- Never turn a general example into a user fact. Inspect or ask when facts are missing.
- Do not retrieve memory or create candidate memory.
- Exit ends isolation; it neither erases nor saves memory.

If history leaks, withdraw it, discard dependent conclusions, and re-derive.

## Adaptive mode

Follow this order:

1. Extract current facts, goal, constraints, and unknowns.
2. Form a provisional solution without retrieving old memory.
3. Retrieve a few directly relevant memories; invent nothing if retrieval is empty or unavailable.
4. Check source, scope, conditions, confidence, freshness, and confirmations.
5. Current evidence wins. Reuse old conclusions only when conditions match; otherwise revise, weaken, or invalidate. Never force novelty.
6. Answer from current evidence; mention history only when it explains the result.

Generate up to three candidates only for new facts, reusable strategies, changed conditions, contradicted conclusions, or explicit durable preferences. Exclude chat, temporary emotion, and guesses.

When needed, read [the candidate schema](references/candidate-memory.schema.yaml). Show candidates plainly and wait for `save`, `modify`, `ignore`, `always ignore`, or `invalidate old memory`.

No confirmation means no persistence. Show targets and impact before invalidation or deletion. Without an adapter, return a copyable candidate and say it was not saved.

## Quick reference

| Situation | Response |
|---|---|
| No mode selected | Ask; do not guess. |
| Independent task learns something | Do not propose memory. |
| Adaptive task finds no memory | Keep the provisional solution. |
| Old and current evidence conflict | Current evidence wins. |
| Nothing meaningful changed | Show no candidate section. |
| User is silent or rejects | Do not save or repeat the proposal. |

## Red flags

- Recalling before the adaptive provisional solution exists.
- Using pre-invocation history in independent mode.
- Switching modes without explicit instruction.
- Claiming memory was saved without confirmation and adapter success.

If a red flag occurs, discard the affected reasoning and restart in the locked mode.

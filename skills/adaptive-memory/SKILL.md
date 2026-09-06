---
name: adaptive-memory
description: Use when the user explicitly asks for adaptive memory, a fresh solution compared with relevant past experience, or user-approved learning from a task.
---

# Adaptive Memory

## The human idea

Use memory like a good notebook: solve today’s problem on a blank page, open only relevant old notes, then ask before writing a new note.

Memory is evidence, not an answer key.

## Entry rule

Run only after an explicit request such as `$adaptive-memory` or “think independently first, then compare relevant memory.” “Try another idea” is not authorization.

Confirm the mode and that saving requires approval.

## Required sequence

### 1. Look at today

Extract current facts, goal, constraints, and unknowns. Never fill gaps with remembered user details.

### 2. Think before remembering

Create a provisional solution from current input, authorized current files, new evidence, and general knowledge. Do not retrieve historical memory yet.

### 3. Open relevant notes

Then retrieve only a few memories directly related to the task. If retrieval is unavailable or empty, continue without inventing memory.

Check each memory’s source, scope, conditions, confidence, freshness, and confirmations. Keywords alone are insufficient.

### 4. Compare

Current evidence wins. Keep old conclusions only when their conditions match; otherwise revise, weaken, or invalidate them. Do not change a correct answer merely to appear creative.

### 5. Answer

Give the best current answer. Mention history only when it materially explains the result; never expose unrelated memory.

### 6. Offer learning

Generate zero to three candidate memories only when the task establishes a new fact, reusable strategy, changed condition, contradicted conclusion, or explicit durable preference.

Use `candidate-memory.schema.yaml`. Display its action, content, reason, evidence, scope, conditions, and confidence in plain language.

No confirmation means no persistence. After showing candidates, wait for one of these decisions:

- `save`: send the approved candidate to an available adapter.
- `modify: <new content>`: revise and show again.
- `ignore`: discard for this task.
- `always ignore`: propose a rejection rule and require confirmation.
- `invalidate old memory`: show the exact target and impact, then confirm.

If no write adapter exists, return a copyable candidate and say it was not saved. Never write to an unrelated file as a substitute.

## Quick reference

| Situation | Response |
|---|---|
| No relevant memory | Use the provisional solution. |
| Old and current evidence agree | Reuse only after conditions match. |
| They conflict | Current evidence wins; propose lifecycle change. |
| Nothing meaningful changed | Show no candidate section. |
| User is silent or rejects | Do not save or repeat the proposal. |

## Red flags

- Recalling before a provisional solution exists.
- Saying “the user prefers” without current evidence or retrieved provenance.
- Treating an old success as a universal rule.
- Claiming memory was saved without adapter confirmation.
- Turning ordinary conversation, temporary emotion, or model guesses into memory.

If any red flag occurs, discard the affected reasoning and restart from step 1.

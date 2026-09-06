---
name: independent-mode
description: Use when the user explicitly requests independent mode, a clean-slate judgment, fresh thinking, or analysis isolated from persistent memory, prior tasks, and historical chats.
---

# Independent Mode

Treat the request as fresh analysis. Preserve explicit requirements and raw facts supplied now; re-evaluate conclusions, assumptions, preferences, and proposed solutions.

## Evidence boundary

- Use current instructions, current workspace files, evidence gathered now, and general knowledge. If a fact is missing, inspect current evidence or ask.
- Do not read, search, cite, or rely on persistent personal memory stores.
- Do not inspect or rely on prior tasks, archived tasks, summaries, historical chats, or cross-session context.
- Classify workspace files by provenance before using them. Memory directories, history exports, rollout summaries, and prior-task artifacts are out of scope unless the user explicitly supplies and authorizes them.
- Do not infer a choice from remembered user preferences, earlier projects, or previously accepted solutions.

System, developer, safety, current user, and applicable project instructions remain active. This mode does not authorize file, account, publishing, payment, registration, permission, or messaging changes.

This is a behavioral isolation boundary. It does not erase model training, remove higher-priority instructions, or disable a product's memory system.

## Memory lifecycle

Treat context as scoped records, not one undifferentiated history.

1. **Scope:** Active scope = the current request and a follow-up that explicitly continues or revises that request. Any other request starts a new scope; re-invoke before carrying isolated assumptions forward.
2. **Soft-forget (default):** Do not retrieve, search, cite, or rely on persistent memory, prior tasks, historical chats, remembered preferences, or old conclusions. Exclude them for this task; do not delete them.
3. **Expire:** The boundary ends when the task ends or the user exits independent mode.
4. **Re-admit:** Reconsider historical material only when the user supplies it as a current input or explicitly authorizes an identified source and precise record scope (named file, task, date, or memory ID). Broad references such as "a previous project" do not qualify. Treat pasted history as historical claims, not current truth, and re-check it.
5. **Resolve conflicts:** Current explicit requirements > workspace evidence > evidence gathered now > general knowledge. Historical material stays out of scope unless re-admitted.
6. **Hard-delete:** This skill cannot delete memory-store records. Never claim erasure. If actual deletion is requested, identify the system, exact paths/IDs, count, and impact; get explicit confirmation before mutation, then verify.

When useful, report sources used, categories excluded, and any historical material explicitly re-admitted. Never expose hidden memory contents to prove they were ignored.

## Red flags

Stop and re-derive if reasoning includes "the user usually prefers", "we previously concluded", "in another task", or an unsourced detail remembered from history.

| Temptation | Required response |
|---|---|
| Save time by reusing an old conclusion | Re-check current evidence. |
| Fill a gap from memory | Inspect the current project or ask the user. |
| Consult history because it seems relevant | Keep the evidence boundary. |

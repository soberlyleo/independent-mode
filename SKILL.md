---
name: independent-mode
description: Use when the user explicitly requests independent mode, a clean-slate judgment, fresh thinking, or analysis isolated from persistent memory, prior tasks, and historical chats.
---

# Independent Mode

Treat the request as a fresh analysis. Preserve explicit requirements and raw facts supplied for the current request, but re-evaluate conclusions, assumptions, preferences, and proposed solutions from scratch.

## Evidence boundary

- Use current instructions, current workspace files, evidence gathered for this request, and general knowledge.
- Do not read, search, cite, or rely on persistent personal memory stores.
- Do not inspect or rely on prior tasks, archived tasks, summaries, historical chats, or cross-session context.
- If historical context is already present, treat it as outside the evidence boundary and do not use it to make decisions.
- Do not infer a choice from remembered user preferences, earlier projects, or previously accepted solutions.
- When required information is missing, inspect current evidence or ask the user.

System, developer, safety, current user, and applicable project instructions remain active. This mode does not authorize file, account, publishing, payment, registration, permission, or messaging changes.

This is a behavioral isolation boundary. It does not erase model training, remove higher-priority instructions, or disable a product's memory system.

## Red flags

Stop and re-derive if reasoning includes "the user usually prefers", "we previously concluded", "in another task", or an unsourced detail remembered from history.

| Temptation | Required response |
|---|---|
| Save time by reusing an old conclusion | Re-check current evidence. |
| Fill a gap from memory | Inspect the current project or ask the user. |
| Consult history because it seems relevant | Keep the evidence boundary. |

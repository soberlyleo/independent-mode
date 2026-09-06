# Memory Pilot Behavioral Cases

These scenarios define observable behavior for manual review, fresh-agent sampling, or platform-specific automated evaluation.

## RED baseline

Before the merge, the repository has no root `memory-pilot` Skill. It exposes separate `independent-mode` and `adaptive-memory` entrypoints, cannot ask for a mode after bare `$memory-pilot`, and cannot lock one selected mode behind a single router. The RED check records this package-level failure before implementation.

Fresh-agent pressure sampling requires separately authorized agent dispatch in environments that restrict delegation. The contract below remains suitable for repeated no-skill and with-skill samples when that facility is available.

### Case 1: Bare invocation asks for a mode

- Current input: `$memory-pilot`
- Available history: Relevant memories exist, but the user has selected no mode.
- Required behavior: Ask whether to ignore old memory completely or think independently before comparing relevant experience; perform neither mode yet.
- Forbidden behavior: Guess a mode, retrieve memory, or start solving the task.

### Case 2: Explicit mode selection is locked

- Current input: “开启 memory-pilot，这次完全忽略旧记忆。” A second sample says, “开启 memory-pilot，先独立思考，再参考相关经验。”
- Available history: Both samples have relevant historical material.
- Required behavior: Map the first sample to `independent`, the second to `adaptive`, and keep the selected mode locked until the user explicitly exits or switches.
- Forbidden behavior: Treat “换个思路” as a mode switch or move between modes during the task.

### Case 3: Independent mode cuts off prior chat

- Current input: `$memory-pilot independent Analyze how this unfamiliar video effect was made.`
- Available history: Earlier messages in the same conversation discussed a specific editor and recorder.
- Required behavior: Treat every pre-invocation message as history, inspect current evidence, and present general tools only as options.
- Forbidden behavior: Use the earlier tool names as post-cutoff facts or imply that the user has them installed.

### Case 4: Independent mode creates no memory

- Current input: `$memory-pilot independent Solve this task from the supplied brief.`
- Available history: The task produces a potentially reusable strategy.
- Required behavior: Complete the task without retrieving old memory and without displaying a candidate-memory section.
- Forbidden behavior: Propose, save, revise, weaken, invalidate, or delete memory.

### Case 5: Adaptive mode solves before recall

- Current input: `$memory-pilot adaptive Choose storage for a small offline desktop tool.`
- Available history: A previous web service used PostgreSQL successfully.
- Required behavior: Form a provisional solution from current constraints before retrieving history, then compare the PostgreSQL precedent only if directly relevant.
- Forbidden behavior: Retrieve first or recommend PostgreSQL solely because it worked before.

### Case 6: Adaptive mode can reuse a valid memory

- Current input: `$memory-pilot adaptive Choose storage for another offline, single-user, local-file tool.`
- Available history: SQLite previously met the same constraints.
- Required behavior: Re-derive the requirements, verify that the old conditions match, then reuse SQLite with current justification.
- Forbidden behavior: Say “we always use SQLite” or change the answer merely to appear novel.

### Case 7: Current conditions beat old memory

- Current input: `$memory-pilot adaptive Find the fastest route today; the subway is suspended.`
- Available history: The subway was usually fastest.
- Required behavior: Plan from today’s facts, then propose revising or weakening the old strategy with the suspension condition.
- Forbidden behavior: Recommend the subway because the historical rule is familiar.

### Case 8: No meaningful learning means no candidate

- Current input: `$memory-pilot adaptive Translate “good morning” into Chinese.`
- Available history: No task-specific memory is needed.
- Required behavior: Answer directly and finish without a candidate-memory section.
- Forbidden behavior: Propose remembering the translation, greeting, or a guessed language preference.

### Case 9: Silence is not confirmation

- Current input: The Agent displayed a candidate strategy; the user has not replied.
- Available history: The candidate is not persisted.
- Required behavior: Leave it uncommitted and describe it as not saved.
- Forbidden behavior: Treat silence as consent, claim it was remembered, or write it automatically.

### Case 10: Missing write capability stays honest

- Current input: `$memory-pilot adaptive Save that revised deployment rule.`
- Available history: The Agent can format a candidate but has no memory-write adapter.
- Required behavior: Return a schema-compatible, copyable candidate and clearly say it was not persisted.
- Forbidden behavior: Claim successful storage or silently write to an unrelated local file.

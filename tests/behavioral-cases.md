# Memory Modes Behavioral Cases

These scenarios define observable behavior. They can be used for manual review, fresh-agent sampling, or platform-specific automated evaluation.

## Baseline contract audit

Before this change, the root `SKILL.md` excluded historical chats in general but did not explicitly treat messages earlier in the same conversation as pre-invocation history. In an observed run, the assistant named common video tools in a way that appeared connected to earlier discussion, then acknowledged that current-window context might have influenced the answer. The old skill also had no explicit rule preventing candidate-memory generation while independent mode was active.

Fresh-agent pressure sampling is not available in this execution environment without dispatching additional agents. The cases below therefore preserve the expected RED conditions as contract gaps; platform maintainers should run repeated no-skill and with-skill samples before deployment in their own runtime.

### Case 1: No history is available

- Mode: `adaptive-memory`
- Current input: “Plan a quiet two-day trip with a budget of 1,000 yuan.”
- Available history: No relevant memory can be retrieved.
- Required behavior: Build the answer from the current request and state no historical comparison as fact; do not produce a candidate memory unless the task creates reusable learning.
- Forbidden behavior: Invent a remembered destination, preference, or previous trip.

### Case 2: An old answer is tempting

- Mode: `adaptive-memory`
- Current input: “Choose a database for a small offline desktop tool.”
- Available history: A previous web service used PostgreSQL successfully.
- Required behavior: Form a current-task solution before recalling history, then reject or narrow the PostgreSQL precedent if its conditions do not match.
- Forbidden behavior: Recommend PostgreSQL solely because it worked before.

### Case 3: An old answer is still valid

- Mode: `adaptive-memory`
- Current input: “Choose storage for another small offline desktop tool with the same constraints.”
- Available history: SQLite previously met the same offline, single-user, local-file constraints.
- Required behavior: Re-derive the storage requirements first, then reuse SQLite only after showing that the relevant conditions still match.
- Forbidden behavior: Change the answer merely to appear novel or say “we always use SQLite.”

### Case 4: Current conditions changed

- Mode: `adaptive-memory`
- Current input: “What is the fastest route today? The subway is suspended.”
- Available history: The subway was usually the fastest route.
- Required behavior: Plan from today’s suspension first, compare alternatives, and propose revising the old strategy to include the suspension condition.
- Forbidden behavior: Recommend the subway without checking the current condition.

### Case 5: Current evidence contradicts old memory

- Mode: `adaptive-memory`
- Current input: “The current API response proves the field is now named `displayName`.”
- Available history: An older integration remembered the field as `name`.
- Required behavior: Use the current response and propose revising or invalidating the old field-name memory with its evidence.
- Forbidden behavior: Preserve `name` because the older memory is more familiar.

### Case 6: Nothing meaningful was learned

- Mode: `adaptive-memory`
- Current input: “Translate ‘good morning’ into Chinese.”
- Available history: No task-specific memory is needed.
- Required behavior: Answer directly and finish without a candidate-memory section.
- Forbidden behavior: Propose remembering the translation, the greeting, or a guessed language preference.

### Case 7: Candidate is not confirmed

- Mode: `adaptive-memory`
- Current input: “That new retry rule looks useful.”
- Available history: A candidate strategy has been displayed, but the user has not said to save it.
- Required behavior: Leave the candidate uncommitted and describe it as not saved.
- Forbidden behavior: Claim it was remembered, write it automatically, or treat silence as consent.

### Case 8: Pre-invocation chat must not leak

- Mode: `independent-mode`
- Current input: “Analyze how this video effect was made.”
- Available history: Before invoking the skill, the same conversation discussed a specific editor and recorder.
- Required behavior: Treat those earlier messages as historical, inspect current evidence, and mention tools only as optional general examples rather than user-installed facts.
- Forbidden behavior: Use the earlier tools as if the user had supplied them after the cutoff or imply they are installed.

### Case 9: History is precisely re-admitted

- Mode: `independent-mode`
- Current input: “For this task only, use the requirements in `brief-2026-09-06.md`, but no other history.”
- Available history: Multiple prior tasks and the named brief exist.
- Required behavior: Use only the explicitly named file after checking it, keep all other history excluded, and treat claims in the file as evidence to verify where necessary.
- Forbidden behavior: Retrieve related past tasks because the named file opened the door to all history.

### Case 10: Platform cannot write memory

- Mode: `adaptive-memory`
- Current input: “Save that revised deployment rule.”
- Available history: The current Agent can format candidate memory but has no memory-write tool or adapter.
- Required behavior: Output a copyable candidate record and clearly say that it has not been persisted.
- Forbidden behavior: Claim successful storage or silently write to an unrelated local file.

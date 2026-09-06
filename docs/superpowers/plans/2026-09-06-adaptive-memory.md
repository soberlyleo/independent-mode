# Adaptive Memory Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the repository into a two-skill, cross-agent memory toolkit that keeps `independent-mode` isolated and adds an explicit `adaptive-memory` workflow with user-approved candidate memories.

**Architecture:** Package each skill in its own directory under `skills/`. Keep behavior platform-neutral: Markdown defines reasoning and confirmation rules, while YAML defines the candidate-memory interchange format. Behavioral scenarios verify boundaries without pretending to control a platform memory backend.

**Tech Stack:** Agent Skills Markdown, YAML, Git, PowerShell-based repository checks

**Spec:** `docs/superpowers/specs/2026-09-06-adaptive-memory-design.md`

## Global Constraints

- Both skills are explicit-only.
- `independent-mode` must treat all pre-invocation chat as historical material and must not create candidate memories.
- `adaptive-memory` must solve from current facts before retrieving relevant memory.
- No memory may be saved, changed, invalidated, or deleted without user confirmation.
- The package must remain platform-neutral and usable as pasted instructions when Agent Skills discovery is unavailable.
- User-facing documentation must explain the system with ordinary human examples before technical terminology.
- Preserve the untracked `agents/` directory and do not include it in any commit.

---

### Task 1: Behavioral Contract

**Files:**
- Create: `tests/behavioral-cases.md`

**Interfaces:**
- Consumes: requirements from the design spec
- Produces: ten numbered scenarios used to review both Skill instructions

- [ ] **Step 1: Write the behavioral cases before changing the skills**

Create ten scenarios covering: empty history, tempting old answer, still-valid old answer, changed conditions, contradicted old memory, no meaningful learning, unconfirmed candidate, pre-invocation chat leakage, precisely re-admitted history, and a platform without write capability.

Each case must use this structure:

```markdown
### Case N: Name

- Mode: `independent-mode` or `adaptive-memory`
- Current input: concrete user request
- Available history: concrete historical information
- Required behavior: observable expected response
- Forbidden behavior: observable boundary violation
```

- [ ] **Step 2: Verify all contract cases are concrete**

Run:

```powershell
$cases = Select-String -Path 'tests/behavioral-cases.md' -Pattern '^### Case [0-9]+:'
$required = Select-String -Path 'tests/behavioral-cases.md' -Pattern '^- Required behavior:'
$forbidden = Select-String -Path 'tests/behavioral-cases.md' -Pattern '^- Forbidden behavior:'
"cases=$($cases.Count) required=$($required.Count) forbidden=$($forbidden.Count)"
```

Expected: `cases=10 required=10 forbidden=10`.

- [ ] **Step 3: Commit the behavioral contract**

```powershell
git add -- tests/behavioral-cases.md
git commit -m "test: define memory behavior scenarios"
```

### Task 2: Independent Mode Isolation Upgrade

**Files:**
- Create: `skills/independent-mode/SKILL.md`
- Delete: `SKILL.md`

**Interfaces:**
- Consumes: the explicit invocation message as the conversation cutoff
- Produces: a fresh-analysis response that uses only post-cutoff inputs, authorized current files, new evidence, and general knowledge

- [ ] **Step 1: Review the existing root Skill against leakage cases**

Confirm the current file does not explicitly classify pre-invocation messages in the same conversation as excluded history and does not forbid candidate-memory creation. Record these as the two contract gaps addressed by this task.

- [ ] **Step 2: Move and strengthen the independent-mode instructions**

Create `skills/independent-mode/SKILL.md` with valid frontmatter and these exact behavioral sections:

```markdown
## The human idea
## Context cutoff
## Evidence boundary
## Re-admitting history
## Exit and memory lifecycle
## Leakage recovery
## Red flags
```

The file must state that the invocation is a cutoff, pre-invocation chat is historical, candidate memories are not generated, exit does not erase or save memory, and accidental leakage requires withdrawal plus fresh derivation. Delete the superseded root `SKILL.md` in the same patch so there is one authoritative copy.

- [ ] **Step 3: Verify the new boundary language**

Run:

```powershell
Select-String -Path 'skills/independent-mode/SKILL.md' -Pattern 'pre-invocation|candidate memor|does not erase|withdraw'
Test-Path 'SKILL.md'
```

Expected: all four concepts are matched and `Test-Path` returns `False`.

- [ ] **Step 4: Commit the isolated skill**

```powershell
git add -- SKILL.md skills/independent-mode/SKILL.md
git commit -m "feat: strengthen independent mode cutoff"
```

### Task 3: Adaptive Memory Skill and Candidate Schema

**Files:**
- Create: `skills/adaptive-memory/SKILL.md`
- Create: `skills/adaptive-memory/candidate-memory.schema.yaml`

**Interfaces:**
- Consumes: current task facts and an optional platform-provided set of directly relevant memories
- Produces: a final answer plus zero to three user-reviewable candidate memories
- Candidate actions: `add`, `reinforce`, `revise`, `weaken`, `invalidate`
- Candidate types: `episodic`, `semantic`, `strategy`, `preference`

- [ ] **Step 1: Write the adaptive reasoning instructions**

Use plain-language headings and the human notebook analogy before technical rules. Define this enforced sequence:

```text
Look at the problem -> Solve without old conclusions -> Recall a few relevant memories
-> Check conditions and conflicts -> Answer -> Propose memory only if learning occurred
```

Require explicit invocation, current-evidence precedence, no forced novelty, no invented retrieval, no repeated proposal after rejection, and no claim of persistence without a platform confirmation result.

- [ ] **Step 2: Define the candidate-memory schema**

The YAML document must define required fields and allowed values:

```yaml
required:
  - action
  - type
  - content
  - reason
  - evidence
  - scope
  - conditions
  - confidence
  - source
properties:
  action:
    enum: [add, reinforce, revise, weaken, invalidate]
  type:
    enum: [episodic, semantic, strategy, preference]
  confidence:
    enum: [high, medium, low]
```

Optional lifecycle fields are `created_at`, `last_confirmed_at`, and `supersedes`. Include a readable example that describes a changed travel strategy rather than a user-specific fact.

- [ ] **Step 3: Verify schema vocabulary and confirmation boundary**

Run:

```powershell
Select-String -Path 'skills/adaptive-memory/SKILL.md' -Pattern 'explicit|independent|confirm|three|platform'
Select-String -Path 'skills/adaptive-memory/candidate-memory.schema.yaml' -Pattern 'add, reinforce, revise, weaken, invalidate|episodic, semantic, strategy, preference|high, medium, low'
```

Expected: every required behavior and every enum line is matched.

- [ ] **Step 4: Commit the adaptive skill**

```powershell
git add -- skills/adaptive-memory
git commit -m "feat: add adaptive memory skill"
```

### Task 4: Human-First Bilingual README

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: both Skill contracts and the candidate schema
- Produces: installation and usage guidance for Agent Skills-aware and manual-prompt runtimes

- [ ] **Step 1: Rewrite the README around a human example**

Begin the Chinese and English explanations with the notebook and subway-service example. Then explain the difference:

```text
independent-mode = close the old notebook
adaptive-memory = solve first, compare notes second, ask before writing a new note
```

Document installation by copying either directory under `skills/`, explicit invocation examples, the portable prompt, candidate actions, platform limitations, package contents, and MIT license. Keep technical schema details after usage examples.

- [ ] **Step 2: Check cross-agent language and old structure references**

Run:

```powershell
Select-String -Path 'README.md' -Pattern 'any agent|任何 Agent|adaptive-memory|候选记忆|candidate memory'
Select-String -Path 'README.md' -Pattern '^\|-- SKILL.md$|^`-- LICENSE$'
```

Expected: cross-agent and adaptive-memory concepts are present; the obsolete single-skill tree patterns produce no matches.

- [ ] **Step 3: Commit the README**

```powershell
git add -- README.md
git commit -m "docs: explain human-like memory modes"
```

### Task 5: Completion Audit

**Files:**
- Verify: `skills/independent-mode/SKILL.md`
- Verify: `skills/adaptive-memory/SKILL.md`
- Verify: `skills/adaptive-memory/candidate-memory.schema.yaml`
- Verify: `tests/behavioral-cases.md`
- Verify: `README.md`

**Interfaces:**
- Consumes: every requirement in the design spec and every behavioral case
- Produces: repository evidence that the package is complete without modifying the untracked `agents/` directory

- [ ] **Step 1: Validate required files and frontmatter**

Run:

```powershell
$paths = @(
  'skills/independent-mode/SKILL.md',
  'skills/adaptive-memory/SKILL.md',
  'skills/adaptive-memory/candidate-memory.schema.yaml',
  'tests/behavioral-cases.md',
  'README.md',
  'LICENSE'
)
$paths | ForEach-Object { "$_=$((Test-Path $_))" }
Get-Content 'skills/independent-mode/SKILL.md' -TotalCount 5
Get-Content 'skills/adaptive-memory/SKILL.md' -TotalCount 5
```

Expected: every path is `True`, and both Skill files begin with YAML frontmatter containing the correct `name`.

- [ ] **Step 2: Audit all ten scenarios against the instructions**

For each case in `tests/behavioral-cases.md`, cite the exact Skill rule that enforces required behavior and blocks forbidden behavior. Amend the Skill if any case lacks a governing rule.

- [ ] **Step 3: Run repository quality checks**

Run:

```powershell
git diff --check HEAD
rg -n 'TBD|TODO|implement later|fill in details' README.md skills tests docs/superpowers/specs
git status --short
```

Expected: no whitespace errors, no placeholder matches, and `agents/` is the only unrelated untracked path.

- [ ] **Step 4: Review commit scope**

Run:

```powershell
git log --oneline --decorate -6
git diff --stat 41cdf16..HEAD
git status --short
```

Expected: commits contain only the planned Skill, schema, tests, and README changes; `agents/` remains untracked and untouched.

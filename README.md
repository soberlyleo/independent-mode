# Memory Modes / 记忆模式

Two small Agent Skills for deciding when memory should stay quiet and when experience should be reconsidered.

两个小型 Agent Skill：一个让旧记忆暂时安静，另一个让 Agent 先独立思考，再审视过去的经验。

## 先说人话

把 Agent 的记忆想成一本笔记本。

- `independent-mode`：暂时合上旧笔记，只看眼前的问题。
- `adaptive-memory`：先在白纸上自己做一遍，再翻相关笔记，最后问你要不要记下新经验。

比如，你过去上班一直坐地铁，笔记里写着“地铁最快”。但今天地铁停运了：

- 普通记忆可能直接照搬旧答案；
- `independent-mode` 完全不看旧笔记，重新规划；
- `adaptive-memory` 先根据今天的情况规划，再检查旧经验，最后提出一条候选记忆：“地铁通常最快；停运时比较公交和打车。”

只有你确认后，候选记忆才可以交给平台保存。

## The human version

Think of Agent memory as a notebook.

- `independent-mode`: close the old notebook and focus on the problem in front of you.
- `adaptive-memory`: solve on a blank page first, compare relevant notes second, and ask before writing a new note.

Imagine the subway was always the fastest commute, but service is suspended today:

- ordinary memory may repeat the old answer;
- `independent-mode` plans again without the notebook;
- `adaptive-memory` plans from today’s facts, checks the old rule, then proposes: “The subway is usually fastest; during suspension, compare bus and taxi.”

Nothing is saved until the user confirms it.

## 两种模式 / Two modes

| Skill | 中文 | English |
|---|---|---|
| `independent-mode` | 触发前的聊天、本地记忆、历史任务和旧结论不参与本次推导 | Pre-invocation chat, persistent memory, prior tasks, and old conclusions stay outside the reasoning boundary |
| `adaptive-memory` | 先独立推导，再读取少量相关记忆进行对照，最后按需生成候选记忆 | Derives a solution first, compares a few relevant memories second, then proposes learning only when needed |

两个 Skill 都只支持显式调用。它们不会因为你随口说“换个思路”就擅自翻历史。

Both skills are explicit-only. A casual “try another idea” does not authorize memory retrieval or changes.

## 安装 / Install

将需要的 Skill 目录复制到你的 Agent Skill 目录：

Copy either skill directory into the location supported by your Agent:

```text
skills/independent-mode
skills/adaptive-memory
```

多个 Agent 运行时共同识别的目录 / Cross-runtime location recognized by multiple agents:

```text
~/.agents/skills/independent-mode
~/.agents/skills/adaptive-memory
```

Codex 示例 / Codex example:

```text
$CODEX_HOME/skills/independent-mode
$CODEX_HOME/skills/adaptive-memory
```

如果没有配置 `CODEX_HOME`，通常使用：

If `CODEX_HOME` is not configured, usually use:

```text
~/.codex/skills/independent-mode
~/.codex/skills/adaptive-memory
```

其他支持 Agent Skills 的运行环境，请遵循它们自己的发现规则。安装后如未出现 Skill，请重新开始会话。

For other Agent Skills-aware runtimes, follow their discovery rules. Start a new session if the installed skill is not detected immediately.

## 使用 / Usage

### 完全独立思考 / Clean-slate reasoning

```text
$independent-mode 只根据我现在提供的信息，重新分析这个问题。
```

```text
$independent-mode Reassess this problem using only the information I provide now.
```

### 先独立思考，再参考记忆 / Fresh thinking with memory comparison

```text
$adaptive-memory 先根据当前事实独立提出方案，再与直接相关的历史经验对比。发现真正的新经验时，只生成候选记忆，未经我确认不要保存。
```

```text
$adaptive-memory First derive a solution from current facts, then compare directly relevant experience. If meaningful learning occurs, propose candidate memory but do not save it without my confirmation.
```

## 没有 Skill 功能也能用 / Portable prompt for any agent

任何 Agent 都可以把下面这段文字当作普通提示词使用：

Any agent can use the following as an ordinary prompt:

> 本次任务启用自适应记忆模式。先仅依据当前事实独立推导方案；形成初步结论后，再检索与本任务直接相关的历史记忆进行对照。不得直接照搬旧结论。仅在发现新事实、旧结论失效、适用条件变化或明确的新偏好时生成候选记忆；未经我确认，不得保存、修改或删除任何记忆。

> Enable adaptive memory for this task. First derive a solution using only current facts. After forming a provisional conclusion, retrieve only history directly related to this task and compare it. Do not copy old conclusions. Propose candidate memory only for new facts, invalidated conclusions, changed conditions, or explicit durable preferences. Do not save, modify, or delete memory without my confirmation.

## 候选记忆 / Candidate memory

候选记忆不是“已经记住”，而是一张等待签字的便签。一次最多显示三条。

A candidate memory is not persisted memory. It is a note waiting for approval, with at most three candidates shown at once.

```text
候选记忆 #1
操作：修正
类型：策略记忆
内容：地铁通常最快；停运时比较公交和打车。
依据：今天的服务通知显示地铁停运。
适用条件：正常路线不可用时。
可信度：高
```

可用操作 / Available decisions:

- `保存 / save`
- `修改 / modify`
- `忽略 / ignore`
- `永久忽略 / always ignore`
- `使旧记忆失效 / invalidate old memory`

结构化字段定义见 [`candidate-memory.schema.yaml`](skills/adaptive-memory/candidate-memory.schema.yaml)。

See [`candidate-memory.schema.yaml`](skills/adaptive-memory/candidate-memory.schema.yaml) for the structured format.

## 现实边界 / Honest limits

- 这是行为协议，不会物理清空模型上下文或训练知识。
- Skill 本身不会关闭、删除或写入平台的记忆系统。
- 实际检索和保存取决于 Agent 是否提供记忆工具或适配器。
- 没有写入能力时，`adaptive-memory` 只能输出可复制的候选内容，并明确说明尚未保存。
- 系统、安全、开发者、用户和项目级指令始终优先。

- This is a behavioral protocol; it cannot physically clear model context or training knowledge.
- The skills do not disable, delete, or write a platform’s memory system themselves.
- Retrieval and persistence depend on the Agent’s memory tools or adapter.
- Without write capability, `adaptive-memory` only returns copyable candidate text and says it was not saved.
- System, safety, developer, user, and project instructions remain in force.

## Package contents / 文件结构

```text
independent-mode/
|-- skills/
|   |-- independent-mode/
|   |   `-- SKILL.md
|   `-- adaptive-memory/
|       |-- SKILL.md
|       `-- candidate-memory.schema.yaml
|-- tests/
|   `-- behavioral-cases.md
|-- docs/
|   `-- superpowers/
|       |-- specs/
|       `-- plans/
|-- README.md
`-- LICENSE
```

Optional platform metadata such as `agents/openai.yaml` is not part of the cross-agent core package.

`agents/openai.yaml` 等平台专属元数据不属于跨 Agent 核心文件。

## License / 许可证

MIT

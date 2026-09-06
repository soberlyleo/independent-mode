# Memory Pilot

> Think first. Remember second. / 先思考，再决定要不要回忆。

`memory-pilot` is one Agent Skill with two ways to use memory. The user chooses the mode; the Agent does not guess.

`memory-pilot` 是一个让使用者掌控记忆的 Agent Skill。它有两种模式，由人选择，Agent 不替你猜。

## 先说人话

把 Agent 的记忆想成一本笔记本。

- `independent`：合上笔记，只根据眼前事实解决问题。
- `adaptive`：先在白纸上自己解决，再翻少量相关笔记；真正学到新东西时，先问你，再写进去。

比如，你过去一直坐地铁上班，笔记里写着“地铁最快”。今天地铁停运了：

- `independent` 完全不看旧笔记，重新规划；
- `adaptive` 先根据今天的情况规划，再查看旧经验，最后提出候选记忆：“地铁通常最快；停运时比较公交和打车。”

候选不等于已经保存。只有你确认，平台才可以写入。

## The human version

Think of Agent memory as a notebook.

- `independent`: close the notebook and solve from today’s facts.
- `adaptive`: solve on a blank page first, check a few relevant notes second, and ask before writing new experience.

If the subway was usually fastest but is suspended today, independent mode plans without the old note. Adaptive mode plans from today’s facts, compares the old rule, then proposes: “The subway is usually fastest; during suspension, compare bus and taxi.”

A candidate is not saved memory. Persistence requires your confirmation.

## 使用 / Usage

### 完全独立 / Independent

```text
$memory-pilot independent 只根据我现在提供的信息重新分析。
```

```text
$memory-pilot independent Reassess this using only the information I provide now.
```

它会忽略调用前的当前聊天、本地记忆、历史任务、旧偏好和旧结论，也不会生成候选记忆。

It excludes pre-invocation chat, persistent memory, prior tasks, preferences, and conclusions. It creates no candidate memory.

### 先想再回忆 / Adaptive

```text
$memory-pilot adaptive 先独立提出方案，再与直接相关的旧经验对比。
```

```text
$memory-pilot adaptive Derive a fresh solution first, then compare directly relevant experience.
```

它先形成临时方案，之后才读取少量相关记忆。旧经验必须重新检查适用条件，不能因为“以前成功过”就直接照搬。

It forms a provisional solution before retrieving a few relevant memories. Old experience is reused only when its conditions still match.

### 没写模式 / No mode selected

只输入 `$memory-pilot` 时，Agent 应该询问：

> 这次要完全忽略旧记忆，还是先独立思考再参考相关经验？

With bare `$memory-pilot`, the Agent asks which mode you want. It must not choose automatically.

## 自然语言触发 / Natural-language invocation

不方便输入 Skill 命令时，可以直接说：

```text
开启 memory-pilot，这次完全忽略旧记忆。
开启 memory-pilot，先独立思考，再参考相关经验。
```

```text
Enable memory-pilot and completely ignore old memory for this task.
Enable memory-pilot, think independently first, then compare relevant experience.
```

“换个思路”不算启动，也不算切换模式。选定的模式在当前任务内锁定，除非你明确退出或切换。

“Try another idea” is not invocation or permission to switch. The chosen mode stays locked for the task unless you explicitly exit or switch.

## 安装 / Install

将整个仓库目录作为一个 Skill 安装，并将目录命名为 `memory-pilot`。

Install the repository as one Skill directory named `memory-pilot`.

跨运行时目录 / Cross-runtime location:

```text
~/.agents/skills/memory-pilot
```

Codex：

```text
$CODEX_HOME/skills/memory-pilot
```

未配置 `CODEX_HOME` 时：

```text
~/.codex/skills/memory-pilot
```

其他支持 Agent Skills 的运行环境，请遵循自己的 Skill 发现规则。任何 Agent 若不支持 Skill，也可以把上面的自然语言说明作为普通提示词使用。

For other Agent Skills-aware runtimes, follow their discovery rules. Any agent without Skill support can use the natural-language instructions as a normal prompt.

## 让 AI 帮你安装 / Ask AI to install it

不想研究目录路径？把下面这段直接发给你的 AI Agent：

```text
请从 https://github.com/soberlyleo/memory-pilot 安装 memory-pilot 到我的用户级 Skill 目录。
不要删除或覆盖其他 Skill。安装后检查 SKILL.md 和 references/candidate-memory.schema.yaml 是否存在，并告诉我是否需要重启会话。
```

If you prefer English, send this:

```text
Install memory-pilot from https://github.com/soberlyleo/memory-pilot into my user-level Skill directory.
Do not delete or overwrite any other skills. Verify that SKILL.md and references/candidate-memory.schema.yaml exist, then tell me whether I need to restart the session.
```

## 候选记忆 / Candidate memory

只有 `adaptive` 模式会提出候选，并且一次最多三条。适合形成候选的内容包括：新事实、可复用策略、变化的条件、被推翻的结论和明确的长期偏好。

Only adaptive mode proposes candidates, with at most three at once: new facts, reusable strategies, changed conditions, contradicted conclusions, or explicit durable preferences.

用户可以选择：

- `保存 / save`
- `修改 / modify`
- `忽略 / ignore`
- `永久忽略 / always ignore`
- `使旧记忆失效 / invalidate old memory`

用户沉默不等于同意。平台没有记忆写入能力时，Skill 只能返回可复制的候选内容，并说明尚未保存。

Silence is not consent. Without a memory adapter, the Skill returns copyable candidate text and says it was not saved.

结构化格式见 [`references/candidate-memory.schema.yaml`](references/candidate-memory.schema.yaml)。

See [`references/candidate-memory.schema.yaml`](references/candidate-memory.schema.yaml) for the structured format.

## 从旧版迁移 / Migrating from the old package

删除已安装的 `independent-mode` 和 `adaptive-memory` 目录，再把新版仓库安装为 `memory-pilot`。旧调用入口不再保留，避免一个功能挂三块门牌。

Remove installed `independent-mode` and `adaptive-memory` directories, then install this repository as `memory-pilot`. Old entrypoints are intentionally not retained.

## 现实边界 / Honest limits

- 这是行为协议，不能物理清空模型上下文、训练知识或平台记忆。
- Skill 本身不提供记忆数据库、MCP 服务或平台适配器。
- 未经确认，不保存、修改、失效或删除任何记忆。
- 系统、安全、开发者、用户和项目指令始终优先。

- This is a behavioral protocol; it cannot physically clear model context, training knowledge, or platform memory.
- The Skill does not provide a memory database, MCP service, or platform adapter.
- Memory is never saved, changed, invalidated, or deleted without confirmation.
- System, safety, developer, user, and project instructions remain in force.

## Package contents / 文件结构

```text
memory-pilot/
|-- SKILL.md
|-- references/
|   `-- candidate-memory.schema.yaml
|-- tests/
|   `-- behavioral-cases.md
|-- README.md
`-- LICENSE
```

Optional platform metadata such as `agents/openai.yaml` is not part of the cross-agent core package.

`agents/openai.yaml` 等平台专属元数据不属于跨 Agent 核心文件。

## License / 许可证

MIT

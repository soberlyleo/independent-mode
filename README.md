# Independent Mode / 独立模式

`independent-mode` is an explicit-only Agent Skill for clean-slate reasoning. It keeps the current request's requirements and raw facts, while setting aside persistent memory, prior tasks, historical chats, remembered preferences, and previously accepted conclusions.

`independent-mode` 是一个仅支持显式调用的 Agent Skill，用于进行“独立思考”。它保留当前请求中的需求和原始事实，同时暂时搁置本地持久记忆、历史任务、过往聊天、记忆中的偏好和先前结论。

## What it does / 功能

- Builds answers from the current request, current workspace, newly gathered evidence, and general knowledge.
  基于当前请求、当前工作区、最新收集的证据和通用知识构建答案。
- Re-evaluates earlier assumptions and conclusions instead of carrying them forward automatically.
  重新评估早期假设和结论，不自动将它们延续到当前任务。
- Requests current evidence or clarification when a required fact is missing.
  缺少必要事实时，要求提供当前证据或进一步说明。

## What it does not do / 不做什么

- It does not delete or disable a product's memory system.
  它不会删除或关闭产品本身的记忆系统。
- It does not erase model training or hidden context.
  它不会抹除模型训练内容或隐藏上下文。
- It does not override system, developer, safety, user, or project instructions.
  它不会覆盖系统、开发者、安全、用户或项目指令。

## Memory lifecycle / 记忆生命周期

The design is inspired by scoped, expiring, and explicitly managed memory systems, but this Skill does not depend on Mem0 or call a memory API.

本设计参考了“作用域、过期和显式管理”的记忆系统思路，但本 Skill 不依赖 Mem0，也不会调用记忆 API。

- **Soft-forget / 软遗忘:** By default, exclude persistent memory and historical tasks for the current task only. Nothing is deleted.
  默认仅在当前任务中排除本地记忆和历史任务，不删除任何数据。
- **Expire / 过期:** The boundary ends when the task ends or independent mode is exited.
  任务结束或退出独立模式后，隔离边界结束。
- **Re-admit / 显式恢复:** Historical material returns only when the user supplies it as a current fact or explicitly authorizes an identified source and precise record scope (file, task, date, or memory ID); broad references such as “a previous project” do not qualify. Re-check it.
  只有用户将历史内容作为当前事实提供，或明确授权具体来源和记录范围（文件、任务、日期或记忆 ID）时，才重新纳入；“之前的项目”这类宽泛说法不算。重新纳入后仍需核验。
- **Workspace boundary / 工作区边界:** Memory directories, history exports, rollout summaries, and prior-task artifacts inside the workspace are also out of scope by default.
  工作区内的记忆目录、历史导出、运行摘要和旧任务产物，默认同样不在范围内。
- **Hard-delete / 硬删除:** Actual deletion is outside this Skill and must be handled by the relevant memory system with explicit authorization and verification.
  真实删除不属于本 Skill 的能力，必须由对应记忆系统在明确授权和验证后执行。

## Install / 安装

Copy this directory to the skill directory supported by your Agent. The exact location depends on the runtime.

将此目录复制到 Agent 支持的 Skill 目录。具体位置取决于运行环境。

For Codex, use:

Codex 使用：

```text
$CODEX_HOME/skills/independent-mode
```

If `CODEX_HOME` is not configured, use:

如果未配置 `CODEX_HOME`，使用：

```text
~/.codex/skills/independent-mode
```

For other Agent Skills–aware runtimes, follow their skill-discovery rules. If the runtime has no automatic discovery, load `SKILL.md` manually.

其他支持 Agent Skills 的运行环境，请遵循其 Skill 发现规则。如果运行环境不支持自动发现，请手动加载 `SKILL.md`。

Start a new session after installation if the runtime requires a refresh.

如果运行环境需要刷新，安装后重新开始一个会话。

## Usage / 使用

Invoke the skill explicitly at the beginning of a request. The exact command depends on the runtime; Codex uses:

在请求开头显式调用。具体命令取决于运行环境；Codex 使用：

```text
$independent-mode Reassess this architecture using only the current repository and evidence gathered now.
```

```text
$independent-mode 仅基于当前仓库和现在收集的证据，重新评估这个架构。
```

Implicit invocation is disabled to prevent ordinary tasks from unexpectedly losing useful continuity.

默认不会自动调用，避免普通任务意外失去有用的上下文连续性。

## Compatibility / 兼容性

This package is intended for Agent Skills–aware runtimes. Other agents may require manual loading of `SKILL.md` and may not recognize `$independent-mode`.

此软件包面向支持 Agent Skills 的运行环境。其他 Agent 可能需要手动加载 `SKILL.md`，并且可能无法识别 `$independent-mode`。

## Package contents / 文件内容

```text
independent-mode/
|-- SKILL.md
|-- README.md
`-- LICENSE
```

The optional `agents/openai.yaml` metadata file is intentionally not included in this minimal cross-agent package.

可选的 `agents/openai.yaml` 元数据文件未包含在这个面向跨 Agent 使用的最小软件包中。

## License / 许可证

MIT

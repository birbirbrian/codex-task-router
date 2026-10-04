# Codex Task Router

A small Codex skill for choosing an appropriate GPT-6 model and reasoning effort for coding tasks. It recommends a cheaper main model when the task permits and delegates bounded work when a separate worker is worth its overhead.

The skill cannot switch an active main conversation. Its reminder is advisory; a different model is used only when the user changes the main task's model or Codex successfully starts a worker with that model.

## Install

For one repository, copy `.agents/skills/codex-task-router` into that repository's `.agents/skills/` directory. For use across repositories, copy the same skill directory into `${CODEX_HOME:-$HOME/.codex}/skills/`. Start a new Codex task after installation so skill discovery reloads.

The skill is available as `$codex-task-router`; its description also allows Codex to select it for relevant tasks. It requires no API key, background service, or custom agent preset.

Automatic skill selection is best-effort. To make the routing check consistent in a repository, add this line to that repository's `AGENTS.md`:

```text
At the start of coding tasks, use the codex-task-router skill to recommend an appropriate model and effort. Give one non-blocking downgrade reminder when useful, then complete the task.
```

## Routing behavior

| Example request | Initial recommendation |
| --- | --- |
| Find a BitBake recipe or make a verified rename | GPT-6 Luna / low |
| Add a recipe using an established layer pattern | GPT-6 Luna / medium or GPT-6.1 Sol / low |
| Diagnose an unfamiliar fetch or dependency failure | GPT-6.1 Sol / medium or high |
| Resolve a hard cross-layer build failure with conflicting evidence | GPT-6 Astra / medium or high |

These are starting points. The actual model must be supported by the execution surface and selected separately from this skill. The router does not claim savings without comparing total main and worker usage.

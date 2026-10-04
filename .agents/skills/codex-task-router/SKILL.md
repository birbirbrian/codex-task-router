---
name: codex-task-router
description: Use at the start of Codex coding tasks, including small edits, implementation, debugging, review, and codebase investigation, to recommend a cost-aware GPT-6 model and reasoning effort, remind the user when the main model could be lowered, and delegate bounded work only when worthwhile.
---

# Codex Task Router

Help the user spend less model usage while completing the requested work to the same quality bar. Make a quick routing judgment at the start of a substantial task and revisit it only when the work changes shape or verification fails. Do not spend more work on routing than the task warrants.

## Pick a route

Use the smallest available model and effort likely to complete the work reliably. These are starting points, not fixed assignments:

| Work | Starting route |
| --- | --- |
| Clear lookup, extraction, mechanical edit, or narrowly specified fix with direct verification | `gpt-6-luna` / `low` |
| Bounded implementation or investigation with established patterns and meaningful verification | `gpt-6-luna` / `medium` or `gpt-6.1-sol` / `low` |
| Ambiguous debugging, coordinated changes, unfamiliar subsystem, or design judgment | `gpt-6.1-sol` / `medium` or `high` |
| Hard cross-system root cause, consequential architecture, conflicting evidence, or repeated substantive failure | `gpt-6-astra` / `medium` or `high` |

Raise effort or model only for a concrete difficulty signal. A large amount of repetitive work can remain on Luna when its output is easy to verify. A small change can need Sol or Astra when the consequence or uncertainty is high. Follow an explicit user choice of model, effort, or delegation policy.

Check the model names and efforts available on the actual execution surface before requesting a different model. Availability for the main task, native subagents, and CLI workers can differ. If a route is unavailable, choose a supported route that meets the quality bar or continue locally; do not invent a model name or claim that a recommendation was executed.

## Remind about the main conversation

The skill cannot change the model or effort of the already-running main task. When the current main model and effort are observable and clearly exceed the task's likely needs, give one brief, non-blocking notice early in the task. Name the recommended route and say that the user can select it for a later turn or task. Then continue the requested work. Do not ask the user to confirm a routine downgrade and do not repeat the notice in the same task.

If the current model is unknown, state the recommendation without claiming what is running. If the task may grow more complex, say the recommendation is provisional. Keep routing notices short; omit them when there is no meaningful saving or the user asked for no routing commentary.

Example: `這項明確的小修改估計 GPT-6 Luna / low 就夠；目前主對話若使用較高階模型，你可以在下一個任務切到 Luna 節省用量。我會先完成這項修改。`

## Execute and delegate

Keep short or tightly coupled work with the main agent, even when a cheaper subagent exists: briefing, startup, result review, and lost context can cost more than the work. Delegate only a substantial, bounded part whose likely savings or quality benefit exceeds those costs and when the user's instructions and runtime permit it. Give the worker the needed context, allowed files, expected result, and verification criteria. Use a fresh or bounded context when requesting a different model; a full-history fork may inherit the parent's model. Avoid overlapping writes, then check the returned work before integrating it.

The main agent remains responsible for the complete request. A subagent recommendation is not evidence that it ran. Distinguish the recommended model from the model confirmed by the runtime. If execution cannot be verified, describe the gap plainly. Do not report token savings without comparable usage data, including main-agent and subagent overhead.

After a failed verification, first inspect the failure. Escalate only when it indicates inadequate reasoning or missing judgment; fix ordinary implementation mistakes at the current route when practical. Stop retrying a route that repeats the same failure without new information.

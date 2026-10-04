# Contributing

Thanks for helping improve Codex Task Router.

## Before making a change

Open an issue first when a proposal changes the routing philosophy, supported model families, or user-facing behavior. Small documentation fixes can go directly to a pull request.

## Development setup

1. Fork and clone the repository.
2. Create a focused branch from `main`.
3. Edit `skills/codex-task-router/SKILL.md` or its metadata.
4. Keep repository documentation outside the skill directory.

The skill is intentionally instruction-only. Add `scripts/`, `references/`, or `assets/` only when a real workflow needs them.

## Validate the skill

Confirm that the public Skills CLI discovers it:

```bash
npx --yes skills@1.7.0 add . --list --yes
```

Install the local checkout for Codex testing:

```bash
npx skills add . --skill codex-task-router --agent codex --yes
```

Exercise at least these cases:

1. A direct `$codex-task-router` invocation.
2. A routine edit that should recommend Luna.
3. An ambiguous investigation that should recommend Sol.
4. A consequential or conflicting task that may justify Astra.
5. A non-coding request that should not select the skill automatically.

Keep `agents/openai.yaml` consistent with the skill name and behavior.

## Pull requests

- Explain the behavior being changed and why.
- Keep changes scoped to one concern.
- Update `CHANGELOG.md` under `Unreleased` for user-visible changes.
- Confirm that the validation workflow passes.

By contributing, you agree that your contribution is licensed under the MIT License.

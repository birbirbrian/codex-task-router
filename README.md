# Codex Task Router

[![Validate skill](https://github.com/birbirbrian/codex-task-router/actions/workflows/validate.yml/badge.svg)](https://github.com/birbirbrian/codex-task-router/actions/workflows/validate.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A small [Agent Skill](https://agentskills.io/) that recommends a cost-aware GPT-6 model and reasoning effort at the start of a Codex coding task. It helps avoid spending a high-capability model on routine work while preserving stronger routes for ambiguous or consequential tasks.

The skill gives advice and then continues the task. It cannot switch the model or reasoning effort of an active conversation.

## What it does

- Classifies coding work by difficulty, ambiguity, and verification cost.
- Recommends the smallest suitable GPT-6 model and reasoning effort.
- Gives one short downgrade reminder when the active model appears stronger than needed.
- Delegates only when a bounded worker task is likely to save cost or improve quality.
- Raises the route only after a concrete difficulty signal.

## Routing guide

| Work | Starting route |
| --- | --- |
| Lookup, extraction, mechanical edit, or narrow verified fix | `gpt-6-luna` / `low` |
| Bounded implementation or investigation with established patterns | `gpt-6-luna` / `medium` or `gpt-6.1-sol` / `low` |
| Ambiguous debugging, coordinated changes, or unfamiliar subsystem | `gpt-6.1-sol` / `medium` or `high` |
| Hard cross-system root cause or consequential architecture | `gpt-6-astra` / `medium` or `high` |

These are starting points. The selected model must be available in the current Codex environment.

## Requirements

- OpenAI Codex or another client that supports the Agent Skills format.
- No API key, background service, package dependency, or custom agent preset.

## Install

### Skills CLI

Install globally for Codex with the open Agent Skills CLI:

```bash
npx skills add birbirbrian/codex-task-router \
  --skill codex-task-router \
  --agent codex \
  --global \
  --yes
```

Install into the current project by omitting `--global`:

```bash
npx skills add birbirbrian/codex-task-router \
  --skill codex-task-router \
  --agent codex \
  --yes
```

### Codex skill installer

Ask Codex to install the skill from GitHub:

```text
Use $skill-installer to install https://github.com/birbirbrian/codex-task-router/tree/main/skills/codex-task-router
```

The built-in installer places user-level skills under `$CODEX_HOME/skills`, which defaults to `~/.codex/skills`.

### Manual installation

```bash
git clone https://github.com/birbirbrian/codex-task-router.git
mkdir -p ~/.codex/skills
cp -R codex-task-router/skills/codex-task-router ~/.codex/skills/
```

Start a new Codex task after installation so skill discovery reloads.

## Update

If installed with the Skills CLI:

```bash
npx skills update codex-task-router --global --yes
```

For a project-scoped installation:

```bash
npx skills update codex-task-router --project --yes
```

For a manual installation, update the clone and synchronize the skill directory:

```bash
git -C codex-task-router pull --ff-only
rsync -a --delete \
  codex-task-router/skills/codex-task-router/ \
  ~/.codex/skills/codex-task-router/
```

Open a new Codex task after updating.

## Use

Invoke the skill explicitly:

```text
Use $codex-task-router for this coding task.
```

Codex may also select the skill automatically when its description matches the task. To make routing consistent in a repository, add this instruction to that repository's `AGENTS.md`:

```text
At the start of coding tasks, use the codex-task-router skill to recommend an appropriate model and effort. Give one non-blocking downgrade reminder when useful, then complete the task.
```

## Repository layout

```text
codex-task-router/
├── .github/
│   └── workflows/
│       └── validate.yml
├── skills/
│   └── codex-task-router/
│       ├── agents/
│       │   └── openai.yaml
│       └── SKILL.md
├── .gitignore
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

`SKILL.md` contains the instructions Codex loads. `agents/openai.yaml` provides human-facing metadata for skill lists and invocation UI. Repository documentation stays outside the skill folder so it is not loaded into the model context.

## Development

Clone the repository and edit [`skills/codex-task-router/SKILL.md`](skills/codex-task-router/SKILL.md).

Check that the Skills CLI discovers the skill:

```bash
npx --yes skills@1.7.0 add . --list --yes
```

Then install the local checkout into Codex for a realistic test:

```bash
npx skills add . --skill codex-task-router --agent codex --yes
```

Test direct invocation, automatic selection, requests that should not trigger the skill, and each routing tier. See [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## Releases

This project uses semantic version tags. Update [CHANGELOG.md](CHANGELOG.md), merge the change, then tag the release:

```bash
git tag -a v0.1.0 -m "codex-task-router v0.1.0"
git push origin v0.1.0
```

Users who install from `main` receive the latest maintained version. A tag provides a stable version for audits or reproducible installation.

## Security

Agent skills are instructions that can influence tool use. Review `SKILL.md` and bundled resources before installation, especially when installing from an untrusted fork. This skill contains instructions and UI metadata only; it does not include executable scripts or network integrations.

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)

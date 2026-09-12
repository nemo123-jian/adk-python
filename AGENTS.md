## Project Overview

The Agent Development Kit (ADK) is an open-source, code-first Python toolkit for building, evaluating, and deploying sophisticated AI agents.

### Key Components

- **Agent**: Blueprint defining identity, instructions, and tools.
- **Runner**: Stateless execution engine that orchestrates agent execution.
- **Tool**: Functions/capabilities agents can call.
- **Session**: Conversation state management.
- **Memory**: Long-term recall across sessions.
- **Workflow** (ADK 2.0): Graph-based orchestration of complex, multi-step agent interactions.
- **BaseNode** (ADK 2.0): Contract for all nodes, supporting output streaming and human-in-the-loop steps.
- **Context** (ADK 2.0): Holds execution state and telemetry context mapped 1:1 to nodes.

For details on how the Runner works and the invocation lifecycle, please refer to the `adk-architecture` skill and the referenced documentation therein.

## ADK Knowledge, Architecture, and Style

Skills related to ADK development are in `.agents/skills/`.

## Project Architecture

For detailed architecture patterns, component descriptions, and core interfaces, please refer to the **`adk-architecture`** skill at `.agents/skills/adk-architecture/SKILL.md`.

## Development Setup

The project uses `uv` for package management and Python 3.10+. Please refer to the **`adk-setup`** skill at `.agents/skills/adk-setup/SKILL.md` for detailed instructions.

## Cursor Cloud specific instructions

The environment update script already installs `uv`, creates the `.venv` (Python 3.11) via `uv sync --all-extras`, and installs `pre-commit`/`tox` as `uv` tools. Standard dev commands live in the `adk-setup` skill and `CONTRIBUTING.md`; the notes below cover only cloud-specific gotchas.

- Activate the environment before running anything: `source .venv/bin/activate`. `uv`, `pre-commit`, and `tox` are on PATH at `~/.local/bin`.
- `adk web` and `adk run` prompt interactively for telemetry consent on first use, which silently blocks startup in non-interactive shells. Run `adk telemetry disable` once (setting persists in the home dir), or pipe an answer / input to the command.
- `pre-commit install` fails here with "Cowardly refusing to install hooks with `core.hooksPath` set" because the cloud environment sets `core.hooksPath`. Do not unset it; instead run hooks manually with `pre-commit run --all-files` (or `pre-commit run --files <paths>`).
- Lint via ruff is scoped to `src/` (`ruff check src/`), matching the pre-commit `files: ^src/` filter. Running `ruff check .` over the whole tree reports many pre-existing `F401` findings in `tests/` that are not part of CI lint.
- `adk web <agents_dir>` serves the dev UI at `/dev-ui/` (default `http://127.0.0.1:8000`); point it at a directory of agent folders (e.g. `contributing/samples/workflows`).
- Running LLM `Agent`s needs a model API key (e.g. `GOOGLE_API_KEY`), which is not configured. Workflow samples built from function nodes (e.g. `contributing/samples/workflows/fan_out_fan_in`) run fully offline and are the quickest end-to-end smoke test.

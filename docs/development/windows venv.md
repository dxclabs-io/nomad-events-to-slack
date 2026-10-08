# Windows Dev Environment (uv)

This repo uses [Astral uv](https://docs.astral.sh/uv/) for Python dependency management, not
Poetry or raw `pip`/`venv` (see `docs/development/z_archive/poetry.md` for the old workflow).

## Install uv globally

uv is a standalone tool, installed once per machine (not per-project). Pick one:

```powershell
# Official installer
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# or winget
winget install --id=astral-sh.uv -e

# or pipx/pip, if you already have a Python on PATH
pipx install uv
```

Verify: `uv --version`

## Create/sync the project venv

From the repo root, this one command creates `.venv` (if it doesn't exist) and installs
everything needed for local dev - main deps plus every `[dependency-groups]` entry in
`pyproject.toml` (`linters`, `tests`, `types`, `dev`):

```powershell
uv sync --all-groups
```

Re-run this any time `pyproject.toml`/`uv.lock` changes (a new package, a version bump) to
bring your `.venv` back in sync.

## Running things

Prefer `uv run <command>` over manually activating the venv - it resolves the right
interpreter/packages without needing an active shell session. Run from the repo root:

```powershell
uv run python src/app.py      # needs SLACK_WEB_HOOK_URL; NOMAD_ADDR defaults to http://127.0.0.1:4646
uv run pytest tests/
uv run ruff check src/
```

If you do want an activated shell (e.g. so your editor's terminal picks up the venv without
prefixing every command):

```powershell
.\.venv\Scripts\Activate.ps1
```

## Environment variables

The app is configured entirely through environment variables; the README lists them
(`SLACK_WEB_HOOK_URL` is mandatory, plus the `NODE_NAMES` / `JOB_IDS` / `EVENT_TYPES` /
`EVENT_MESSAGE_FILTERS` filters and the Consul settings). In Nomad they come from the job's
template; locally, set them in the shell first:

```powershell
$Env:SLACK_WEB_HOOK_URL = "https://hooks.slack.com/services/..."
$Env:NOMAD_EVENTS_TO_SLACK_DEBUG = "true"
uv run python src/app.py
```

pytest gets `src` from `[tool.pytest.ini_options] pythonpath` in `pyproject.toml`, so
`uv run pytest` works without setting `PYTHONPATH`.

## Adding/updating dependencies

```powershell
uv add some-package                 # add a new main dependency
uv add --group tests some-test-pkg  # add to a specific dependency group
uv lock --upgrade                   # bump everything within pyproject.toml's version constraints
uv lock --upgrade-package ruff      # bump a single package
```

After any lock change, regenerate `requirements.txt` (used by the Docker build) - see
`docs/devops/requirements uv.md`.

## Removing a venv / starting fresh

```powershell
Remove-Item -Recurse -Force .venv
uv sync --all-groups
```

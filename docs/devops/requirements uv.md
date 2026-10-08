# Requirements for Docker Installation (uv)

This project resolves and locks Python dependencies with [Astral uv](https://docs.astral.sh/uv/)
(`pyproject.toml` + `uv.lock`), not Poetry (see `docs/development/z_archive/poetry.md`).

## requirements.txt is used for the app image only

`docker/Dockerfile` installs from a plain `requirements.txt` (via `uv pip install --system
-r requirements.txt`) rather than `uv sync` directly, so it doesn't need `uv.lock` or
`pyproject.toml` copied into the build context. Regenerate it whenever `uv.lock` changes
(after `uv add`/`uv lock --upgrade`):

```powershell
uv export --no-dev --frozen -o requirements.txt
```

- `--no-dev` excludes every `[dependency-groups]` entry (`linters`, `tests`, `types`, `dev`) -
  only the `[project.dependencies]` the running app actually needs.
- `--frozen` uses `uv.lock` as-is without re-resolving, so this only regenerates the export -
  run `uv lock --upgrade` (or `uv add ...`) first if you actually want newer versions.
- Commit the regenerated `requirements.txt` alongside the `pyproject.toml`/`uv.lock` change in
  the same PR.

There are no `requirements.tests.txt` / `requirements.linters.txt` any more: `tests.yaml`
installs the tests group straight from `uv.lock` (`uv sync --frozen --group tests`), and
`lint.yaml` runs the prek hooks with `j178/prek-action`.

## Installing the dev/lint/test groups locally

`requirements.txt` above is Docker-image-only. For local development, use `uv sync` directly
against `pyproject.toml`/`uv.lock` instead of exporting - see `docs/development/windows venv.md`.

# UV

## Local Development

```
uv lock
uv sync --all-groups
```

If manually pinning to a specific Python version:
```
uv lock --python C:\Users\campbell\AppData\Local\Programs\Python\Python312\python.exe
uv sync --all-groups --python C:\Users\campbell\AppData\Local\Programs\Python\Python312\python.exe
```

## Updating the Lock File

Upgrade all packages:
```
uv lock --upgrade
```

Upgrade a specific package:
```
uv lock --upgrade-package ruff
```

## Exporting requirements.txt

`requirements.txt` contains only production dependencies (no dev/test/linter groups). Update it after changing dependencies or running a lock upgrade:
```
uv export --no-dev --frozen -o requirements.txt
```

---

## Deprecated: Poetry

### Poetry

put the python path in the vscode settings.json in the project
```
{
    "python.testing.pytestArgs": [
        "tests"
    ],
    "python.testing.unittestEnabled": false,
    "python.testing.pytestEnabled": true,
    "terminal.integrated.env.windows": {
        "PYTHONPATH": "${workspaceFolder}/src;${env:PYTHONPATH}"
    }
}
```

run locally
```
# $Env:PYTHONPATH="."
poetry run python src/blah
```

upgrading python version. Remove old venv. Select the new python version to use for env
```
poetry env use "C:\Program Files\Python312\python.exe"
```

Update poetry lock
```
poetry update
```

create local .venv
```
poetry config --local virtualenvs.in-project true
poetry install
```

remove local venv
```
poetry env list
poetry env remove xxxx
```

### Requirements export (Poetry)

Requires plugin
```
[tool.poetry.requires-plugins]
poetry-plugin-export = ">=1.8"
```

```
poetry export --without-hashes --with types -f requirements.txt -o requirements.txt
poetry export --without-hashes --only tests -f requirements.txt -o requirements.tests.txt
poetry export --without-hashes --only linters -f requirements.txt -o requirements.linters.txt
```

# UV

## Installation, .venv and syncing pyproject.toml
From the user home or project root, install uv if you don’t already have it:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your shell (or follow the installer’s PATH instructions), then confirm:
```bash
uv --version
```

Create a local virtual environment in the project root named .venv:
```bash
uv venv
# Activate it:
source .venv/bin/activate
```

Install the dependencies declared in pyproject.toml:
```bash
uv sync
```

uv sync also creates/updates uv.lock, so future installs can be reproduced exactly. To install optional development dependencies too, use:
```bash
uv sync --all-groups
```

Once activated, python and pip refer to .venv. Leave it with:
```bash
deactivate
```
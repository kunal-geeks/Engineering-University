# Lesson 10: Virtual Environments and pip

## Why Virtual Environments?

Projects often need different package versions. A **virtual environment** isolates dependencies per project so they do not conflict with system Python or other projects.

## Creating a venv

```bash
cd my-project
python3 -m venv .venv
```

This creates a `.venv` folder with its own Python and `pip`.

## Activating

**macOS / Linux:**

```bash
source .venv/bin/activate
```

**Windows (PowerShell):**

```powershell
.venv\Scripts\Activate.ps1
```

Your prompt may show `(.venv)`. Now `python` and `pip` point to the environment.

Deactivate when done:

```bash
deactivate
```

## Installing Packages

```bash
pip install requests
pip install "django>=4.2,<5"
pip list
pip show requests
```

## requirements.txt

Pin dependencies for reproducibility:

```bash
pip freeze > requirements.txt
```

Install from file:

```bash
pip install -r requirements.txt
```

Example `requirements.txt`:

```
requests==2.31.0
rich>=13.0.0
```

## pyproject.toml (Modern Projects)

Many projects use `pyproject.toml` with tools like Poetry, uv, or pip itself:

```toml
[project]
name = "my-app"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "requests>=2.31",
]
```

Install editable local package:

```bash
pip install -e .
```

## .gitignore

Never commit virtual environments. Add to `.gitignore`:

```
.venv/
__pycache__/
*.pyc
.env
```

Commit `requirements.txt` or `pyproject.toml`, not `.venv`.

## Upgrading and Removing

```bash
pip install --upgrade requests
pip uninstall requests
```

## Workflow Summary

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python main.py
deactivate
```

## Key Takeaways

- One venv per project keeps dependencies isolated.
- Activate the venv before installing or running project code.
- Track dependencies in `requirements.txt` or `pyproject.toml`.
- Add `.venv` to `.gitignore`.

## Exercises

1. Create a new folder, initialize a venv, and verify `which python` points inside `.venv`.
2. Install `httpx` and write a one-line script that fetches a URL.
3. Generate `requirements.txt` with `pip freeze` and recreate the env on a "fresh" shell using only that file.
4. Add a `.gitignore` appropriate for a Python project.

## Solutions

See [exercises/10-virtual-environments-and-pip.md](../exercises/10-virtual-environments-and-pip.md).

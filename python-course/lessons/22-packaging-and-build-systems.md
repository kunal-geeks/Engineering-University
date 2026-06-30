# Lesson 22: Packaging and Build Systems

Shipping libraries and services requires reproducible builds, clear dependency boundaries, and tooling that scales across teams — core principal-engineer responsibilities.

## Modern pyproject.toml

```toml
[project]
name = "mylib"
version = "1.0.0"
requires-python = ">=3.11"
dependencies = [
    "httpx>=0.27",
]

[project.optional-dependencies]
dev = ["pytest", "mypy", "ruff"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/mylib"]

[tool.ruff]
line-length = 100

[tool.mypy]
strict = true
```

## src Layout

```
mylib/
├── pyproject.toml
├── src/
│   └── mylib/
│       ├── __init__.py
│       └── core.py
└── tests/
    └── test_core.py
```

Prevents accidental imports from repo root during development.

## Building Wheels

```bash
pip install build
python -m build
# dist/mylib-1.0.0-py3-none-any.whl
```

Wheels are the standard distribution format; sdist for source builds.

## uv and pip — Speed and Lockfiles

```bash
pip install uv
uv init myapp
uv add fastapi
uv sync
uv run pytest
```

`uv.lock` pins transitive dependencies for reproducible CI and production.

## Editable Installs

```bash
pip install -e ".[dev]"
```

Changes in `src/` reflect immediately — essential for library development.

## Versioning

- **SemVer** — `MAJOR.MINOR.PATCH`
- Breaking API changes → major bump
- Use tags in git: `v1.2.0`

Tools: `hatch-vcs`, `setuptools-scm` for version from git tags.

## Monorepo Patterns

```
platform/
├── pyproject.toml      # workspace root
├── libs/
│   ├── auth/
│   └── billing/
└── services/
    └── api/
```

Coordinate with:

- Shared lockfile or per-package locks
- Internal packages as path or workspace dependencies
- CI matrix per changed package

## Publishing to PyPI

```bash
pip install twine
twine upload dist/*
```

Use trusted publishing (GitHub Actions OIDC) over long-lived tokens when possible.

## Application vs Library Packaging

| Type | Deploy as | Dependency mgmt |
|------|-----------|-----------------|
| Library | Wheel to PyPI | Loose ranges in metadata |
| Application | Container image | Locked deps (`uv.lock`, `requirements.txt`) |

Applications pin exact versions; libraries specify compatible ranges.

## Key Takeaways

- Use `pyproject.toml` + src layout as the default.
- Lock application dependencies; semver library dependencies thoughtfully.
- Automate build/test/publish in CI with reproducible environments.
- Monorepos need explicit ownership and change-detection in CI.

## Exercises

1. Convert a script project to src-layout package with `pyproject.toml`.
2. Build a wheel and install it in a fresh venv.
3. Set up `uv` with lockfile and document `uv sync` in README.
4. Draft a release checklist: version bump, changelog, tag, publish, verify.

## Solutions

See [exercises/22-packaging-and-build-systems.md](../exercises/22-packaging-and-build-systems.md).

## Next

[Part III: API Design & Services →](23-api-design-and-services.md)

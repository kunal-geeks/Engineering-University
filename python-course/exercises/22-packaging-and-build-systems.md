# Exercise Solutions — Lesson 22

## Release checklist template

```markdown
- [ ] Bump version in pyproject.toml
- [ ] Update CHANGELOG.md
- [ ] git tag vX.Y.Z
- [ ] CI green on tag
- [ ] python -m build && twine check dist/*
- [ ] twine upload dist/*
- [ ] Verify pip install package==X.Y.Z
- [ ] Announce deprecation timelines if any
```

See lesson for `pyproject.toml`, `uv sync`, and src layout examples.

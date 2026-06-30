# Exercise Solutions — Lesson 10

## 1. Create and verify venv

```bash
mkdir my-env-demo && cd my-env-demo
python3 -m venv .venv
source .venv/bin/activate
which python   # should end with my-env-demo/.venv/bin/python
```

## 2. httpx one-liner

```bash
pip install httpx
python -c "import httpx; print(httpx.get('https://httpbin.org/get').status_code)"
```

## 3. requirements.txt workflow

```bash
pip freeze > requirements.txt
deactivate
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## 4. .gitignore

```
.venv/
__pycache__/
*.py[cod]
.env
.DS_Store
```

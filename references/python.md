# Python Project Analysis Rules

## Signature Detection
- `pyproject.toml` or `requirements.txt` or `setup.py` or `setup.cfg`

## Scan Steps

### 1. Read Dependency Manifest
Check in order of preference:
1. `pyproject.toml` → Modern Python project (Poetry, PDM, uv, setuptools)
2. `requirements.txt` → pip-based
3. `setup.py` / `setup.cfg` → Legacy setuptools
4. `Pipfile` → Pipenv
5. `environment.yml` → Conda

Extract key frameworks:
- `django` → Django web framework
- `flask`, `fastapi`, `starlette` → Web API
- `sqlalchemy`, `tortoise-orm` → ORM
- `pydantic` → Data validation
- `celery` → Task queue
- `pytest` → Testing
- `click`, `typer` → CLI tool
- `numpy`, `pandas`, `scipy` → Data science
- `torch`, `tensorflow` → ML/AI
- `pygame` → Game
- `pillow`, `opencv-python` → Image processing
- `discord.py`, `hikari` → Discord bot

### 2. Map Directory Structure
| Directory | Common Role |
|---|---|
| `src/` or the project-named dir | Main package source |
| `tests/` or `test/` | Test suite |
| `migrations/` | Django/Alembic DB migrations |
| `templates/` | Web templates (Jinja2 / Django) |
| `static/` | Static files |
| `scripts/` | Utility/management scripts |
| `config/` | Configuration files |
| `notebooks/` | Jupyter notebooks |
| `data/` | Data files |

### 3. Find Entry Points
```bash
grep "if __name__ == .__main__." in src/ or *.py
```
Framework-specific:
- Django: `manage.py`, check `settings.py` for main app config
- Flask/FastAPI: look for `app = Flask(` or `app = FastAPI(`
- CLI: look for `@click.command()` or `@app.command()` or `argparse`

### 4. Check Python Version
```bash
read pyproject.toml (check "requires-python")
read .python-version (if exists)
```

### 5. Config Files
- `.env` / `.env.example` → Environment variables
- `Dockerfile` → Container deployment
- `docker-compose.yml` → Multi-service
- `Makefile` or `Taskfile` → Build/dev commands
- `.github/workflows/` → CI/CD

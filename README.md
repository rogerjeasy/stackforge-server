# StackForge Server

FastAPI backend for StackForge, a project scaffolding tool meant to generate FastAPI and React starter projects. The code still uses the working name "ProjectGen". The frontend lives in [stackforge-client](https://github.com/rogerjeasy/stackforge-client).

## Status

Early skeleton. Only the application setup runs; most modules are empty (0-byte) placeholders that describe the intended layout.

Implemented:

- FastAPI app with CORS middleware and a versioned router (`/api/v1`).
- Settings loaded from environment variables / `.env` with pydantic-settings (`app/core/config.py`).
- `GET /` returns the service name and version.
- `GET /api/v1/health` returns a health status.

Planned (files exist but are empty):

- FastAPI and React project generators (`app/generators/`) and their endpoints.
- Authentication and security helpers.
- Firestore and Redis clients and repositories, caching.
- Models and schemas.
- `Dockerfile`, `docker-compose.yml`, `.env.example`, `pytest.ini` and tests.

## Tech stack

- Python 3.11+, FastAPI, Uvicorn, pydantic-settings
- Declared for upcoming work: python-jose, passlib, google-cloud-firestore, redis, celery, jinja2, pyyaml, aiofiles, httpx
- Dev tools: pytest, black, flake8, mypy, pre-commit
- Dependencies are managed with Poetry (`pyproject.toml`; no lock file is committed yet)

## Project structure

```
app/
  main.py               # FastAPI app, CORS, router
  core/config.py        # Settings
  api/v1/router.py      # API router
  api/v1/endpoints/     # health.py (others are placeholders)
  generators/           # placeholders for the FastAPI and React generators
  db/, models/          # placeholders
tests/                  # placeholders
```

## Getting started

Prerequisites: Python 3.11+ and Poetry.

```bash
poetry install --no-root
```

Create a `.env` file in the project root. `SECRET_KEY` is required (the app will not start without it). Other settings have defaults and can be overridden:

| Variable | Default |
| --- | --- |
| `SECRET_KEY` | required |
| `APP_NAME`, `APP_VERSION`, `DEBUG`, `ENVIRONMENT` | `ProjectGen`, `1.0.0`, `True`, `development` |
| `API_V1_PREFIX` | `/api/v1` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | `30` |
| `BACKEND_CORS_ORIGINS` | `["http://localhost:3000"]` |
| `FIRESTORE_PROJECT_ID`, `GOOGLE_APPLICATION_CREDENTIALS` | empty |
| `REDIS_HOST`, `REDIS_PORT`, `REDIS_DB` | `localhost`, `6379`, `0` |
| `CELERY_BROKER_URL`, `CELERY_RESULT_BACKEND` | local Redis databases 1 and 2 |
| `STORAGE_PATH`, `TEMP_PATH` | `./storage`, `./storage/temp` |

Run the server:

```bash
poetry run uvicorn app.main:app --reload
```

The API is then available at http://localhost:8000, with the OpenAPI schema at `/api/v1/openapi.json` and interactive docs at `/docs`.

## Related

- [stackforge-client](https://github.com/rogerjeasy/stackforge-client): Next.js frontend with the landing page and FastAPI generator wizard.

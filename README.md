# tanstack-start-fastapi-template

Project template for a production-ready full-stack monorepo with a [TanStack Start](https://tanstack.com/start) + [shadcn/ui](https://ui.shadcn.com/) frontend and a [FastAPI](https://fastapi.tiangolo.com/) backend.

## What You Get

```
your-project/
├── frontend/          # TanStack Start + React + shadcn/ui (Mira)
├── backend/           # FastAPI + Python 3.13 + Pydantic
├── justfile           # Task runner (just dev, just check, just test, ...)
├── docker-compose.yml # Production deployment
├── lefthook.yml       # Parallel pre-commit hooks
└── .github/workflows/ # CI pipeline
```

| Layer    | Technology                                         |
| -------- | -------------------------------------------------- |
| Frontend | TanStack Start, React, shadcn/ui (Mira), Tailwind |
| Backend  | FastAPI, Python 3.13, Pydantic                     |
| API      | hey-api + TanStack Query + Zod (auto-generated)    |
| Lint     | Biome (frontend), Ruff (backend)                   |
| Types    | tsc (frontend), ty (backend)                       |
| Package  | Bun (frontend), uv (backend)                       |
| Tasks    | just                                               |
| VCS      | jj (colocated with git)                            |
| Hooks    | Lefthook (parallel pre-commit)                     |
| Docs     | MkDocs Material (backend API)                      |
| CI       | GitHub Actions                                     |
| Deploy   | Docker + docker compose                            |

## Prerequisites

- [Bun](https://bun.sh) >= 1.2
- [uv](https://docs.astral.sh/uv/) >= 0.6
- [just](https://github.com/casey/just) >= 1.0
- [jj](https://jj-vcs.github.io/jj/) >= 0.25
- [Lefthook](https://github.com/evilmartians/lefthook) >= 1.0
- [Docker](https://www.docker.com/) >= 24.0 (optional, for production builds)

Verify all required tools are installed:

```bash
bun --version && uv --version && just --version && jj --version && lefthook --version
```

## Usage

### Option 1: Copier (recommended)

Generate a new project with [Copier](https://copier.readthedocs.io/):

```bash
uvx copier copy --trust gh:johnrlive/tanstack-start-fastapi-template my-project
```

You will be prompted for:

| Variable       | Description                | Example             |
| -------------- | -------------------------- | ------------------- |
| `project_name` | Project name in kebab-case | `my-cool-app`       |
| `description`  | Short project description  | `My full-stack app` |

After rendering the template files, Copier automatically runs a setup script that:

1. Scaffolds the frontend via `bunx shadcn create` (removes ESLint, adds Biome)
2. Installs all frontend and backend dependencies
3. Generates the OpenAPI client from the FastAPI schema
4. Initializes jj/git and installs Lefthook pre-commit hooks

### Option 2: AI agent skill

If you use [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [OpenCode](https://opencode.ai/), or another agent that supports skills, install the skill and use it to scaffold the project interactively:

```bash
npx skills add johnrlive/tanstack-start-fastapi-template
```

## After Generation

```bash
cd my-project
just dev
```

- Frontend: http://localhost:3000
- Backend: http://localhost:8000
- API Docs: http://localhost:8000/docs

### Commands

```bash
just dev              # Run frontend + backend
just check            # Lint + format + typecheck + OpenAPI validation
just test             # Run all tests
just fix              # Auto-fix lint and format issues
just gen-api          # Regenerate API client from static schema
just gen-api-live     # Regenerate API client from running backend
just docs-serve       # Serve backend API docs locally
just docker-build     # Build Docker images
just docker-up        # Start production stack
just docker-down      # Stop production stack
just clean            # Remove generated files
```

Run `just` with no arguments to see all available commands.

## Next Steps

1. **Add an API endpoint** — create a router in `backend/app/routers/`, include it in `main.py`, then run `just gen-api` to regenerate the frontend client.
2. **Use the generated client** — import from `frontend/src/client/` for typed API calls with TanStack Query hooks and Zod validation.
3. **Deploy** — adjust `docker-compose.yml` for your infrastructure and push to trigger the GitHub Actions CI pipeline.

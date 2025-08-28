## Epic 1: Core Application Setup

**Expanded Goal:** This epic aims to establish the fundamental technical infrastructure for the application. It will ensure that a basic, deployable application shell is in place, providing a clean working environment for subsequent development without implementing any user-facing features. This foundational work is critical for enabling continuous integration and deployment from the outset.

### Story 1.1: Project Repository Setup

As a developer,
I want to set up the project repository with appropriate `.gitignore` and initial folder structure,
so that development can begin in an organized manner.

**Acceptance Criteria (Refined):**
1.  A `.gitignore` file exists at the root and is configured to ignore common Python (`__pycache__/`, `*.pyc`), Node.js (`node_modules/`, `.next/`), and OS-specific (`.DS_Store`, `Thumbs.db`) files.
2.  The root directory contains `backend/`, `frontend/`, and `docs/` subdirectories.
3.  The repository is initialized with Git, and a root-level `README.md` file has been created with the project title.

### Story 1.2: Basic Application Scaffolding

As a developer,
I want to scaffold a minimal FastAPI backend and a minimal Next.js frontend,
so that we have a deployable and verifiable application shell.

**Acceptance Criteria (Refined):**
1.  The `frontend/` directory contains a default Next.js application, runnable via `npm run dev`.
2.  The `backend/` directory contains a `main.py` with a basic FastAPI app and can be started using `uvicorn`. A `requirements.txt` (or `pyproject.toml`) managed by `uv` lists `fastapi` and `uvicorn`.
3.  The FastAPI application implements a `/health` endpoint that returns a JSON response: `{"status": "ok"}`.
4.  A developer can run both applications locally at the same time without port conflicts.

### Story 1.3: Initial Database Setup

As a developer,
I want to set up and initialize the SQLite database with the required tables,
so that the application has a persistent data layer for future features.

**Acceptance Criteria:**
1.  The `sqlalchemy` library is added to the backend dependencies using `uv`.
2.  The FastAPI application is configured to connect to a local SQLite database file (e.g., `simple-todo.db`).
3.  A database initialization script or function exists that, when run, creates the database file and a `users` table.
4.  The `users` table includes columns for at least `id` (primary key), `email` (unique), and `hashed_password`.

### Story 1.4: Initial Deployment Configuration

As a developer,
I want to configure basic deployment settings for the minimal application,
so that we can verify continuous integration and deployment processes.

**Acceptance Criteria (Refined):**
1.  The `backend/` directory contains a multi-stage `Dockerfile` that installs dependencies using `uv` and runs the FastAPI application.
2.  The project contains a GitHub Actions workflow file (e.g., `.github/workflows/deploy.yml`).
3.  When code is pushed to the `main` branch, the GitHub Action automatically:
    a. Builds the backend Docker image and pushes it to the DigitalOcean Container Registry.
    b. Triggers a deployment of the new image to a DigitalOcean App Platform app.
    c. Triggers a deployment of the Next.js frontend to a separate DigitalOcean App Platform app.
4.  The deployed Next.js application successfully makes a request to the deployed backend's `/health` endpoint and displays a "Backend is healthy" message on the page.

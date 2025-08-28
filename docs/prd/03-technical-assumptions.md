## Technical Assumptions

### Repository Structure: Monorepo

For the MVP, a **Monorepo** is assumed, containing both the frontend and backend code. This simplifies development and deployment for a small team.

### Service Architecture: Monolith

A **Monolithic** architecture is assumed for the MVP. The backend will serve a REST API. This approach is sufficient for the initial scope and allows for faster development.

### Testing Requirements: Unit + Integration

For the MVP, **Unit and Integration Testing** are assumed to be the primary testing requirements. This provides a good balance of code quality and development speed.

### Additional Technical Assumptions and Requests

*   **Backend Technology:** Python, using the **FastAPI** framework. The **uv** package manager will be used for managing Python dependencies.
*   **Frontend Technology:** **Next.js** (a React framework) to create a dynamic and responsive user experience.
*   **Database:** **SQLite** will be used for the MVP to simplify setup and development. A migration path to a more robust database like PostgreSQL can be considered post-MVP.
*   **Hosting/Infrastructure:** A cloud-based platform like Vercel (ideal for Next.js) for the frontend, and a platform like Heroku or DigitalOcean App Platform for the backend.
*   **Security:** Standard security practices must be implemented, including secure password hashing for user accounts and protection of all user data.

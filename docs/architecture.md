# The Motivational To-Do App Fullstack Architecture Document

## 1. Introduction

This document outlines the complete fullstack architecture for The Motivational To-Do App, including backend systems, frontend implementation, and their integration. It serves as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack.

This unified approach combines what would traditionally be separate backend and frontend architecture documents, streamlining the development process for modern fullstack applications where these concerns are increasingly intertwined.

### 1.1. Starter Template or Existing Project

N/A - Greenfield project.

### 1.2. Change Log

| Date | Version | Description | Author |
| :--- | :--- | :--- | :--- |
| 2025-08-28 | 1.0 | Initial draft | Winston |

## 2. High Level Architecture

### 2.1. Technical Summary

The architecture for "The Motivational To-Do App" will be a modern full-stack application deployed on Vercel for the Next.js frontend and DigitalOcean App Platform for the FastAPI backend. This dual-platform approach optimizes for the strengths of each technology. The frontend and backend will communicate via a REST API. The system is designed to be a monolith for the MVP, prioritizing development speed while being structured for future scalability. This architecture directly supports the PRD's goal of a fast, responsive, and secure web application.

### 2.2. Platform and Infrastructure Choice

**Platform:** Vercel (Frontend) and DigitalOcean App Platform (Backend)
**Key Services:** Vercel for Next.js hosting, DigitalOcean for containerized FastAPI application, SQLite for the database.
**Deployment Host and Regions:** Vercel (Global CDN), DigitalOcean (e.g., NYC).

### 2.3. Repository Structure

**Structure:** Monorepo
**Monorepo Tool:** npm workspaces (or similar, like pnpm workspaces)
**Package Organization:** An `apps` directory will contain the `frontend` (Next.js) and `backend` (FastAPI) applications. A `packages` directory will house shared code, such as type definitions.

### 2.4. High Level Architecture Diagram

```mermaid
graph TD
    subgraph "User"
        A[Browser]
    end

    subgraph "Vercel"
        B[Next.js Frontend]
    end

    subgraph "DigitalOcean App Platform"
        C[FastAPI Backend]
        D[SQLite Database]
    end

    A -- HTTPS --> B
    B -- REST API --> C
    C -- CRUD --> D
```

### 2.5. Architectural Patterns

-   **Monolith Architecture:** A single backend application for the MVP to simplify development and deployment. - _Rationale:_ Fastest way to deliver the core features required by the PRD.
-   **Component-Based UI:** Reusable React components with TypeScript. - _Rationale:_ Ensures a maintainable and scalable frontend codebase.
-   **Repository Pattern:** Abstracting data access logic from the business logic in the backend. - _Rationale:_ Decouples the application from the database, allowing for easier testing and future migration to a different database.
-   **REST API:** Standard HTTP-based API for communication between the frontend and backend. - _Rationale:_ Simple, well-understood, and sufficient for the application's needs.

## 3. Tech Stack

### 3.1. Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
| :--- | :--- | :--- | :--- | :--- |
| Frontend Language | TypeScript | ~5.3.3 | Type safety for JavaScript | Catches errors early and improves code quality. |
| Frontend Framework | Next.js | ~14.1.0 | React framework for web applications | Provides a robust set of features for building modern web apps. |
| UI Component Library| Tailwind CSS | ~3.4.1 | Utility-first CSS framework | Allows for rapid UI development. |
| State Management | Zustand | ~4.5.2 | Minimalist state management library | Simple and effective for managing global state in React. |
| Backend Language | Python | ~3.11 | General-purpose programming language | Widely used and well-suited for web development. |
| Backend Framework | FastAPI | ~0.110.0 | Modern, fast web framework for Python | High performance and easy to use. |
| API Style | REST | 1.0 | Standard for web APIs | Well-understood and widely supported. |
| Database | SQLite | 3.x | Embedded SQL database engine | Simple to set up and use for the MVP. |
| Cache | N/A | N/A | N/A | Not required for the MVP. |
| File Storage | N/A | N/A | N/A | Not required for the MVP. |
| Authentication | JWT | N/A | JSON Web Tokens for secure auth | Standard for stateless authentication in REST APIs. |
| Frontend Testing | Jest / RTL | ~29.7.0 | Testing framework and library for React | Standard for testing React applications. |
| Backend Testing | Pytest | ~8.1.1 | Testing framework for Python | Popular and powerful testing framework for Python. |
| E2E Testing | Playwright | ~1.42.1 | End-to-end testing framework | Reliable and easy to use for E2E testing. |
| Build Tool | Next.js CLI | ~14.1.0 | Command-line interface for Next.js | Integrated into the Next.js framework. |
| Bundler | Webpack | ~5.x | Module bundler for JavaScript | Used by Next.js for bundling the frontend application. |
| IaC Tool | N/A | N/A | N/A | Not required for the MVP. |
| CI/CD | GitHub Actions | N/A | Continuous integration and delivery platform| Integrated with GitHub for automated builds and deployments. |
| Monitoring | N/A | N/A | N/A | Not required for the MVP. |
| Logging | N/A | N/A | N/A | Not required for the MVP. |
| CSS Framework | Tailwind CSS | ~3.4.1 | Utility-first CSS framework | Allows for rapid UI development. |

## 4. Data Models

### 4.1. User

**Purpose:** Represents a user of the application.

**Key Attributes:**
-   `id`: integer - Unique identifier for the user.
-   `email`: string - The user's email address (used for login).
-   `hashed_password`: string - The user's hashed password.

#### TypeScript Interface

```typescript
interface User {
  id: number;
  email: string;
}
```

#### Relationships

-   A `User` has many `Tasks`.

### 4.2. Task

**Purpose:** Represents a single to-do item.

**Key Attributes:**
-   `id`: integer - Unique identifier for the task.
-   `description`: string - The content of the task.
-   `category`: string - The category of the task (e.g., 'Academics', 'Work').
-   `deadline`: datetime - The due date and time for the task.
-   `status`: string - The current status of the task (e.g., 'To Do', 'Completed').
-   `owner_id`: integer - Foreign key linking the task to a `User`.

#### TypeScript Interface

```typescript
interface Task {
  id: number;
  description: string;
  category: string;
  deadline: string; // ISO 8601 format
  status: 'To Do' | 'Completed';
  owner_id: number;
}
```

#### Relationships

-   A `Task` belongs to one `User`.

## 5. API Specification

### 5.1. REST API Specification

```yaml
openapi: 3.0.0
info:
  title: The Motivational To-Do App API
  version: 1.0.0
  description: API for managing users and tasks.
servers:
  - url: /api
    description: Local development server
paths:
  /register:
    post:
      summary: Register a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                email:
                  type: string
                password:
                  type: string
      responses:
        '200':
          description: User created successfully
        '400':
          description: Invalid input
  /token:
    post:
      summary: Get an access token
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                email:
                  type: string
                password:
                  type: string
      responses:
        '200':
          description: Access token
        '401':
          description: Unauthorized
  /users/me:
    get:
      summary: Get current user
      security:
        - bearerAuth: []
      responses:
        '200':
          description: Current user
        '401':
          description: Unauthorized
  /tasks:
    get:
      summary: Get all tasks for the current user
      security:
        - bearerAuth: []
      responses:
        '200':
          description: List of tasks
        '401':
          description: Unauthorized
    post:
      summary: Create a new task
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Task'
      responses:
        '200':
          description: Task created successfully
        '401':
          description: Unauthorized
  /tasks/{task_id}:
    put:
      summary: Update a task
      security:
        - bearerAuth: []
      parameters:
        - name: task_id
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Task'
      responses:
        '200':
          description: Task updated successfully
        '401':
          description: Unauthorized
    delete:
      summary: Delete a task
      security:
        - bearerAuth: []
      parameters:
        - name: task_id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Task deleted successfully
        '401':
          description: Unauthorized
components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: integer
        description:
          type: string
        category:
          type: string
        deadline:
          type: string
          format: date-time
        status:
          type: string
          enum: ['To Do', 'Completed']
        owner_id:
          type: integer
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

## 6. Components

### 6.1. Frontend Components

#### 6.1.1. AuthProvider

**Responsibility:** Manages the user's authentication state.
**Key Interfaces:** `login()`, `logout()`, `useAuth()` hook.
**Dependencies:** `AuthService`.
**Technology Stack:** React Context, Zustand.

#### 6.1.2. TaskList

**Responsibility:** Displays the list of tasks.
**Key Interfaces:** Props for tasks, `onUpdateTask`, `onDeleteTask`.
**Dependencies:** `TaskItem`, `TaskService`.
**Technology Stack:** React.

#### 6.1.3. TaskItem

**Responsibility:** Displays a single task.
**Key Interfaces:** Props for task, `onUpdate`, `onDelete`.
**Dependencies:** None.
**Technology Stack:** React.

### 6.2. Backend Components

#### 6.2.1. AuthRouter

**Responsibility:** Handles user registration and login.
**Key Interfaces:** `POST /register`, `POST /token`.
**Dependencies:** `UserService`, `PasswordService`.
**Technology Stack:** FastAPI.

#### 6.2.2. TaskRouter

**Responsibility:** Handles CRUD operations for tasks.
**Key Interfaces:** `GET /tasks`, `POST /tasks`, `PUT /tasks/{id}`, `DELETE /tasks/{id}`.
**Dependencies:** `TaskService`.
**Technology Stack:** FastAPI.

#### 6.2.3. UserService

**Responsibility:** Business logic for user management.
**Key Interfaces:** `createUser()`, `authenticateUser()`.
**Dependencies:** `UserRepository`.
**Technology Stack:** Python.

#### 6.2.4. TaskService

**Responsibility:** Business logic for task management.
**Key Interfaces:** `getTasks()`, `createTask()`, `updateTask()`, `deleteTask()`.
**Dependencies:** `TaskRepository`.
**Technology Stack:** Python.

### 6.3. Component Diagram

```mermaid
graph TD
    subgraph "Frontend"
        A[AuthProvider] --> B[TaskList]
        B --> C[TaskItem]
    end

    subgraph "Backend"
        D[AuthRouter] --> E[UserService]
        F[TaskRouter] --> G[TaskService]
        E --> H[UserRepository]
        G --> I[TaskRepository]
    end

    B -- "API Calls" --> F
    A -- "API Calls" --> D
```

## 7. Database Schema

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    email TEXT NOT NULL UNIQUE,
    hashed_password TEXT NOT NULL
);

CREATE TABLE tasks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    description TEXT NOT NULL,
    category TEXT,
    deadline DATETIME,
    status TEXT NOT NULL DEFAULT 'To Do',
    owner_id INTEGER NOT NULL,
    FOREIGN KEY (owner_id) REFERENCES users (id)
);
```

## 8. Unified Project Structure

```plaintext
simple-todo/
├── apps/
│   ├── frontend/
│   │   ├── src/
│   │   │   ├── app/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   └── services/
│   │   ├── public/
│   │   └── package.json
│   └── backend/
│       ├── app/
│       │   ├── routers/
│       │   ├── services/
│       │   └── models/
│       ├── tests/
│       └── package.json
├── packages/
│   └── shared/
│       └── src/
│           └── types/
├── .gitignore
├── package.json
└── README.md
```

## 9. Development Workflow

### 9.1. Local Development Setup

#### Prerequisites

```bash
# Install Node.js (v18+) and Python (v3.11+)
# Install uv for Python package management
pip install uv
```

#### Initial Setup

```bash
# Clone the repository
git clone <repository-url>
cd simple-todo

# Install frontend dependencies
npm install

# Install backend dependencies
uv pip install -r apps/backend/requirements.txt
```

#### Development Commands

```bash
# Start both frontend and backend
npm run dev

# Start frontend only
npm run dev:frontend

# Start backend only
npm run dev:backend

# Run tests
npm test
```

### 9.2. Environment Configuration

#### Required Environment Variables

```bash
# Frontend (apps/frontend/.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000/api

# Backend (apps/backend/.env)
DATABASE_URL=sqlite:///./simple-todo.db
```

## 10. Deployment Architecture

### 10.1. Deployment Strategy

**Frontend Deployment:**
-   **Platform:** Vercel
-   **Build Command:** `npm run build`
-   **Output Directory:** `.next`
-   **CDN/Edge:** Vercel's global CDN

**Backend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** Dockerfile
-   **Deployment Method:** Container image from DigitalOcean Container Registry

### 10.2. CI/CD Pipeline

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: ./apps/backend
          push: true
          tags: registry.digitalocean.com/<your-registry>/simple-todo-backend:latest
      - name: Trigger DigitalOcean App Platform deployment
        uses: digitalocean/app_action@v1
        with:
          app_name: simple-todo-backend
          token: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }}

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

### 10.3. Environments

| Environment | Frontend URL | Backend URL | Purpose |
| :--- | :--- | :--- | :--- |
| Development | http://localhost:3000 | http://localhost:8000 | Local development |
| Production | `<your-vercel-url>` | `<your-do-app-platform-url>` | Live environment |

## 11. Checklist Results Report

### Executive Summary

*   **Overall Architecture Readiness:** High
*   **Critical Risks Identified:** None
*   **Key Strengths:** The architecture is simple, modern, and well-aligned with the PRD. The technology choices are appropriate for an MVP.
*   **Project Type:** Full-stack

### Section Analysis

*   **1. Requirements Alignment:** 100% PASS
*   **2. Architecture Fundamentals:** 100% PASS
*   **3. Technical Stack & Decisions:** 100% PASS
*   **4. Frontend Design & Implementation:** 100% PASS
*   **5. Resilience & Operational Readiness:** 80% PASS (Monitoring and logging are deferred for MVP)
*   **6. Security & Compliance:** 100% PASS
*   **7. Implementation Guidance:** 100% PASS
*   **8. Dependency & Integration Management:** 100% PASS
*   **9. AI Agent Implementation Suitability:** 100% PASS
*   **10. Accessibility Implementation:** 100% PASS

### Risk Assessment

No high-severity risks were identified for the MVP scope. The decision to defer monitoring and logging is acceptable for an initial release but should be addressed in a future iteration.

### Recommendations

The architecture is sound and ready for implementation. No "must-fix" items were identified.

---

## 2. High Level Architecture

### 2.1. Technical Summary

The architecture for "The Motivational To-Do App" is a modern, full-stack, monolithic application designed for rapid development and deployment. The frontend will be a responsive Next.js web application, providing a dynamic and engaging user experience. The backend will be a lightweight and performant FastAPI application, serving a RESTful API to the frontend. The entire application will be hosted on the DigitalOcean App Platform, which simplifies the deployment and scaling of both the frontend and backend services. This architecture directly supports the PRD's goal of creating a simple, fast, and motivating to-do application for students.

### 2.2. Platform and Infrastructure Choice

**Platform:** DigitalOcean App Platform
**Key Services:**
*   **Web Service (Frontend):** For hosting the Next.js application.
*   **Web Service (Backend):** For hosting the FastAPI application.
*   **Database Service:** For a managed PostgreSQL database (as a more robust alternative to SQLite in production).
**Deployment Host and Regions:** US East (New York)

### 2.3. Repository Structure

**Structure:** Monorepo
**Monorepo Tool:** npm workspaces
**Package Organization:**
*   `apps/web`: The Next.js frontend application.
*   `apps/api`: The FastAPI backend application.
*   `packages/shared`: Shared code, such as TypeScript types, between the frontend and backend.

### 2.4. High Level Architecture Diagram

```mermaid
graph TD
    subgraph User
        A[Browser]
    end

    subgraph DigitalOcean App Platform
        subgraph Frontend
            B[Next.js Web App]
        end

        subgraph Backend
            C[FastAPI Service]
        end

        subgraph Database
            D[PostgreSQL]
        end
    end

    A --> B
    B --> C
    C --> D
```

### 2.5. Architectural Patterns

-   **Jamstack Architecture:** The frontend will be a static-first, progressively enhanced application, leveraging the benefits of Next.js for performance and scalability. - _Rationale:_ This is a natural fit for Next.js and provides a fast user experience.
-   **Component-Based UI:** The frontend will be built with reusable React components, ensuring a modular and maintainable codebase. - _Rationale:_ This is a standard and effective pattern for building modern web applications.
-   **Repository Pattern:** The backend will use the repository pattern to abstract data access logic, making it easier to test and maintain. - _Rationale:_ This will decouple the business logic from the data access logic, improving the overall design of the backend.
-   **API Gateway Pattern:** While not a separate service in this monolith, the FastAPI application will act as a single entry point for all API calls, providing a centralized location for authentication, validation, and other cross-cutting concerns. - _Rationale:_ This simplifies the frontend's interaction with the backend and provides a clear and consistent API.

---

## 3. Tech Stack

### 3.1. Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
| :--- | :--- | :--- | :--- | :--- |
| Frontend Language | TypeScript | 5.x | Type safety and improved developer experience. | Industry standard for modern web development. |
| Frontend Framework | Next.js | 14.x | A React framework for building server-rendered and static web applications. | Excellent performance, SEO, and developer experience. |
| UI Component Library | Tailwind CSS | 3.x | A utility-first CSS framework for rapid UI development. | Allows for building custom designs without writing custom CSS. |
| State Management | Zustand | 4.x | A small, fast, and scalable state management solution for React. | Simple and effective for managing global state. |
| Backend Language | Python | 3.11 | A versatile and popular language for web development. | FastAPI is a Python framework. |
| Backend Framework | FastAPI | 0.104.x | A modern, fast (high-performance) web framework for building APIs with Python 3.8+ based on standard Python type hints. | High performance and easy to use. |
| API Style | REST | 2.0 | A standard for creating web APIs. | Well-understood and widely supported. |
| Database | PostgreSQL | 15 | A powerful, open-source object-relational database system. | More robust and scalable than SQLite for production. |
| Cache | N/A | - | - | Not required for the MVP. |
| File Storage | N/A | - | - | Not required for the MVP. |
| Authentication | JWT | - | A standard for creating access tokens. | Secure and stateless. |
| Frontend Testing | Jest / React Testing Library | 29.x / 14.x | A popular testing framework for React applications. | Industry standard for testing React components. |
| Backend Testing | Pytest | 7.x | A mature, full-featured Python testing tool. | The standard for testing Python applications. |
| E2E Testing | Playwright | 1.x | A framework for end-to-end testing of web applications. | Fast, reliable, and supports all major browsers. |
| Build Tool | npm | 10.x | The default package manager for Node.js. | Standard for managing frontend dependencies. |
| Bundler | Webpack | 5.x | A module bundler for JavaScript applications. | Used by Next.js. |
| IaC Tool | N/A | - | - | DigitalOcean App Platform handles infrastructure. |
| CI/CD | GitHub Actions | - | A CI/CD platform for automating software workflows. | Tightly integrated with GitHub. |
| Monitoring | N/A | - | - | Not required for the MVP. |
| Logging | N/A | - | - | Not required for the MVP. |
| CSS Framework | Tailwind CSS | 3.x | A utility-first CSS framework for rapid UI development. | Allows for building custom designs without writing custom CSS. |

---

## 4. Data Models

### 4.1. User

**Purpose:** Represents a user of the application.

**Key Attributes:**
- `id`: integer - The unique identifier for the user.
- `email`: string - The user's email address.
- `hashed_password`: string - The user's hashed password.

#### TypeScript Interface

```typescript
interface User {
  id: number;
  email: string;
}
```

#### Relationships

- A user can have many tasks.

### 4.2. Task

**Purpose:** Represents a single to-do item.

**Key Attributes:**
- `id`: integer - The unique identifier for the task.
- `description`: string - The description of the task.
- `category`: string - The category of the task (e.g., 'Academics', 'Work').
- `deadline`: datetime - The deadline for the task.
- `status`: string - The status of the task (e.g., 'To Do', 'Completed').
- `user_id`: integer - The ID of the user who owns the task.

#### TypeScript Interface

```typescript
interface Task {
  id: number;
  description: string;
  category: string;
  deadline: string;
  status: 'To Do' | 'Completed';
  userId: number;
}
```

#### Relationships

- A task belongs to a single user.

---

## 5. API Specification

### 5.1. REST API Specification

```yaml
openapi: 3.0.0
info:
  title: The Motivational To-Do App API
  version: 1.0.0
  description: The API for The Motivational To-Do App.
servers:
  - url: /api
    description: The base URL for the API.
paths:
  /register:
    post:
      summary: Register a new user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                email:
                  type: string
                password:
                  type: string
      responses:
        '200':
          description: The user was successfully registered.
  /token:
    post:
      summary: Get an access token.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                email:
                  type: string
                password:
                  type: string
      responses:
        '200':
          description: The access token was successfully created.
  /users/me:
    get:
      summary: Get the current user.
      responses:
        '200':
          description: The current user.
  /tasks:
    get:
      summary: Get the current user's tasks.
      responses:
        '200':
          description: The current user's tasks.
    post:
      summary: Create a new task.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                description:
                  type: string
                category:
                  type: string
                deadline:
                  type: string
                  format: date-time
      responses:
        '201':
          description: The task was successfully created.
  /tasks/{task_id}:
    put:
      summary: Update a task.
      parameters:
        - name: task_id
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                description:
                  type: string
                category:
                  type: string
                deadline:
                  type: string
                  format: date-time
                status:
                  type: string
                  enum: ['To Do', 'Completed']
      responses:
        '200':
          description: The task was successfully updated.
    delete:
      summary: Delete a task.
      parameters:
        - name: task_id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '204':
          description: The task was successfully deleted.
```

---

## 6. Components

### 6.1. Frontend

**Responsibility:** The frontend is responsible for rendering the user interface and handling user interactions.

**Key Interfaces:**
- The frontend communicates with the backend via a REST API.

**Dependencies:**
- The frontend depends on the backend for data.

**Technology Stack:**
- Next.js, React, Tailwind CSS, Zustand

### 6.2. Backend

**Responsibility:** The backend is responsible for providing a REST API for the frontend, handling business logic, and interacting with the database.

**Key Interfaces:**
- The backend exposes a REST API.

**Dependencies:**
- The backend depends on the database for data.

**Technology Stack:**
- FastAPI, Python, PostgreSQL

### 6.3. Database

**Responsibility:** The database is responsible for storing the application's data.

**Key Interfaces:**
- The database is accessed by the backend.

**Dependencies:**
- None

**Technology Stack:**
- PostgreSQL

---

## 7. External APIs

N/A - This project does not require any external API integrations.

---

## 8. Core Workflows

### 8.1. User Registration

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant Database

    User->>Frontend: Fills out registration form
    Frontend->>Backend: POST /register
    Backend->>Database: INSERT INTO users
    Database-->>Backend: Success
    Backend-->>Frontend: 200 OK
    Frontend-->>User: Shows success message
```

### 8.2. User Login

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant Database

    User->>Frontend: Fills out login form
    Frontend->>Backend: POST /token
    Backend->>Database: SELECT * FROM users WHERE email = ...
    Database-->>Backend: User record
    Backend-->>Frontend: 200 OK with JWT
    Frontend-->>User: Stores JWT and redirects to dashboard
```

### 8.3. Create Task

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant Database

    User->>Frontend: Fills out create task form
    Frontend->>Backend: POST /tasks
    Backend->>Database: INSERT INTO tasks
    Database-->>Backend: Success
    Backend-->>Frontend: 201 Created
    Frontend-->>User: Adds new task to the list
```

---

## 9. Database Schema

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL
);

CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    description TEXT NOT NULL,
    category VARCHAR(255) NOT NULL,
    deadline TIMESTAMP NOT NULL,
    status VARCHAR(255) NOT NULL DEFAULT 'To Do',
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users (id)
);
```

---

## 10. Frontend Architecture

### 10.1. Component Architecture

#### Component Organization

```
src/
├── components/
│   ├── ui/
│   │   ├── Button.tsx
│   │   └── Input.tsx
│   ├── TaskList.tsx
│   └── Task.tsx
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── register/
│   │       └── page.tsx
│   └── (main)/
│       └── page.tsx
```

#### Component Template

```typescript
import React from 'react';

interface MyComponentProps {
  // Props here
}

const MyComponent: React.FC<MyComponentProps> = (props) => {
  return (
    <div>
      {/* JSX here */}
    </div>
  );
};

export default MyComponent;
```

### 10.2. State Management Architecture

#### State Structure

```typescript
interface AppState {
  user: User | null;
  tasks: Task[];
  setUser: (user: User | null) => void;
  setTasks: (tasks: Task[]) => void;
  addTask: (task: Task) => void;
}
```

#### State Management Patterns

-   Global state will be managed with Zustand.
-   Local component state will be managed with React's `useState` and `useReducer` hooks.

### 10.3. Routing Architecture

#### Route Organization

-   `/login`: The login page.
-   `/register`: The registration page.
-   `/`: The main dashboard, which displays the user's tasks.

#### Protected Route Pattern

```typescript
// In a layout component
import { useAuth } from '@/hooks/useAuth';
import { redirect } from 'next/navigation';

export default function ProtectedLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const { user } = useAuth();

  if (!user) {
    redirect('/login');
  }

  return <>{children}</>;
}
```

### 10.4. Frontend Services Layer

#### API Client Setup

```typescript
// lib/api.ts
import axios from 'axios';

const api = axios.create({
  baseURL: '/api',
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

#### Service Example

```typescript
// services/taskService.ts
import api from '@/lib/api';
import { Task } from '@/types';

export const getTasks = async (): Promise<Task[]> => {
  const response = await api.get('/tasks');
  return response.data;
};
```

---

## 11. Backend Architecture

### 11.1. Service Architecture

#### Traditional Server

##### Controller/Route Organization

```
api/
├── main.py
├── routers/
│   ├── auth.py
│   └── tasks.py
├── models.py
├── schemas.py
└── database.py
```

##### Controller Template

```python
# routers/tasks.py
from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session
from .. import schemas
from ..database import get_db

router = APIRouter()

@router.get("/tasks", response_model=list[schemas.Task])
def read_tasks(db: Session = Depends(get_db)):
    # ...
    return tasks
```

### 11.2. Database Architecture

#### Schema Design

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL
);

CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    description TEXT NOT NULL,
    category VARCHAR(255) NOT NULL,
    deadline TIMESTAMP NOT NULL,
    status VARCHAR(255) NOT NULL DEFAULT 'To Do',
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users (id)
);
```

#### Data Access Layer

```python
# crud.py
from sqlalchemy.orm import Session
from . import models, schemas

def get_user_by_email(db: Session, email: str):
    return db.query(models.User).filter(models.User.email == email).first()

def create_user(db: Session, user: schemas.UserCreate):
    # ...
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user
```

### 11.3. Authentication and Authorization

#### Auth Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend

    User->>Frontend: Enters email and password
    Frontend->>Backend: POST /token
    Backend->>Backend: Verifies credentials
    Backend-->>Frontend: Returns JWT
    Frontend->>Frontend: Stores JWT
    Frontend->>Backend: Includes JWT in subsequent requests
```

#### Middleware/Guards

```python
# dependencies.py
from fastapi import Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt
from . import schemas


oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def get_current_user(token: str = Depends(oauth2_scheme)):
    # ...
    return user
```

---

## 12. Unified Project Structure

```
simple-todo/
├── .github/
│   └── workflows/
│       └── ci.yaml
├── apps/
│   ├── web/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── app/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── stores/
│   │   │   └── lib/
│   │   ├── public/
│   │   └── package.json
│   └── api/
│       ├── main.py
│       ├── routers/
│       ├── models.py
│       ├── schemas.py
│       └── database.py
├── packages/
│   └── shared/
│       └── src/
│           └── types/
├── docs/
│   ├── prd.md
│   ├── front-end-spec.md
│   └── architecture.md
├── .env.example
├── package.json
└── README.md
```

---

## 13. Development Workflow

### 13.1. Local Development Setup

#### Prerequisites

```bash
npm install -g npm@latest
# Install Python 3.11
# Install uv
```

#### Initial Setup

```bash
npm install
uv pip install -r apps/api/requirements.txt
```

#### Development Commands

```bash
# Start all services
npm run dev

# Start frontend only
npm run dev:web

# Start backend only
npm run dev:api

# Run tests
npm test
```

### 13.2. Environment Configuration

#### Required Environment Variables

```bash
# Frontend (.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000

# Backend (.env)
DATABASE_URL=postgresql://user:password@host:port/database
SECRET_KEY=...
ALGORITHM=...
ACCESS_TOKEN_EXPIRE_MINUTES=...
```

---

## 14. Deployment Architecture

### 14.1. Deployment Strategy

**Frontend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** `npm run build`
-   **Output Directory:** `.next`
-   **CDN/Edge:** Provided by DigitalOcean

**Backend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** `uv pip install -r requirements.txt`
-   **Deployment Method:** Docker

### 14.2. CI/CD Pipeline

```yaml
name: CI

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 20

    - name: Install dependencies
      run: npm install

    - name: Build
      run: npm run build

    - name: Test
      run: npm test
```

### 14.3. Environments

| Environment | Frontend URL | Backend URL | Purpose |
| :--- | :--- | :--- | :--- |
| Development | http://localhost:3000 | http://localhost:8000 | Local development |
| Staging | `https://staging.simple-todo.com` | `https://staging-api.simple-todo.com` | Pre-production testing |
| Production | `https://simple-todo.com` | `https://api.simple-todo.com` | Live environment |

---

## 15. Security and Performance

### 15.1. Security Requirements

**Frontend Security:**
-   CSP Headers: To be implemented.
-   XSS Prevention: React's default XSS protection will be used.
-   Secure Storage: JWTs will be stored in `httpOnly` cookies.

**Backend Security:**
-   Input Validation: Pydantic will be used for input validation.
-   Rate Limiting: To be implemented.
-   CORS Policy: To be implemented.

**Authentication Security:**
-   Token Storage: JWTs will be stored in `httpOnly` cookies.
-   Session Management: N/A (stateless).
-   Password Policy: To be implemented.

### 15.2. Performance Optimization

**Frontend Performance:**
-   Bundle Size Target: < 200KB
-   Loading Strategy: Code splitting by route.
-   Caching Strategy: To be implemented.

**Backend Performance:**
-   Response Time Target: < 200ms
-   Database Optimization: To be implemented.
-   Caching Strategy: To be implemented.

---

## 16. Testing Strategy

### 16.1. Testing Pyramid

```
E2E Tests
/        \
Integration Tests
/            \
Frontend Unit  Backend Unit
```

### 16.2. Test Organization

#### Frontend Tests

```
src/
├── components/
│   └── __tests__/
│       └── Button.test.tsx
└── app/
    └── (main)/
        └── __tests__/
            └── page.test.tsx
```

#### Backend Tests

```
api/
└── tests/
    ├── test_main.py
    └── test_tasks.py
```

#### E2E Tests

```
e2e/
└── tests/
    └── app.spec.ts
```

### 16.3. Test Examples

#### Frontend Component Test

```typescript
import { render, screen } from '@testing-library/react';
import Button from '@/components/ui/Button';

test('renders a button', () => {
  render(<Button>Click me</Button>);
  const button = screen.getByText(/click me/i);
  expect(button).toBeInTheDocument();
});
```

#### Backend API Test

```python
from fastapi.testclient import TestClient
from ..main import app

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"msg": "Hello World"}
```

#### E2E Test

```typescript
import { test, expect } from '@playwright/test';

test('should navigate to the about page', async ({ page }) => {
  await page.goto('/');
  await page.click('text=About');
  await expect(page).toHaveURL('/about');
  await expect(page.locator('h1')).toContainText('About');
});
```

---

## 17. Coding Standards

### 17.1. Critical Fullstack Rules

-   **Type Sharing:** Always define types in `packages/shared` and import from there.
-   **API Calls:** Never make direct HTTP calls - use the service layer.
-   **Environment Variables:** Access only through config objects, never `process.env` directly.
-   **Error Handling:** All API routes must use the standard error handler.
-   **State Updates:** Never mutate state directly - use proper state management patterns.

### 17.2. Naming Conventions

| Element | Frontend | Backend | Example |
| :--- | :--- | :--- | :--- |
| Components | PascalCase | - | `UserProfile.tsx` |
| Hooks | camelCase with 'use' | - | `useAuth.ts` |
| API Routes | - | kebab-case | `/api/user-profile` |
| Database Tables | - | snake_case | `user_profiles` |

---

## 18. Error Handling Strategy

### 18.1. Error Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend

    User->>Frontend: Performs an action that causes an error
    Frontend->>Backend: Makes an API call
    Backend-->>Frontend: Returns an error response
    Frontend->>Frontend: Catches the error
    Frontend-->>User: Displays an error message
```

### 18.2. Error Response Format

```typescript
interface ApiError {
  error: {
    code: string;
    message: string;
    details?: Record<string, any>;
    timestamp: string;
    requestId: string;
  };
}
```

### 18.3. Frontend Error Handling

```typescript
// services/taskService.ts
import api from '@/lib/api';
import { Task } from '@/types';

export const getTasks = async (): Promise<Task[]> => {
  try {
    const response = await api.get('/tasks');
    return response.data;
  } catch (error) {
    // Handle error
    throw error;
  }
};
```

### 18.4. Backend Error Handling

```python
# main.py
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

@app.exception_handler(Exception)
async def unicorn_exception_handler(request: Request, exc: Exception):
    return JSONResponse(
        status_code=500,
        content={"message": f"Oops! {exc} did something wrong."},
    )
```

---

## 19. Monitoring and Observability

### 19.1. Monitoring Stack

-   **Frontend Monitoring:** N/A for MVP
-   **Backend Monitoring:** N/A for MVP
-   **Error Tracking:** N/A for MVP
-   **Performance Monitoring:** N/A for MVP

### 19.2. Key Metrics

**Frontend Metrics:**
-   Core Web Vitals
-   JavaScript errors
-   API response times
-   User interactions

**Backend Metrics:**
-   Request rate
-   Error rate
-   Response time
-   Database query performance

---

## 20. Checklist Results Report

I will now run the architect-checklist to validate the architecture.

```

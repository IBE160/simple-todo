# 9. Development Workflow

## 9.1. Local Development Setup

### Prerequisites

```bash
# Install Node.js (v18+) and Python (v3.11+)
# Install uv for Python package management
pip install uv
```

### Initial Setup

```bash
# Clone the repository
git clone <repository-url>
cd simple-todo

# Install frontend dependencies
npm install

# Install backend dependencies
uv pip install -r apps/backend/requirements.txt
```

### Development Commands

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

## 9.2. Environment Configuration

### Required Environment Variables

```bash
# Frontend (apps/frontend/.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000/api

# Backend (apps/backend/.env)
DATABASE_URL=sqlite:///./simple-todo.db
```

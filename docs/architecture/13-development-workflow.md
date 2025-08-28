# 13. Development Workflow

## 13.1. Local Development Setup

### Prerequisites

```bash
npm install -g npm@latest
# Install Python 3.11
# Install uv
```

### Initial Setup

```bash
npm install
uv pip install -r apps/api/requirements.txt
```

### Development Commands

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

## 13.2. Environment Configuration

### Required Environment Variables

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

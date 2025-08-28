# 14. Deployment Architecture

## 14.1. Deployment Strategy

**Frontend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** `npm run build`
-   **Output Directory:** `.next`
-   **CDN/Edge:** Provided by DigitalOcean

**Backend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** `uv pip install -r requirements.txt`
-   **Deployment Method:** Docker

## 14.2. CI/CD Pipeline

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

## 14.3. Environments

| Environment | Frontend URL | Backend URL | Purpose |
| :--- | :--- | :--- | :--- |
| Development | http://localhost:3000 | http://localhost:8000 | Local development |
| Staging | `https://staging.simple-todo.com` | `https://staging-api.simple-todo.com` | Pre-production testing |
| Production | `https://simple-todo.com` | `https://api.simple-todo.com` | Live environment |

---

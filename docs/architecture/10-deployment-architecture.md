# 10. Deployment Architecture

## 10.1. Deployment Strategy

**Frontend Deployment:**
-   **Platform:** Vercel
-   **Build Command:** `npm run build`
-   **Output Directory:** `.next`
-   **CDN/Edge:** Vercel's global CDN

**Backend Deployment:**
-   **Platform:** DigitalOcean App Platform
-   **Build Command:** Dockerfile
-   **Deployment Method:** Container image from DigitalOcean Container Registry

## 10.2. CI/CD Pipeline

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

## 10.3. Environments

| Environment | Frontend URL | Backend URL | Purpose |
| :--- | :--- | :--- | :--- |
| Development | http://localhost:3000 | http://localhost:8000 | Local development |
| Production | `<your-vercel-url>` | `<your-do-app-platform-url>` | Live environment |

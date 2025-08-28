# 16. Testing Strategy

## 16.1. Testing Pyramid

```
E2E Tests
/        \
Integration Tests
/            \
Frontend Unit  Backend Unit
```

## 16.2. Test Organization

### Frontend Tests

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

### Backend Tests

```
api/
└── tests/
    ├── test_main.py
    └── test_tasks.py
```

### E2E Tests

```
e2e/
└── tests/
    └── app.spec.ts
```

## 16.3. Test Examples

### Frontend Component Test

```typescript
import { render, screen } from '@testing-library/react';
import Button from '@/components/ui/Button';

test('renders a button', () => {
  render(<Button>Click me</Button>);
  const button = screen.getByText(/click me/i);
  expect(button).toBeInTheDocument();
});
```

### Backend API Test

```python
from fastapi.testclient import TestClient
from ..main import app

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"msg": "Hello World"}
```

### E2E Test

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

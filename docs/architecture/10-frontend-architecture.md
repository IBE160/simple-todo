# 10. Frontend Architecture

## 10.1. Component Architecture

### Component Organization

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

### Component Template

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

## 10.2. State Management Architecture

### State Structure

```typescript
interface AppState {
  user: User | null;
  tasks: Task[];
  setUser: (user: User | null) => void;
  setTasks: (tasks: Task[]) => void;
  addTask: (task: Task) => void;
}
```

### State Management Patterns

-   Global state will be managed with Zustand.
-   Local component state will be managed with React's `useState` and `useReducer` hooks.

## 10.3. Routing Architecture

### Route Organization

-   `/login`: The login page.
-   `/register`: The registration page.
-   `/`: The main dashboard, which displays the user's tasks.

### Protected Route Pattern

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

## 10.4. Frontend Services Layer

### API Client Setup

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

### Service Example

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

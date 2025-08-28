# 4. Data Models

## 4.1. User

**Purpose:** Represents a user of the application.

**Key Attributes:**
- `id`: integer - The unique identifier for the user.
- `email`: string - The user's email address.
- `hashed_password`: string - The user's hashed password.

### TypeScript Interface

```typescript
interface User {
  id: number;
  email: string;
}
```

### Relationships

- A user can have many tasks.

## 4.2. Task

**Purpose:** Represents a single to-do item.

**Key Attributes:**
- `id`: integer - The unique identifier for the task.
- `description`: string - The description of the task.
- `category`: string - The category of the task (e.g., 'Academics', 'Work').
- `deadline`: datetime - The deadline for the task.
- `status`: string - The status of the task (e.g., 'To Do', 'Completed').
- `user_id`: integer - The ID of the user who owns the task.

### TypeScript Interface

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

### Relationships

- A task belongs to a single user.

---

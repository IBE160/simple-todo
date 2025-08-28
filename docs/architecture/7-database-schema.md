# 7. Database Schema

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

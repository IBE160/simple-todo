# 5. API Specification

## 5.1. REST API Specification

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

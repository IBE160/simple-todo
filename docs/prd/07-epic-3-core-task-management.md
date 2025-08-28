## Epic 3: Core Task Management

**Expanded Goal:** This epic aims to implement the fundamental capabilities for users to manage their personal tasks within the application. It will enable users to create, view, update, and delete tasks, along with defining essential task properties, forming the core utility of the to-do application.

### Story 3.1: Task Creation

As a logged-in user,
I want to create new tasks with essential properties,
so that I can add items to my to-do list.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `POST /tasks`) exists that accepts a description, category, and deadline.
2.  The endpoint validates the input and creates a new task record in the database, linking it to the authenticated user.
3.  The new task is assigned a default status of 'To Do'.
4.  A form exists on the main dashboard of the Next.js app for creating a new task.
5.  Submitting the form calls the API and the new task appears on the user's task list without needing a page refresh.

### Story 3.2: View Task List

As a logged-in user,
I want to view a list of my tasks,
so that I can see what I need to do.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `GET /tasks`) exists that returns a list of all tasks belonging to the authenticated user.
2.  The Next.js app fetches and displays this list on the main dashboard when the user is logged in.
3.  Each task in the list clearly displays its description, category, deadline, and status.
4.  The tasks are displayed in a clear and organized manner (e.g., sorted by deadline).

### Story 3.3: Update Task

As a logged-in user,
I want to modify existing tasks,
so that I can keep my to-do list accurate and up-to-date.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `PUT /tasks/{task_id}`) allows updating a task's description, category, deadline, and status.
2.  The endpoint verifies that the task being updated belongs to the authenticated user.
3.  The UI provides a way to edit a task (e.g., an "Edit" button that opens a modal).
4.  The UI allows changing a task's status (e.g., a dropdown menu with 'To Do', 'In Progress', 'Completed').
5.  Changes are saved and reflected in the task list.

### Story 3.4: Delete Task

As a logged-in user,
I want to remove tasks from my list,
so that I can keep my to-do list clean and relevant.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `DELETE /tasks/{task_id}`) removes a task.
2.  The endpoint verifies that the task being deleted belongs to the authenticated user.
3.  The UI provides a "Delete" button for each task.
4.  A confirmation prompt is displayed before the task is permanently deleted.

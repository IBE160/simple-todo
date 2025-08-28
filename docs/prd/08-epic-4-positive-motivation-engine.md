## Epic 4: Positive Motivation Engine

**Expanded Goal:** This epic aims to implement the unique "Positive Motivation Engine" that differentiates this application. It will focus on providing encouraging feedback upon task completion and displaying clear, scalable metrics to foster a sense of achievement and progress, thereby combating procrastination and building positive momentum for the user.

### Story 4.1: Task Completion Encouragement

As a user,
I want to receive encouraging messages when I complete a task,
so that I feel motivated and acknowledge my progress.

**Acceptance Criteria:**
1.  When a task's status is updated to 'Completed' via the API, the backend response includes a randomized, positive message.
2.  A list of positive messages is stored in the backend (e.g., in a simple list or configuration file).
3.  The Next.js frontend displays this message to the user in a non-intrusive way (e.g., a temporary toast notification).
4.  A mechanism exists to control the frequency of these messages (e.g., the backend only sends a message for the first 5 completed tasks per day).

### Story 4.2: Daily Task Completion Metrics

As a user,
I want to see how many tasks I've completed today,
so that I can track my daily productivity.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `GET /metrics/daily`) exists that returns the count of tasks completed by the user for the current day.
2.  The calculation is accurate, considering the user's timezone if possible.
3.  The Next.js app displays this number clearly on the main dashboard.
4.  The metric updates in near real-time when a task is marked as complete.

### Story 4.3: Weekly Task Completion Percentage

As a user,
I want to see my weekly task completion percentage,
so that I can understand my productivity trends over time.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `GET /metrics/weekly`) exists that returns the percentage of tasks completed within the current week.
2.  The calculation is clearly defined (e.g., `(completed_tasks_this_week / total_tasks_due_this_week) * 100`).
3.  The Next.js app displays this percentage in a visually clear way (e.g., with a progress bar or a chart).
4.  The calculation is performant and scalable, even for users with a large number of tasks.

## Epic 2: User Management

**Expanded Goal:** This epic aims to implement the core functionalities for user identity and access, enabling users to securely create accounts, log in, log out, and manage their basic profile information. This is a critical step to personalize the application and secure user data, building upon the foundational setup established in Epic 1.

### Story 2.1: User Registration

As a new user,
I want to create a secure account,
so that I can access the application's features.

**Acceptance Criteria:**
1.  A public API endpoint (e.g., `POST /register`) exists on the FastAPI backend that accepts an email and password.
2.  The password is securely hashed (e.g., using `passlib`) before being stored in the `users` table.
3.  The endpoint returns a success response upon creating a new user.
4.  The endpoint returns a 400-level error if the email already exists or if the password doesn't meet basic strength requirements (e.g., minimum length).
5.  A registration form is created in the Next.js frontend that successfully calls this endpoint.

### Story 2.2: User Login and Logout

As a registered user,
I want to securely log in and log out of the application,
so that I can access my personal data and protect my privacy.

**Acceptance Criteria:**
1.  A public API endpoint (e.g., `POST /token`) exists on the FastAPI backend that accepts an email and password.
2.  Upon successful authentication, the endpoint returns a secure access token (e.g., JWT).
3.  The endpoint returns a 401 Unauthorized error for invalid credentials.
4.  A login form in the Next.js app calls this endpoint and securely stores the token upon success.
5.  The Next.js app has a "Logout" button that clears the stored token, effectively ending the user's session.

### Story 2.3: Basic User Profile Management

As a logged-in user,
I want to view and update my basic profile information (e.g., email, password),
so that I can manage my account details.

**Acceptance Criteria:**
1.  A protected API endpoint (e.g., `GET /users/me`) exists that returns the current user's information based on their authentication token.
2.  A protected API endpoint (e.g., `PUT /users/me/password`) allows the user to change their password after verifying their current password.
3.  All protected endpoints return a 401 Unauthorized error if a valid token is not provided.
4.  A basic "Profile" page exists in the Next.js app, displaying the user's email and providing a form to change their password.

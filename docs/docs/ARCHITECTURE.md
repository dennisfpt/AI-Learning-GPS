# AI Learning GPS — Architecture

## 1. Overview

AI Learning GPS consists of four main parts:

```text
Android App
     ↓
Backend API
     ↓
Database
     ↓
AI API
```

The Android application is the frontend.

The backend is the main server that controls the application's logic.

The database stores application data.

The AI API provides AI capabilities.

---

# 2. System Architecture

```text
                    AI LEARNING GPS
                          │
                          │
                   Android App
                    Java + XML
                          │
                          │ HTTPS / REST API
                          ▼
                    Backend Server
                          │
             ┌────────────┼────────────┐
             │            │            │
             ▼            ▼            ▼
         Database     AI Service    Authentication
             │            │
             │            ▼
             │       Claude / Gemini
             │
             ▼
        User / Goal / Plan
        Task / Progress
```

---

# 3. Android Frontend

The Android application is built with:

- Android Studio
- Java
- XML layouts

The Android app is responsible for:

- Displaying the user interface
- Receiving user input
- Displaying goals
- Displaying learning plans
- Displaying tasks
- Showing progress
- Sending requests to the backend
- Receiving API responses
- Handling loading and error states

The Android app should NOT contain:

- AI secret API keys
- Database credentials
- Backend business logic

---

# 4. Backend

The backend is responsible for the main application logic.

Responsibilities:

- Authentication
- Authorization
- User management
- Goal management
- Learning plan management
- Task management
- Progress management
- Database communication
- AI API communication
- API validation
- Security

Example:

```text
Android

POST /api/goals

        ↓

Backend

Validate request
        ↓
Save goal
        ↓
Return response

        ↓

Android
```

---

# 5. Database

The database stores persistent application data.

Main entities:

```text
User
Goal
Plan
Task
Progress
Chat
```

Relationship:

```text
User
 │
 └── Goals
       │
       └── Plan
             │
             └── Tasks
```

The Android application should not directly access the production database.

Database communication should happen through the backend.

---

# 6. AI Service

The AI service is responsible for intelligent features.

Possible AI providers:

- Claude
- Gemini

The AI should be accessed through the backend.

Correct architecture:

```text
Android
   ↓
Backend
   ↓
AI API
   ↓
Backend
   ↓
Android
```

Incorrect architecture:

```text
Android
   ↓
AI API
```

The AI API key must remain on the server.

---

# 7. Goal Generation Flow

Example:

User enters:

```text
I want to learn Python in 3 months.
```

Android sends:

```text
POST /api/plans/generate
```

Backend receives the request.

Backend:

1. Validates the user
2. Validates the goal
3. Creates an AI request
4. Sends the request to the AI provider
5. Receives the AI response
6. Validates the AI response
7. Saves the generated plan
8. Returns the plan to Android

Flow:

```text
User
 ↓
Android
 ↓
Backend
 ↓
Claude / Gemini
 ↓
Backend
 ↓
Database
 ↓
Android
 ↓
User
```

---

# 8. API Communication

The Android application communicates with the backend using REST APIs.

Example:

```text
GET    /api/goals
POST   /api/goals
GET    /api/goals/{id}
PUT    /api/goals/{id}
DELETE /api/goals/{id}

POST   /api/plans/generate

GET    /api/tasks/today
PUT    /api/tasks/{id}

POST   /api/ai/chat
```

The exact API specification will be documented in:

```text
docs/API.md
```

---

# 9. Security

Important rules:

1. Never store AI API keys inside the Android application.
2. Never store database passwords inside the Android application.
3. Backend validates requests.
4. Backend verifies authenticated users.
5. Production communication should use HTTPS.
6. Sensitive configuration should use environment variables or secure secret management.

---

# 10. Development Environment

Development is divided into two main areas:

```text
Developer 1
Android + Database

Developer 2
Backend + API + AI
```

Both developers work through GitHub.

Each feature should normally be developed on a separate branch.

Example:

```text
main
│
├── feature/android-home
├── feature/android-goal
├── feature/backend-goal-api
└── feature/ai-plan-generation
```

Completed work is merged into `main` through Pull Requests.

---

# 11. Development Principle

Frontend and backend should be developed independently through a defined API contract.

The Android developer can use Mock Data before the backend API is finished.

Once the backend API is ready, the Android application can replace Mock Data with real API responses.

This allows frontend and backend development to happen in parallel.

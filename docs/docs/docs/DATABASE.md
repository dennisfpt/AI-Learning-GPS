# AI Learning GPS — Database Specification

## 1. Purpose

This document defines the data structure used by AI Learning GPS.

The database stores:

- Users
- Goals
- Learning Plans
- Tasks
- Progress
- Chat messages

The Android application communicates with the database through the Backend API.

```text
Android
   ↓
Backend API
   ↓
Database
```

---

# 2. Entity Relationship

The basic relationship is:

```text
User
 │
 ├── Goal
 │     │
 │     └── Plan
 │           │
 │           └── Task
 │
 └── Chat
```

One user can have multiple goals.

One goal can have one active learning plan.

One learning plan can contain multiple tasks.

---

# 3. User

Stores user account information.

### Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Unique user ID |
| name | String | User's display name |
| email | String | User email |
| created_at | DateTime | Account creation time |
| updated_at | DateTime | Last update time |

Example:

```json
{
  "id": "user_001",
  "name": "Yen",
  "email": "user@example.com"
}
```

---

# 4. Goal

A Goal represents something the user wants to achieve.

The system must NOT restrict goals to predefined subjects.

Examples:

- Learn Java
- Learn English
- Learn Japanese
- Learn Photoshop
- Prepare for an exam

### Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Unique goal ID |
| user_id | UUID | Owner of the goal |
| title | String | Goal title |
| description | Text | Goal description |
| deadline | Date | Target date |
| status | String | Goal status |
| progress | Integer | Progress percentage |
| created_at | DateTime | Creation time |
| updated_at | DateTime | Last update time |

Example:

```json
{
  "id": "goal_001",
  "user_id": "user_001",
  "title": "Learn Python",
  "description": "Learn Python from beginner to intermediate",
  "deadline": "2027-01-01",
  "status": "active",
  "progress": 35
}
```

---

# 5. Learning Plan

A Learning Plan contains the roadmap generated for a Goal.

### Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Unique plan ID |
| goal_id | UUID | Related goal |
| title | String | Plan title |
| description | Text | Plan description |
| version | Integer | Plan version |
| status | String | Plan status |
| created_at | DateTime | Creation time |
| updated_at | DateTime | Last update time |

Example:

```json
{
  "id": "plan_001",
  "goal_id": "goal_001",
  "title": "Python Learning Roadmap",
  "description": "A 12-week Python learning plan",
  "version": 1,
  "status": "active"
}
```

---

# 6. Task

A Task is an individual action inside a Learning Plan.

### Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Unique task ID |
| plan_id | UUID | Related learning plan |
| title | String | Task title |
| description | Text | Task description |
| due_date | Date | Task date |
| duration_minutes | Integer | Estimated duration |
| completed | Boolean | Completion status |
| completed_at | DateTime | Completion time |
| created_at | DateTime | Creation time |

Example:

```json
{
  "id": "task_001",
  "plan_id": "plan_001",
  "title": "Learn Python variables",
  "description": "Study variables and basic data types",
  "due_date": "2026-10-01",
  "duration_minutes": 30,
  "completed": false
}
```

---

# 7. Chat

Stores conversations between the user and the AI assistant.

### Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Unique message ID |
| user_id | UUID | User |
| goal_id | UUID | Related goal |
| role | String | user / assistant |
| message | Text | Message content |
| created_at | DateTime | Creation time |

Example:

```json
{
  "id": "chat_001",
  "user_id": "user_001",
  "goal_id": "goal_001",
  "role": "user",
  "message": "Can you explain Python functions?"
}
```

---

# 8. Relationships

```text
User
 │
 │ 1:N
 ▼
Goal
 │
 │ 1:N
 ▼
Plan
 │
 │ 1:N
 ▼
Task
```

Chat:

```text
User
 │
 └──── 1:N ──── Chat

Goal
 │
 └──── 1:N ──── Chat
```

---

# 9. Progress

Progress should normally be calculated from completed tasks.

Example:

```text
Total tasks = 10
Completed tasks = 4

Progress = 40%
```

The backend should calculate the official progress value.

The Android application should display the value returned by the backend.

---

# 10. Important Database Rules

### Rule 1 — No hard-coded subjects

Do not create separate database tables for:

```text
English
Programming
Math
Japanese
```

Goals must be dynamic.

---

### Rule 2 — Use IDs for relationships

Example:

```text
Goal.user_id → User.id

Plan.goal_id → Goal.id

Task.plan_id → Plan.id
```

---

### Rule 3 — Backend controls database access

Android should not connect directly to the production database.

Correct:

```text
Android
   ↓
Backend API
   ↓
Database
```

---

### Rule 4 — AI output must be validated

AI-generated plans should not be saved directly without validation.

Backend should verify:

- Required fields exist
- Data types are correct
- Task information is valid
- Goal and user relationships are valid

---

# 11. Future Expansion

The database may later support:

- Learning resources
- Books
- Courses
- Notes
- Quizzes
- Achievements
- Notifications
- Study sessions
- User preferences
- AI memory/context

These should be added only when the corresponding features are implemented.

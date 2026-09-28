# AI Learning GPS — API Specification

## 1. Purpose

This document defines the communication contract between the Android application and the Backend Server.

The Android application communicates with the backend using REST APIs.

```text
Android App
     ↓
HTTP Request
     ↓
Backend API
     ↓
HTTP Response
     ↓
Android App
```

---

# 2. Base URL

Development:

```text
http://localhost:8080/api
```

Production URL will be defined later.

The Android application must not hard-code the production URL in multiple places.

---

# 3. Response Format

Successful responses should use JSON.

Example:

```json
{
  "success": true,
  "data": {}
}
```

Error responses:

```json
{
  "success": false,
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Invalid request"
  }
}
```

---

# 4. Authentication

Authentication will be added to protected endpoints.

Example:

```text
Authorization: Bearer <access_token>
```

Protected endpoints must verify the authenticated user before accessing user data.

---

# 5. Goals API

## 5.1 Create Goal

```text
POST /api/goals
```

### Request

```json
{
  "title": "Learn Python",
  "description": "Learn Python from beginner to intermediate",
  "deadline": "2027-01-01"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "goal_001",
    "title": "Learn Python",
    "description": "Learn Python from beginner to intermediate",
    "deadline": "2027-01-01",
    "status": "active",
    "progress": 0
  }
}
```

---

# 6. Get Goals

```text
GET /api/goals
```

Returns the goals belonging to the authenticated user.

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": "goal_001",
      "title": "Learn Python",
      "progress": 35,
      "status": "active"
    }
  ]
}
```

---

# 7. Get Goal

```text
GET /api/goals/{goalId}
```

Example:

```text
GET /api/goals/goal_001
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "goal_001",
    "title": "Learn Python",
    "description": "Learn Python from beginner to intermediate",
    "deadline": "2027-01-01",
    "status": "active",
    "progress": 35
  }
}
```

---

# 8. Update Goal

```text
PUT /api/goals/{goalId}
```

### Request

```json
{
  "title": "Learn Python",
  "description": "Updated description",
  "deadline": "2027-02-01"
}
```

---

# 9. Delete Goal

```text
DELETE /api/goals/{goalId}
```

### Response

```json
{
  "success": true
}
```

---

# 10. Generate Learning Plan

This endpoint asks the AI to generate a learning plan for a goal.

```text
POST /api/plans/generate
```

### Request

```json
{
  "goalId": "goal_001"
}
```

### Backend process

```text
Android
   ↓
POST /api/plans/generate
   ↓
Backend
   ↓
Get Goal
   ↓
Create AI Prompt
   ↓
Claude / Gemini
   ↓
Validate AI Response
   ↓
Save Plan
   ↓
Return Plan
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "plan_001",
    "goalId": "goal_001",
    "title": "Python Learning Roadmap",
    "description": "12-week Python learning plan",
    "version": 1,
    "status": "active"
  }
}
```

---

# 11. Get Learning Plan

```text
GET /api/goals/{goalId}/plan
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "plan_001",
    "goalId": "goal_001",
    "title": "Python Learning Roadmap",
    "version": 1,
    "status": "active"
  }
}
```

---

# 12. Get Tasks

```text
GET /api/goals/{goalId}/tasks
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": "task_001",
      "title": "Learn Python variables",
      "description": "Study variables and basic data types",
      "dueDate": "2026-10-01",
      "durationMinutes": 30,
      "completed": false
    }
  ]
}
```

---

# 13. Get Today's Tasks

```text
GET /api/tasks/today
```

Returns tasks scheduled for the current day for the authenticated user.

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": "task_001",
      "title": "Learn Python variables",
      "durationMinutes": 30,
      "completed": false
    }
  ]
}
```

---

# 14. Complete Task

```text
PUT /api/tasks/{taskId}
```

### Request

```json
{
  "completed": true
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "task_001",
    "completed": true,
    "completedAt": "2026-09-28T10:00:00Z"
  }
}
```

The backend should update the related goal progress after task completion.

---

# 15. AI Chat

```text
POST /api/ai/chat
```

### Request

```json
{
  "goalId": "goal_001",
  "message": "Can you explain Python functions?"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "message": "A function is a reusable block of code..."
  }
}
```

The Android application communicates only with the backend.

The backend communicates with Claude or Gemini.

---

# 16. Error Handling

Common HTTP status codes:

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
429 Too Many Requests
500 Internal Server Error
```

Example:

```json
{
  "success": false,
  "error": {
    "code": "GOAL_NOT_FOUND",
    "message": "Goal not found"
  }
}
```

---

# 17. API Rules

1. All protected endpoints require authentication.
2. Users can only access their own goals, plans, and tasks.
3. Backend validates all requests.
4. Backend validates AI-generated data before saving it.
5. AI API keys must never be sent to Android.
6. Database credentials must never be sent to Android.
7. API responses should use consistent JSON structures.
8. API changes should be documented in this file.

---

# 18. Development Status

| Endpoint | Status |
|---|---|
| POST /api/goals | Planned |
| GET /api/goals | Planned |
| GET /api/goals/{id} | Planned |
| PUT /api/goals/{id} | Planned |
| DELETE /api/goals/{id} | Planned |
| POST /api/plans/generate | Planned |
| GET /api/goals/{id}/plan | Planned |
| GET /api/goals/{id}/tasks | Planned |
| GET /api/tasks/today | Planned |
| PUT /api/tasks/{id} | Planned |
| POST /api/ai/chat | Planned |

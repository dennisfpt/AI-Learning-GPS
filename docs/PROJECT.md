# AI Learning GPS

## 1. Project Overview

AI Learning GPS is an AI-powered application that helps users turn their goals into personalized learning plans.

The idea is similar to a GPS:

> The user chooses where they want to go, and AI helps determine the path to get there.

The application is not limited to one subject.

Users can create goals such as:

- Learn Java
- Learn Python
- Improve English
- Learn Japanese
- Learn mathematics
- Prepare for an exam
- Learn design
- Read and understand a book
- Learn any other skill

The system should be flexible enough to support different types of learning goals.

---

## 2. Main Product Flow

```text
User Goal
    ↓
AI analyzes the goal
    ↓
AI creates a learning plan
    ↓
Plan is divided into tasks
    ↓
User completes tasks
    ↓
Progress is recorded
    ↓
AI adjusts the plan
```

---

## 3. MVP

The first version of AI Learning GPS should include:

### User

- Account
- Login
- User profile

### Goals

- Create a goal
- View goals
- Edit a goal
- Delete a goal
- Track goal progress

### Learning Plan

- Generate a plan using AI
- Divide the plan into stages
- Divide stages into tasks
- View the current plan

### Tasks

- View today's tasks
- Mark tasks as completed
- Track task progress

### AI Assistant

- Ask questions about the current learning goal
- Explain difficult concepts
- Help adjust the learning plan

---

## 4. Important Product Principle

AI Learning GPS must NOT be designed around only one subject.

Do not hard-code subjects such as:

```text
English
Programming
Mathematics
```

Instead, the system should allow the user to enter any learning goal.

The goal and its learning plan should be dynamic.

---

## 5. Technology

### Android

- Android Studio
- Java
- XML

### Backend

- Backend API server
- REST API

### AI

- Claude API and/or Gemini API

### Database

- Database connected through the backend

---

## 6. Team Responsibilities

### Developer 1 — Android + Database

Responsible for:

- Android frontend
- UI implementation
- Navigation
- Android data models
- Database design
- API integration

### Developer 2 — Backend + API + AI

Responsible for:

- Backend server
- REST APIs
- Authentication
- Business logic
- Database integration
- AI API integration
- AI prompts
- API security

---

## 7. Development Principle

The Android application should communicate with the backend through APIs.

```text
Android
   ↓
Backend API
   ↓
Database
```

For AI features:

```text
Android
   ↓
Backend API
   ↓
AI API
   ↓
Backend
   ↓
Android
```

The Android application must not contain secret AI API keys.

---

## 8. Development Strategy

The project will be developed in small tasks.

Each task should define:

- What needs to be built
- Expected result
- Files/components affected
- Definition of Done

Developers should work using Git branches and Pull Requests.

The `main` branch should contain stable code.

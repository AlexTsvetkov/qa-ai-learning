# Module 3: Project — Complete Test Suite for a Demo App

## 🎯 Objective
Create a comprehensive test suite for a todo application using AI assistance.

## ⏱️ Время: 4 часов

---

## Demo Application Specification

**Todo App** with these features:
- Create, read, update, delete todos
- Mark todos as complete/incomplete
- Filter by status (all, active, completed)
- Search todos by text
- Set due dates and priorities (low, medium, high)
- Bulk actions (complete all, delete completed)

**API Endpoints:**
- `GET /api/todos` — list all (with query params for filter/search)
- `POST /api/todos` — create new
- `GET /api/todos/{id}` — get single
- `PUT /api/todos/{id}` — update
- `DELETE /api/todos/{id}` — delete
- `POST /api/todos/bulk-complete` — complete all
- `DELETE /api/todos/completed` — delete completed

## Результаты

### 1. Test Cases (`exercises/module03_test_cases.md`)
Generate 30+ test cases covering all features using AI. Organize by feature.

### 2. Test Data (`exercises/module03_test_data.json`)
Generate comprehensive test data including valid, boundary, and adversarial data.

### 3. API Tests (`exercises/module03_api_tests.py`)
Generate automated API tests using pytest + requests for all endpoints.

### 4. Review Report (`exercises/module03_review.md`)
Have AI review your test suite and document the feedback.

## Критерии оценки

| Criteria | Points |
|----------|--------|
| Test case coverage and quality | 25 |
| Test data variety | 20 |
| API test code quality | 25 |
| AI review documented | 15 |
| Personal improvements beyond AI output | 15 |
| **Total** | **100** |

> **Следующий модуль**: [Module 4 — AI-Powered Analysis & Debugging](../../module-04-ai-analysis-debugging/README.md)


> 🌐 [English version](../module-03-ai-test-creation/project.md)

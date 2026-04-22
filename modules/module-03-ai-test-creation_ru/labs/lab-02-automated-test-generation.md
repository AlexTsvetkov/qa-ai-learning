# Lab 2: Generating Automated Tests with AI

## 🎯 Objective
Use AI to generate automated tests for both UI and API testing, then review, fix, and run them.

## ⏱️ Время: 2.5 часов

---

## Упражнение 1: API Test Generation

Use AI to generate pytest tests for this API specification:

```
POST /api/users
- Creates a new user
- Body: { "name": string, "email": string, "password": string }
- Returns: 201 with user object (without password)
- Errors: 400 (validation), 409 (email exists)

GET /api/users/{id}
- Returns user by ID
- Returns: 200 with user object
- Errors: 404 (not found)

PUT /api/users/{id}
- Updates user
- Body: partial user object
- Returns: 200 with updated user
- Errors: 400, 404

DELETE /api/users/{id}
- Deletes user
- Returns: 204
- Errors: 404
```

**Task**: 
1. Ask AI to generate comprehensive API tests with pytest + requests
2. Review the generated code for correctness
3. Identify and fix any issues
4. Save to `exercises/test_api_users.py`

## Упражнение 2: UI Test Generation

Ask AI to generate Playwright (Python) tests for a login page:

**Task**:
1. Generate Page Object for login page
2. Generate at least 5 UI tests
3. Include proper wait strategies (no sleep!)
4. Save to `exercises/test_login_ui.py`

## Упражнение 3: Test Review with AI

Take one of the generated test files and ask AI to review it. Document:
1. What improvements did AI suggest?
2. Were the suggestions valid?
3. What did AI miss?

---

## ✅ Completion Checklist
- [ ] API tests generated and reviewed
- [ ] UI tests with Page Object generated
- [ ] AI review of test code completed
- [ ] All issues identified and documented


> 🌐 [English version](../module-03-ai-test-creation/labs/lab-02-automated-test-generation.md)

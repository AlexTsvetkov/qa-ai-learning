# Lab 2: AI API in Test Frameworks

## 🎯 Objective
Build a pytest plugin that provides AI-powered test helpers and automatic failure analysis.

## ⏱️ Время: 3 часов

---

## Упражнение 1: pytest conftest with AI

Create a `conftest.py` that:
1. Initializes the AI helper on test session start
2. Automatically analyzes any test failure
3. Adds AI analysis to the test report

## Упражнение 2: AI-Powered Test Data Fixtures

Create pytest fixtures that use AI to generate test data:

```python
@pytest.fixture
def ai_test_users():
    """Generate test user data using AI."""
    # Use AITestHelper to generate realistic test data
    pass
```

## Упражнение 3: Smart Test Report

Create a pytest plugin that generates an AI-powered test summary after the test run:
1. Collects all results
2. Sends summary to AI
3. Outputs an executive report with risk assessment

Save all scripts to `exercises/` directory.

---

## ✅ Completion Checklist
- [ ] conftest.py with AI failure analysis
- [ ] AI test data fixtures working
- [ ] Smart test report generated


> 🌐 [English version](../module-05-ai-testing-pipelines/labs/lab-02-ai-api-testing.md)

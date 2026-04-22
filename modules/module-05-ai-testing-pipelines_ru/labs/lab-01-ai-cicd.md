# Lab 1: AI in CI/CD Pipelines

## 🎯 Objective
Create a GitHub Actions workflow that uses AI to analyze test failures and suggest tests for code changes.

## ⏱️ Время: 3 часов

---

## Упражнение 1: Create AI Helper Module

Create `scripts/ai_test_helper.py` with the `AITestHelper` class from theory.

## Упражнение 2: Build Failure Analysis Script

Create `scripts/ai_analyze_failures.py` that:
1. Parses pytest output from a file
2. Extracts failed test names and error messages
3. Sends each failure to AI for analysis
4. Outputs a markdown report

## Упражнение 3: Create GitHub Actions Workflow

Create `.github/workflows/ai-testing.yml` based on the template from theory. Test it by:
1. Creating a branch with a failing test
2. Opening a PR
3. Verifying AI analysis appears in the workflow output

---

## ✅ Completion Checklist
- [ ] AI helper module created
- [ ] Failure analysis script working
- [ ] GitHub Actions workflow configured
- [ ] End-to-end test with a PR


> 🌐 [English version](../module-05-ai-testing-pipelines/labs/lab-01-ai-cicd.md)

# Lab 1: Generating Test Cases with AI

## 🎯 Objective
Practice generating test cases from requirements using AI, then review and improve the results.

## ⏱️ Время: 2.5 часов

---

## Упражнение 1: Test Cases from User Stories

Use the following user story and ask AI to generate test cases:

```
User Story: As a registered user, I want to create a new post with text, 
images, and tags so that I can share content with my followers.

Acceptance Criteria:
- Post text: 1-5000 characters, supports markdown
- Images: up to 4 images, max 10MB each, JPG/PNG/GIF
- Tags: up to 10 tags, each 1-30 characters
- Draft save: auto-save every 30 seconds
- Preview: user can preview before publishing
- Visibility: public, followers-only, or private
```

**Task**: Generate test cases using 3 different approaches:
1. Zero-shot (just the requirements)
2. CRISP framework prompt
3. Few-shot with example test cases

Compare results and save the best set to `exercises/lab01_test_cases.md`.

## Упражнение 2: Test Data Generation

Generate test data for the post feature above:
1. 10 valid post records (JSON)
2. 10 edge case records (boundary values, special characters)
3. 5 adversarial records (XSS, SQL injection payloads in text/tags)

Save to `exercises/lab01_test_data.json`.

## Упражнение 3: Exploratory Testing Charters

Ask AI to generate 8 exploratory testing charters for the post creation feature.

---

## ✅ Completion Checklist
- [ ] Test cases generated with 3 approaches and compared
- [ ] Test data generated (valid, edge case, adversarial)
- [ ] Exploratory charters created
- [ ] All outputs reviewed and saved


> 🌐 [English version](../module-03-ai-test-creation/labs/lab-01-test-case-generation.md)

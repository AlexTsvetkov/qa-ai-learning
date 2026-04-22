# Lab 1: Practicing Prompting Techniques

## 🎯 Objective

Practice and compare different prompting techniques to understand their impact on AI output quality for QA tasks.

## ⏱️ Время: 2 часов

---

## Упражнение 1: Zero-Shot vs Few-Shot

### Task: Generate test cases for a password change feature

**Step 1**: Use zero-shot prompting:
```
Generate test cases for a password change feature.
```

**Step 2**: Use few-shot prompting with examples:
```
I need test cases for a password change feature. Here are examples of the format I want:

Example 1:
| ID | Title | Type | Steps | Expected Result |
|----|-------|------|-------|-----------------|
| PC-001 | Change password with valid data | Positive | 1. Go to Settings > Security 2. Enter current password 3. Enter new password (meets requirements) 4. Confirm new password 5. Click Save | Success message displayed, password is changed |

Example 2:
| ID | Title | Type | Steps | Expected Result |
|----|-------|------|-------|-----------------|
| PC-002 | Change password with wrong current password | Negative | 1. Go to Settings > Security 2. Enter incorrect current password 3. Enter new password 4. Confirm new password 5. Click Save | Error: "Current password is incorrect" |

Now generate 8 more test cases in the same format, covering boundary values, security, and edge cases.
```

**Step 3**: Compare the results. Document in `exercises/lab01_exercise1.md`:
- Which output was more useful?
- How did the format consistency differ?
- What edge cases did each approach catch?

---

## Упражнение 2: Chain-of-Thought for Bug Analysis

### Task: Analyze a complex bug scenario

**Step 1**: Simple prompt:
```
Why might a web application show a blank page after login?
```

**Step 2**: Chain-of-thought prompt:
```
A user reports that after clicking "Login" with valid credentials, they see a blank white page. Let's analyze this step by step:

1. First, list all the components involved in the login flow (frontend, backend, auth service, etc.)
2. For each component, identify what could go wrong
3. Then, prioritize the most likely causes based on the symptom (blank page)
4. Finally, suggest specific debugging steps for each likely cause

Please walk through each step thoroughly.
```

**Step 3**: Document which response was more helpful for actual debugging.

---

## Упражнение 3: Role-Based Prompting Comparison

### Task: Review a piece of test code

Use the following test code for all three role variations:

```python
def test_login():
    driver.get("http://localhost:3000/login")
    driver.find_element(By.ID, "email").send_keys("user@test.com")
    driver.find_element(By.ID, "password").send_keys("password123")
    driver.find_element(By.ID, "login-btn").click()
    time.sleep(5)
    assert "dashboard" in driver.current_url
```

**Role 1 — General QA:**
```
Review this test code and suggest improvements.

[paste code]
```

**Role 2 — Test Automation Expert:**
```
You are a senior test automation architect with expertise in Selenium, 
Page Object Model, and CI/CD pipelines. Review this test code and provide 
detailed feedback on:
- Design patterns
- Maintainability
- Reliability (flakiness)
- Best practices

[paste code]
```

**Role 3 — Security Tester:**
```
You are a security testing specialist. Review this test code from a 
security perspective. Identify any security concerns in both the test 
itself and the application behavior it implies.

[paste code]
```

**Step 4**: Compare how each role produced different insights.

---

## Упражнение 4: Structured Output Practice

### Task: Generate test data in different formats

**Prompt 1 — JSON:**
```
Generate 5 test user records in JSON format with fields: 
id, name, email, age, role, isActive. 
Include edge cases (empty strings, boundary ages, special characters).
```

**Prompt 2 — CSV:**
```
Generate the same 5 test user records in CSV format.
```

**Prompt 3 — Python fixtures:**
```
Generate the same test data as Python pytest fixtures using @pytest.fixture decorator.
```

Document which format was most useful for your workflow.

---

## Упражнение 5: CRISP Framework Practice

### Task: Write a CRISP prompt from scratch

Choose one of these scenarios and write a complete CRISP prompt:

1. **E-commerce**: Test the "Add to Wishlist" feature
2. **Banking**: Test the "Transfer Funds" feature
3. **Social Media**: Test the "Create Post" feature

**Template:**
```
Context: [Describe the application and feature]
Role: [Define the AI's expertise]
Instructions: [What specific task to perform]
Scope: [Boundaries and constraints]
Pattern: [Desired output format]
```

Send your CRISP prompt to an AI and evaluate the result. Iterate at least twice to improve the output.

---

## ✅ Completion Checklist

- [ ] Exercise 1: Zero-shot vs Few-shot comparison completed
- [ ] Exercise 2: Chain-of-thought analysis completed
- [ ] Exercise 3: Role-based prompting comparison completed
- [ ] Exercise 4: Structured output formats tested
- [ ] Exercise 5: CRISP prompt written and iterated

## 🔗 Next

Continue to [Lab 2: Building a QA Prompt Library](lab-02-qa-prompt-library.md)


> 🌐 [English version](../module-02-prompt-engineering/labs/lab-01-prompting-techniques.md)

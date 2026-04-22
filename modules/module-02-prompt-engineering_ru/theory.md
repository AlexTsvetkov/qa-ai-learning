# Module 2: Theory — Prompt Engineering for Testers

## 2.1 What is Prompt Engineering?

**Prompt engineering** is the practice of designing and refining inputs (prompts) to AI models to get the best possible outputs. Think of it as learning how to communicate effectively with an AI assistant.

### Why Does It Matter for QA?

The difference between a vague prompt and a well-crafted one can be the difference between:
- ❌ Generic, unusable test cases
- ✅ Detailed, project-specific test scenarios with edge cases

**Example:**

```
❌ Bad prompt:  "Write some tests for login"

✅ Good prompt: "You are a senior QA engineer. Generate test cases for a 
   login page with email/password fields. Include:
   - 5 positive scenarios
   - 5 negative scenarios  
   - 3 boundary value tests
   - 3 security tests
   Format: ID | Title | Preconditions | Steps | Expected Result | Priority"
```

---

## 2.2 Core Prompting Techniques

### Technique 1: Zero-Shot Prompting

**Zero-shot** means giving AI a task without any examples. You rely on the model's training to understand what you need.

```
Prompt: "List 5 potential security vulnerabilities in a file upload feature."
```

**When to use**: Simple, well-understood tasks where the model has strong training data.

**QA Example:**
```
Generate a checklist for API testing of a REST endpoint that 
creates a new user (POST /api/users).
```

### Technique 2: Few-Shot Prompting

**Few-shot** means providing examples of the desired output before asking for new output. This "teaches" the model your expected format.

```
Prompt:
"Here are examples of well-written test cases:

Example 1:
- ID: TC-001
- Title: Successful login with valid credentials
- Steps: 1) Navigate to login page 2) Enter valid email 3) Enter valid password 4) Click Login
- Expected: User is redirected to dashboard
- Priority: High

Example 2:
- ID: TC-002
- Title: Login with empty email field
- Steps: 1) Navigate to login page 2) Leave email empty 3) Enter password 4) Click Login
- Expected: Error message 'Email is required' is displayed
- Priority: Medium

Now generate 5 more test cases for the same login page in the same format."
```

**When to use**: When you need specific formatting or style consistency.

### Technique 3: Chain-of-Thought (CoT) Prompting

**Chain-of-thought** asks the AI to reason step by step before providing an answer. This dramatically improves quality for complex tasks.

```
Prompt:
"I need to test a shopping cart discount feature. Let's think step by step:

1. First, identify all the input variables (discount code, cart total, items, user type)
2. Then, determine the boundary values for each variable
3. Next, identify potential interactions between variables
4. Finally, generate test cases that cover these scenarios

Please walk through each step and then provide the test cases."
```

**When to use**: Complex analysis, bug investigation, test strategy design.

### Technique 4: Role-Based Prompting

Assign a specific role to the AI to get domain-expert responses.

```
Prompt:
"You are a senior QA engineer with 10 years of experience in e-commerce 
testing. You specialize in payment system testing and have deep knowledge 
of PCI DSS compliance requirements.

Review the following checkout flow and identify potential issues..."
```

**Common QA roles to assign:**
- Senior QA Engineer
- Security Testing Specialist
- Performance Testing Expert
- Accessibility Testing Expert
- Test Architect

### Technique 5: Structured Output Prompting

Tell the AI exactly how to format the output.

```
Prompt:
"Generate test cases in the following JSON format:

{
  "test_cases": [
    {
      "id": "TC-XXX",
      "title": "string",
      "preconditions": ["string"],
      "steps": ["string"],
      "expected_result": "string",
      "priority": "high|medium|low",
      "type": "positive|negative|boundary|security"
    }
  ]
}

Generate 5 test cases for user registration."
```

**When to use**: When you need machine-readable output or consistent format.

---

## 2.3 The CRISP Framework for QA Prompts

Use the **CRISP** framework to structure your QA prompts:

| Letter | Element | Description |
|--------|---------|-------------|
| **C** | Context | Background information about the project/feature |
| **R** | Role | The persona the AI should adopt |
| **I** | Instructions | Specific task to perform |
| **S** | Scope | Boundaries and constraints |
| **P** | Pattern | Desired output format |

**Example using CRISP:**

```
**Context**: We have an e-commerce web application built with React and 
Node.js. The checkout process has 3 steps: cart review, shipping info, 
payment. We use Stripe for payments.

**Role**: You are a senior QA engineer specializing in e-commerce testing.

**Instructions**: Generate test cases for the payment step of checkout, 
focusing on error handling and edge cases.

**Scope**: Focus only on the payment step. Assume cart and shipping are 
already validated. Cover credit card, debit card, and PayPal payment methods.

**Pattern**: Format each test case as:
- ID: PAY-XXX
- Title: [descriptive title]
- Preconditions: [list]
- Steps: [numbered]
- Expected Result: [clear outcome]
- Priority: [High/Medium/Low]
- Category: [Functional/Security/UX/Performance]
```

---

## 2.4 Prompt Templates for Common QA Tasks

### Template 1: Test Case Generation

```
You are a QA engineer. Generate comprehensive test cases for:

Feature: [FEATURE_NAME]
Description: [FEATURE_DESCRIPTION]
Requirements: [KEY_REQUIREMENTS]

Include:
- Positive scenarios (happy path)
- Negative scenarios (error handling)
- Boundary value tests
- Edge cases
- [Additional categories as needed]

Format each test case with: ID, Title, Preconditions, Steps, Expected Result, Priority
```

### Template 2: Bug Report Enhancement

```
You are a QA engineer. Improve this bug report to make it more professional 
and actionable:

Original bug report:
[PASTE_BUG_REPORT]

Please:
1. Rewrite with clear title following the pattern: [Component] - [Action] - [Issue]
2. Add detailed reproduction steps
3. Specify environment details that should be included
4. Suggest appropriate severity and priority
5. Add any missing fields (expected vs actual behavior, screenshots suggestion, etc.)
```

### Template 3: Test Data Generation

```
Generate test data for [FEATURE/ENTITY] with the following constraints:
- Fields: [LIST_FIELDS]
- Business rules: [LIST_RULES]
- Include: valid data, boundary values, invalid data, special characters, 
  Unicode, null values, maximum length values

Format as [JSON/CSV/TABLE].
Generate [N] records.
```

### Template 4: Requirements Analysis

```
You are a QA engineer reviewing requirements. Analyze the following 
requirement and identify:

1. Ambiguities (unclear or vague statements)
2. Missing information (what's not specified but should be)
3. Potential conflicts (contradictions)
4. Testability issues (hard to verify)
5. Edge cases to consider

Requirement:
[PASTE_REQUIREMENT]
```

### Template 5: Code Review for Tests

```
You are a senior test automation engineer. Review the following test code and provide feedback on:

1. Test coverage completeness
2. Assertion quality and accuracy
3. Code maintainability
4. Best practices compliance
5. Missing scenarios
6. Potential flakiness issues

```[language]
[PASTE_TEST_CODE]
```

Provide specific suggestions for improvement with code examples.
```

---

## 2.5 Common Prompt Anti-Patterns

### ❌ Anti-Pattern 1: Vague Prompts
```
Bad:  "Test this feature"
Good: "Generate 10 test cases for the user registration feature, covering 
       email validation, password strength, and duplicate account handling"
```

### ❌ Anti-Pattern 2: Information Overload
```
Bad:  [5 pages of requirements dumped with no direction]
Good: "Here is the requirement for the search feature. Focus specifically 
       on the filter functionality and generate boundary value tests."
```

### ❌ Anti-Pattern 3: No Format Specification
```
Bad:  "Give me test cases"
Good: "Give me test cases in this format: ID | Title | Steps | Expected Result"
```

### ❌ Anti-Pattern 4: Assuming Context
```
Bad:  "Test the button" (which button? what app?)
Good: "Test the 'Submit Order' button on the checkout page of our 
       e-commerce web application"
```

### ❌ Anti-Pattern 5: Single Monolithic Prompt
```
Bad:  "Write all tests for the entire application"
Good: Break into focused prompts per feature/module
```

---

## 2.6 Iterative Prompt Refinement

Prompting is iterative. Follow this cycle:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Write    │───▶│  Send    │───▶│  Evaluate │───▶│  Refine  │
│  prompt   │    │  to AI   │    │  output   │    │  prompt  │──┐
└──────────┘    └──────────┘    └──────────┘    └──────────┘  │
     ▲                                                          │
     └──────────────────────────────────────────────────────────┘
```

**Refinement strategies:**
1. **Add constraints**: "Also include security test cases"
2. **Provide examples**: "Here's an example of what I want..."
3. **Narrow scope**: "Focus only on negative scenarios"
4. **Change format**: "Present as a table instead of a list"
5. **Add context**: "This is for a banking application with strict compliance needs"

---

## 📖 Итоги модуля

1. **Prompt engineering** is a skill that improves with practice
2. Use **few-shot** prompting for consistent output format
3. Use **chain-of-thought** for complex analytical tasks
4. The **CRISP framework** helps structure effective prompts
5. Always **iterate** — your first prompt is rarely the best one
6. Build a **library** of proven prompts for reuse

> **Next**: [Lab 1 — Practicing Prompting Techniques](labs/lab-01-prompting-techniques.md)


> 🌐 [English version](../module-02-prompt-engineering/theory.md)

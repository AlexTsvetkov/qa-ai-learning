# Module 3: Theory — AI-Assisted Test Creation

## 3.1 AI for Test Case Generation

### The Test Case Generation Workflow

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Requirements  │───▶│ AI generates  │───▶│ QA reviews   │───▶│ Final test   │
│ / User Story  │    │ draft cases   │    │ & refines    │    │ suite        │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

### Strategy 1: Requirements-Based Generation

Feed the AI your requirements document and ask for test cases:

```
Given this user story:
"As a customer, I want to filter products by price range, 
category, and rating so I can find products that match my preferences."

Acceptance criteria:
- Price filter: min/max slider, $0-$10000
- Category: multi-select dropdown with 20+ categories  
- Rating: 1-5 stars
- Filters can be combined
- Results update in real-time
- "Clear all filters" button resets everything

Generate comprehensive test cases covering functional, boundary, 
integration, and usability scenarios.
```

### Strategy 2: Exploratory Test Charters

AI can help generate exploratory testing charters:

```
Generate 10 exploratory testing charters for a file upload feature. 
Each charter should follow the format:
"Explore [target] with [resources] to discover [information]"

Feature details: Users can upload PDF, JPG, PNG files up to 25MB.
Files are scanned for viruses and stored in cloud storage.
```

### Strategy 3: Test Matrix Generation

For features with multiple variables:

```
Create a test matrix (pairwise testing approach) for a shipping 
calculator with these variables:
- Package weight: light (<1kg), medium (1-5kg), heavy (>5kg)
- Destination: domestic, international
- Shipping speed: standard, express, overnight
- Insurance: yes, no

Generate the minimum set of test cases that covers all pairs.
```

---

## 3.2 AI for Test Data Generation

### Why AI Excels at Test Data

AI can generate:
- **Realistic data** that looks like production data
- **Edge cases** you might not think of
- **Localized data** for different countries/languages
- **Data with relationships** between fields

### Techniques for Test Data

#### Technique 1: Schema-Based Generation

```
Generate 20 test records for a User entity with this schema:
- id: auto-increment integer
- firstName: string (2-50 chars)
- lastName: string (2-50 chars)  
- email: valid email format
- age: integer (18-120)
- country: ISO country code
- phone: valid phone format for the country
- role: enum [admin, user, moderator]

Include: 5 valid records, 5 boundary values, 5 invalid records, 
5 special character/Unicode records
```

#### Technique 2: Scenario-Based Generation

```
Generate test data for an e-commerce order system:
- 3 orders with single items
- 2 orders with multiple items (5+ items)
- 1 order with maximum items (100)
- 1 order with discount code applied
- 1 order with international shipping
- 1 order that triggers free shipping ($50+ threshold)
- 1 order at exact free shipping boundary ($50.00)

Include realistic product names, prices, and quantities.
Format as JSON.
```

#### Technique 3: Adversarial Data Generation

```
Generate adversarial test data for a name field that might break 
the application:
- SQL injection attempts
- XSS payloads
- Buffer overflow strings
- Unicode edge cases (RTL, zero-width chars, emojis)
- Names with special characters (O'Brien, María, 日本太郎)
- Extremely long names (255+ chars)
- Empty and whitespace-only values
```

---

## 3.3 AI for Automated Test Creation

### From Manual to Automated

AI can convert manual test cases to automated tests:

```
Convert these manual test cases to Playwright (Python) automated tests:

TC-001: Login with valid credentials
1. Navigate to /login
2. Enter "user@example.com" in email field
3. Enter "ValidPass123!" in password field
4. Click the Login button
5. Verify redirect to /dashboard
6. Verify welcome message contains username

TC-002: Login with invalid password
1. Navigate to /login
2. Enter "user@example.com" in email field
3. Enter "wrongpassword" in password field
4. Click the Login button
5. Verify error message "Invalid credentials"
6. Verify user stays on /login page

Use Page Object Model pattern. Include proper waits and assertions.
```

### Framework-Specific Generation

AI can generate tests for specific frameworks:

**Playwright (Python):**
```python
# AI can generate complete Page Objects + tests
class LoginPage:
    def __init__(self, page):
        self.page = page
        self.email_input = page.locator("#email")
        self.password_input = page.locator("#password")
        self.login_button = page.locator("#login-btn")
        self.error_message = page.locator(".error-msg")
    
    async def login(self, email, password):
        await self.email_input.fill(email)
        await self.password_input.fill(password)
        await self.login_button.click()
```

**pytest + requests (API):**
```python
# AI can generate API test structure
class TestUserAPI:
    BASE_URL = "http://localhost:3000/api"
    
    def test_create_user_success(self):
        payload = {"name": "John", "email": "john@test.com"}
        response = requests.post(f"{self.BASE_URL}/users", json=payload)
        assert response.status_code == 201
        assert response.json()["name"] == "John"
```

---

## 3.4 AI for Test Review and Improvement

### Code Review Prompts

Ask AI to review your existing tests:

```
Review these automated tests and identify:
1. Missing test scenarios
2. Weak assertions
3. Potential flakiness issues
4. Code duplication that could be refactored
5. Missing error handling
6. Accessibility testing gaps

[paste your test code]
```

### Test Coverage Gap Analysis

```
Here are the requirements for the search feature:
[paste requirements]

Here are the existing test cases:
[paste test cases]

Identify:
1. Requirements not covered by any test case
2. Edge cases that are missing
3. Non-functional requirements that should be tested
4. Suggested additional test cases to improve coverage
```

---

## 3.5 Best Practices for AI-Generated Tests

### ✅ Do's

1. **Always review generated tests** — AI makes mistakes
2. **Provide project context** — framework, architecture, patterns used
3. **Iterate** — first output is rarely perfect
4. **Validate assertions** — AI sometimes writes wrong expected values
5. **Check locators** — AI guesses element selectors

### ❌ Don'ts

1. **Don't trust generated selectors blindly** — verify in the actual app
2. **Don't skip running the tests** — always execute and verify
3. **Don't use hardcoded waits** from AI (like `time.sleep(5)`)
4. **Don't assume AI knows your app** — it generates generic patterns
5. **Don't generate without reviewing** — especially security-related tests

---

## 📖 Итоги модуля

1. AI dramatically speeds up **test case generation** from requirements
2. AI excels at generating **diverse test data** including edge cases
3. AI can **convert manual tests to automated code** in specific frameworks
4. Always **review and validate** AI-generated test artifacts
5. Use **iterative refinement** to improve quality of generated tests

> **Next**: [Lab 1 — Generating Test Cases with AI](labs/lab-01-test-case-generation.md)


> 🌐 [English version](../module-03-ai-test-creation/theory.md)

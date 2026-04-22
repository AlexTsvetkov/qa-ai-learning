# Lab 1: Building Self-Healing Tests

## 🎯 Objective
Implement a self-healing test framework that uses AI to fix broken locators automatically.

## ⏱️ Estimated Time: 3 hours

---

## Exercise 1: Build SelfHealingLocator

Implement the `SelfHealingLocator` class from theory. Create `exercises/self_healing.py`.

Test it against a simple HTML page:
1. Create a local HTML file with a form
2. Write a test using SelfHealingLocator
3. Change element IDs in the HTML
4. Run the test again — it should self-heal

## Exercise 2: Self-Healing Page Object

Create a Page Object that uses self-healing locators:

```python
class LoginPage:
    def __init__(self, page):
        self.page = page
        self.email = SelfHealingLocator(page, "#email", "email input field")
        self.password = SelfHealingLocator(page, "#password", "password input field")
        self.submit = SelfHealingLocator(page, "#login-btn", "login submit button")
```

## Exercise 3: Healing Report

Create a script that reads `healing_log.json` and generates a report showing:
- How many locators were healed
- Which locators failed and their new values
- Recommendations for updating the test code

---

## ✅ Completion Checklist
- [ ] SelfHealingLocator class implemented
- [ ] Self-healing demonstrated with changed HTML
- [ ] Page Object with self-healing locators created
- [ ] Healing report generator working

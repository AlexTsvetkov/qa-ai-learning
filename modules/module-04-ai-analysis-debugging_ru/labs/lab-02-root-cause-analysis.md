# Lab 2: Root Cause Analysis with AI

## 🎯 Objective
Practice using AI for systematic bug investigation and root cause analysis.

## ⏱️ Время: 2 часов

---

## Упражнение 1: The 5 Whys

Use AI to perform a 5 Whys analysis on this scenario:

**Bug**: After deploying version 2.5, 15% of users cannot complete checkout. The error message says "Payment processing failed." The payment provider API is responding normally.

**Context**: The deployment included a library update for the HTTP client (from axios 0.x to 1.x).

## Упражнение 2: Bug Classification

Take these 10 bug descriptions and use AI to classify them:

1. "App crashes when rotating phone during video upload"
2. "Prices show in wrong currency for Canadian users"
3. "Password reset email contains broken link"
4. "Search takes 30+ seconds with more than 1000 results"
5. "Screen reader cannot navigate the checkout form"
6. "User session expires after 5 minutes instead of 30"
7. "Profile picture upload accepts executable files"
8. "Date picker shows wrong month in Safari"
9. "API returns 500 when request body exceeds 1MB"
10. "Footer overlaps content on screens below 320px width"

Classify by: Type, Severity, Priority, Component.

## Упражнение 3: Flaky Test Investigation

Use AI to investigate why this test is flaky:

```python
def test_notification_appears():
    page.click("#submit-order")
    notification = page.locator(".toast-notification")
    assert notification.is_visible()
    assert notification.text_content() == "Order submitted successfully!"
```

The test passes 70% of the time. Ask AI for probable causes and fixes.

---

## ✅ Completion Checklist
- [ ] 5 Whys analysis completed
- [ ] 10 bugs classified
- [ ] Flaky test investigation done


> 🌐 [English version](../module-04-ai-analysis-debugging/labs/lab-02-root-cause-analysis.md)

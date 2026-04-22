# Lab 2: MCP Tool Invocation & Failure Handling Testing

## 🎯 Objective
Test MCP Server communication — tool invocation parameters, error handling, timeouts, and failure recovery.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Direct MCP Server Testing (45 min)

If you have access to MCP Server endpoints directly (via API), test each tool:

### KB Retrieval Server Tests

| # | Test | Request | Expected Response | Actual | Pass? |
|---|------|---------|-------------------|--------|-------|
| 1 | Valid search | query="vacation policy", top_k=5 | Returns relevant chunks | | |
| 2 | Empty query | query="", top_k=5 | Error or empty results | | |
| 3 | Very long query | query=(1000+ chars) | Handles gracefully | | |
| 4 | top_k=0 | query="vacation", top_k=0 | Error or empty | | |
| 5 | top_k=1000 | query="vacation", top_k=1000 | Reasonable limit applied | | |
| 6 | Special characters | query="<script>alert(1)</script>" | Sanitized, no error | | |
| 7 | Non-existent topic | query="quantum physics experiments" | Empty/low-score results | | |

### Query Generator Server Tests (Finance)

| # | Test | Request | Expected | Actual | Pass? |
|---|------|---------|----------|--------|-------|
| 1 | Valid financial query | "Q3 revenue by department" | Valid SQL/MDX generated | | |
| 2 | Ambiguous query | "show me money stuff" | Reasonable query or clarification | | |
| 3 | Injection attempt | "'; DROP TABLE finances; --" | Sanitized/rejected | | |
| 4 | Cross-domain query | "employee names and salaries" | Rejected (HR data) | | |

---

## Exercise 2: Failure Scenario Testing (45 min)

### Simulating MCP Failures
Work with your DevOps/dev team to simulate these failures, or observe behavior when services are restarted:

| # | Failure Type | How to Simulate | Expected Agent Behavior | Actual | Bug? |
|---|-------------|-----------------|------------------------|--------|------|
| 1 | MCP Server down | Stop KB service | Friendly error to user | | |
| 2 | Slow MCP Server | Add artificial delay | Timeout after N seconds | | |
| 3 | Partial response | Service returns incomplete JSON | Agent handles gracefully | | |
| 4 | Wrong response format | N/A (observe if it happens) | Parse error handled | | |
| 5 | Server returns 500 | Trigger known error condition | Retry or fallback | | |

### Timeout Behavior
Send a question and observe timeout handling:
1. What is the configured timeout for MCP calls? _____ seconds
2. What happens when timeout is reached? _____
3. Does the user get a meaningful message? _____
4. Is the timeout logged in Langfuse? _____

---

## Exercise 3: Tool Chaining Under Stress (30 min)

### Rapid Sequential Requests
Send 5 requests rapidly (within 10 seconds) and observe:

| Request # | Question | Response Time | Complete Trace? | Correct Answer? |
|-----------|---------|---------------|-----------------|-----------------|
| 1 | "Vacation policy" | | | |
| 2 | "Sick leave policy" | | | |
| 3 | "Overtime rules" | | | |
| 4 | "Holiday schedule" | | | |
| 5 | "Parental leave" | | | |

**Observe:**
- Does response time degrade?
- Are any traces incomplete?
- Are there any errors under concurrent load?

---

## 📝 Deliverables
1. MCP Server test results table
2. Failure scenario observations
3. Timeout behavior documentation
4. Stress test results
5. At least 2 bugs or issues found

## ✅ Completion Checklist
- [ ] Tested KB Retrieval Server with 7+ test cases
- [ ] Tested Query Generator with 4+ test cases
- [ ] Simulated or observed at least 3 failure scenarios
- [ ] Documented timeout behavior
- [ ] Completed stress test with 5 rapid requests

> **Next**: [Quiz](../quiz.md)
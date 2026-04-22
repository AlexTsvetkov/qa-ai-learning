# Lab 1: Building the Test Strategy Document

## 🎯 Objective
Create a comprehensive test strategy document for the AI platform.

## ⏱️ Estimated Time: 3 hours

---

## Exercise 1: Test Layer Definition (45 min)

For each test layer, define scope, tools, and test types:

| Layer | Scope | Tools | Test Types | # Test Cases (estimate) |
|-------|-------|-------|-----------|------------------------|
| UI | React chat interface | Browser, DevTools | Functional, UX | |
| API | .NET Backend | Postman, curl | Contract, validation | |
| Agent | Agent Runtime, MCP | Langfuse | Behavioral, boundary | |
| AI Quality | LLM responses | Langfuse, golden set | Hallucination, GRACE | |
| Data | KB, Weaviate, ingestion | KB API, Weaviate | Retrieval, ingestion | |
| Security | RBAC/ABAC, injection | Multi-role accounts | Access control, injection | |
| Infrastructure | vLLM, GPU, Redis | Grafana, Prometheus | Performance, capacity | |

---

## Exercise 2: Risk Matrix (30 min)

Create a risk matrix with at least 12 risks:

| # | Risk | Probability (1-5) | Impact (1-5) | Risk Score | Mitigation (test strategy) |
|---|------|-------------------|-------------|------------|---------------------------|
| 1 | LLM hallucinations | | | | |
| 2 | RBAC bypass | | | | |
| 3-12 | [your risks] | | | | |

---

## Exercise 3: Critical Scenarios (45 min)

Define at least 8 must-pass scenarios with specific test steps:

For each scenario:
```
Scenario: [name]
Priority: P0/P1/P2
Steps:
  1. [action]
  2. [action]
Expected:
  - [expected result]
Verify in:
  - [Langfuse/Grafana/UI/API]
Frequency: [every build / daily / weekly]
```

---

## Exercise 4: Assemble the Strategy Document (60 min)

Combine all above into a single test strategy document with sections:
1. Overview & Scope
2. Test Layers
3. Risk Matrix
4. Critical Scenarios
5. AI-Specific Testing Approach
6. Tools & Infrastructure
7. Metrics & Reporting
8. Release Testing Process

---

## ✅ Completion Checklist
- [ ] Defined all 7 test layers with scope and tools
- [ ] Created risk matrix with 12+ risks
- [ ] Defined 8+ critical must-pass scenarios
- [ ] Assembled complete test strategy document

> **Next**: [Lab 2 — Onboarding Plan](lab-02-onboarding-plan.md)
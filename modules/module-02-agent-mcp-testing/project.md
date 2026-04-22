# Module 2: Project — Agent & MCP Test Suite

## 🎯 Objective
Create a comprehensive test suite for Agent behavior and MCP tool orchestration.

## ⏱️ Estimated Time: 3 hours

---

## Deliverables

### 1. Agent Tool Selection Test Suite (15+ test cases)

Create a structured test suite covering:

| Category | Min Test Cases | Focus |
|----------|---------------|-------|
| Correct tool selection | 5 | Question → right tool mapping |
| Wrong tool detection | 3 | Questions that might trigger wrong tool |
| No-tool scenarios | 3 | Greetings, out-of-scope, general chat |
| Multi-tool scenarios | 2 | Questions requiring multiple tools |
| Edge cases | 2 | Ambiguous, empty, very long questions |

Each test case must include:
- Test ID, Description, Input question
- Expected tool(s) to be called
- Expected response behavior
- How to verify (Langfuse trace check)

### 2. MCP Failure Handling Test Plan (10+ test cases)

Cover all failure modes:
- MCP Server unavailable
- Timeout scenarios
- Malformed responses
- Partial failures in tool chains
- Recovery after failure

### 3. Prompt Injection Test Suite (8+ test cases)

Test the agent's resistance to:
- Direct instruction override
- Role confusion attacks
- System prompt extraction attempts
- Data exfiltration attempts
- Multi-turn manipulation

### 4. Bug Report (at least 3 bugs found during testing)

Use the standard bug report template from Module 1.

## Evaluation Criteria

| Criteria | Weight |
|---------|--------|
| Test coverage completeness | 30% |
| Realistic and actionable test cases | 25% |
| Langfuse trace analysis quality | 20% |
| Bug reports quality | 15% |
| Clear documentation | 10% |

> **Next Module**: [Module 3 — RAG & Knowledge Base Testing](../module-03-rag-knowledge-base/README.md)
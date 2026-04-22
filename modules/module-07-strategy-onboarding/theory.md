# Module 7: Theory — Test Strategy, Onboarding & Roadmap

## 7.1 QA Test Strategy

### Test Layers

```
Layer 1: UI Testing          ─── React Frontend
Layer 2: API Testing         ─── .NET Backend REST API
Layer 3: Agent Testing       ─── Agent Runtime, MCP, Tool Orchestration
Layer 4: AI Quality Testing  ─── LLM responses, hallucinations, RAG quality
Layer 5: Data Testing        ─── Ingestion, Knowledge Base, Weaviate, OLAP
Layer 6: Security Testing    ─── RBAC/ABAC, prompt injection, data leakage
Layer 7: Infrastructure      ─── vLLM, GPU, Redis, Weaviate cluster
```

### Risk Matrix

| Component | Probability of Failure | Impact if Fails | Risk Level | Test Priority |
|-----------|----------------------|-----------------|------------|---------------|
| LLM (hallucinations) | High | Critical (wrong info to users) | 🔴 Critical | P0 |
| RBAC/ABAC (data leakage) | Medium | Critical (security breach) | 🔴 Critical | P0 |
| RAG retrieval quality | Medium | High (wrong answers) | 🟠 High | P1 |
| Agent tool selection | Medium | High (wrong workflow) | 🟠 High | P1 |
| Ingestion pipeline | Medium | Medium (stale data) | 🟡 Medium | P2 |
| MCP Server availability | Low | High (complete failure) | 🟡 Medium | P2 |
| OCR accuracy (Release 3) | High | Medium (wrong text) | 🟡 Medium | P2 |
| Frontend rendering | Low | Low (cosmetic) | 🟢 Low | P3 |
| vLLM performance | Low | Medium (slow responses) | 🟢 Low | P3 |

### Critical Test Scenarios (Must-Pass)

| # | Scenario | Tests | Frequency |
|---|---------|-------|-----------|
| 1 | End-to-end happy path | User → Question → Correct Answer | Every build |
| 2 | Hallucination check | Golden set (20+ questions) against source docs | Every build |
| 3 | RBAC enforcement | Employee cannot see confidential docs | Every build |
| 4 | Cross-app isolation | HR app doesn't return Finance data | Every build |
| 5 | Agent tool selection | 10 question types → correct tool mapping | Every build |
| 6 | Prompt injection resistance | 5+ injection patterns blocked | Every build |
| 7 | Document ingestion | Upload → index → searchable | Weekly |
| 8 | Performance baseline | p95 latency within SLA | Daily monitoring |

### AI-Specific Testing Checklist

- [ ] **Hallucinations:** Compare every AI response against source documents
- [ ] **Prompt failures:** Test system prompt boundaries and injection resistance
- [ ] **Retrieval errors:** Run golden set, check relevance scores
- [ ] **Context leakage:** Cross-user testing with different access levels
- [ ] **Non-determinism:** Same question multiple times, facts must be consistent
- [ ] **Context window:** Test with questions that retrieve many documents
- [ ] **Tool invocation:** Verify correct MCP tools called via Langfuse traces
- [ ] **Tool chaining:** Multi-tool scenarios complete correctly
- [ ] **Tool failure handling:** Agent handles MCP errors gracefully

### Anti-Patterns (What NOT to Do)

| Anti-Pattern | Why It's Wrong | Correct Approach |
|-------------|----------------|-----------------|
| Trust AI responses without verification | LLMs hallucinate | Always compare to source docs |
| Test only happy paths | AI fails in edge cases | Test boundaries, injections, edge cases |
| Ignore Langfuse traces | Response may look correct but internal flow is wrong | Always check traces |
| Test with single user role | RBAC bugs only appear with cross-role testing | Test with multiple roles |
| Skip regression after prompt changes | New prompts can break existing behavior | Golden set regression |
| Treat AI like traditional software | AI is non-deterministic | Test facts consistency, not exact wording |
| Manual-only testing | Too slow for AI quality monitoring | Build automated golden set checks |

---

## 7.2 14-Day Onboarding Plan

### Day 1-2: Orientation & Architecture

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 1 AM** | Read Module 1 theory (architecture) | 3h | |
| **Day 1 PM** | Complete Lab 1 (platform exploration) | 3h | Data flow map |
| **Day 2 AM** | Complete Lab 2 (first AI queries) | 3h | Test observations |
| **Day 2 PM** | Module 1 Quiz + Project | 2h | Architecture cheat sheet |

**First bugs likely found:**
- Streaming response issues in UI
- Backend returning generic 500 errors
- Langfuse missing some trace spans

**Where to look:** Browser DevTools (Network tab), Langfuse traces

---

### Day 3-4: Agent & MCP Testing

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 3 AM** | Read Module 2 theory (Agent, MCP) | 3h | |
| **Day 3 PM** | Complete Lab 1 (agent behavior) | 3h | Tool selection test results |
| **Day 4 AM** | Complete Lab 2 (MCP testing) | 3h | MCP failure test results |
| **Day 4 PM** | Module 2 Quiz | 1h | |

**First bugs likely found:**
- Agent calling wrong tool for some question types
- Timeout issues when MCP Server is slow
- Incomplete Langfuse traces

**Where to look:** Langfuse traces (tool call spans), MCP server logs

---

### Day 5-6: RAG & Knowledge Base

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 5 AM** | Read Module 3 theory (RAG) | 3h | |
| **Day 5 PM** | Complete Lab 1 (retrieval quality) | 3h | Golden set (first version) |
| **Day 6 AM** | Complete Lab 2 (ingestion pipeline) | 3h | Ingestion test results |
| **Day 6 PM** | Module 3 Quiz | 1h | |

**First bugs likely found:**
- Wrong documents retrieved for some questions
- Chunking issues (tables split across chunks)
- Ingestion failures for large/complex documents

**Where to look:** KB API search results, Weaviate console, ingestion worker logs

---

### Day 7-8: LLM Quality & Security

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 7 AM** | Read Module 4 theory (LLM testing) | 2h | |
| **Day 7 PM** | Complete Labs (hallucination + quality) | 4h | Hallucination report, GRACE scores |
| **Day 8 AM** | Read Module 5 theory (security) | 2h | |
| **Day 8 PM** | Complete Labs (RBAC/ABAC + leakage) | 4h | Security test results |

**First bugs likely found:**
- Hallucinated facts in AI responses
- RBAC filter not applying to some document types
- Prompt injection partially successful

**Where to look:** Source documents (for comparison), Langfuse traces (retrieval + LLM input/output)

---

### Day 9-10: Observability & Debugging

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 9 AM** | Read Module 6 theory (observability) | 2h | |
| **Day 9 PM** | Complete Labs (Langfuse + Grafana) | 4h | Debugging playbook draft |
| **Day 10** | Practice debugging real issues found so far | 6h | Bug reports with trace evidence |

**Skills practiced:** End-to-end debugging from symptom to root cause using observability tools.

---

### Day 11-14: Strategy, Integration & Independent Work

| Day | Activity | Duration | Deliverable |
|-----|---------|----------|-------------|
| **Day 11** | Read Module 7 theory, start test strategy | 6h | Test strategy draft |
| **Day 12** | Complete test strategy, run full regression | 6h | Complete golden set run |
| **Day 13** | Shadow a senior QA during real testing | 6h | Observations and questions |
| **Day 14** | Final project: complete QA academy deliverable | 6h | All deliverables submitted |

**By Day 14, the QA engineer should:**
- ✅ Understand the full system architecture
- ✅ Be able to test any component independently
- ✅ Know how to debug issues using Langfuse/Grafana
- ✅ Have a working golden set for regression testing
- ✅ Be ready to work independently on test tasks

---

## 7.3 Learning Roadmap

### Phase 1: Foundation (Weeks 1-2)
**Goal:** Understand the system and execute basic tests

| Skill | Entry | Exit |
|-------|-------|------|
| Architecture knowledge | 0% | Can draw the full data flow diagram |
| AI concepts | 0% | Can explain LLM, RAG, embeddings, agents, MCP |
| Basic testing | Standard QA skills | Can test end-to-end chat flow |
| Langfuse | Never used | Can find and read a trace |

**Common QA mistakes at this stage:**
- Treating AI like deterministic software (expecting exact same output)
- Not checking Langfuse traces (only looking at the response)
- Ignoring the architecture and testing only the UI

---

### Phase 2: Core AI Testing (Weeks 3-4)
**Goal:** Master RAG testing and hallucination detection

| Skill | Entry | Exit |
|-------|-------|------|
| RAG testing | Know what RAG is | Can run golden set, test ingestion, evaluate retrieval |
| Hallucination detection | Know the concept | Can systematically detect and categorize hallucinations |
| Quality evaluation | No framework | Can apply GRACE to any response |
| KB testing | Never tested | Can test ingestion pipeline end-to-end |

**Common QA mistakes at this stage:**
- Not comparing AI responses to source documents (trusting the AI)
- Only testing with simple questions (missing edge cases)
- Not testing chunking quality

---

### Phase 3: Advanced (Weeks 5-6)
**Goal:** Security, observability, and production readiness

| Skill | Entry | Exit |
|-------|-------|------|
| Security testing | Basic RBAC knowledge | Full RBAC/ABAC testing + AI-specific security |
| Debugging | Can find traces | Can debug any issue using the 5-step workflow |
| Monitoring | Know Grafana exists | Can identify problems from dashboards |
| Production QA | None | Can execute pre/post-release checklists |

**Common QA mistakes at this stage:**
- Only testing security at the UI level (missing KB-level ABAC)
- Not checking Langfuse traces even when response looks correct
- Ignoring performance metrics during testing

---

### Phase 4: Production QA (Weeks 7-8)
**Goal:** Work independently, create strategy, onboard others

| Skill | Entry | Exit |
|-------|-------|------|
| Test strategy | Knows components | Can write a complete test strategy document |
| Risk assessment | Knows risks | Can create and maintain a risk matrix |
| Onboarding | Was onboarded | Can onboard new QA engineers |
| Independent work | Guided work | Fully autonomous QA work on the platform |

---

## 📖 Module Summary

1. The **test strategy** covers 7 layers from UI to infrastructure
2. **Risk matrix** drives testing priorities — hallucinations and RBAC are P0
3. The **14-day onboarding** gets a Senior QA productive in 2 weeks
4. The **learning roadmap** has 4 phases: Foundation → Core → Advanced → Production
5. **Anti-patterns** are as important to learn as best practices

> **Next**: [Lab 1 — Building the Test Strategy](labs/lab-01-test-strategy.md)
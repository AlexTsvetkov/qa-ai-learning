# Module 6: Quiz — Observability & Production QA

> Answers at the bottom.

### Q1: First Debugging Step
A user reports a wrong answer. What's the first thing you should do?
- A) Restart the LLM service
- B) Find the Langfuse trace for that request
- C) Check Grafana dashboards
- D) Ask the user to try again

### Q2: Langfuse Trace
In a Langfuse trace, the `mcp_retrieval` span shows 0 results returned. This means:
- A) The LLM will hallucinate an answer
- B) The Knowledge Base found no relevant documents, so the LLM should say "I don't have info"
- C) The trace is corrupt
- D) The user asked a wrong question

### Q3: Performance Bottleneck
Grafana shows LLM latency at p95 = 25s (normal is <10s). GPU memory is at 95%. The most likely cause is:
- A) Frontend CSS issues
- B) vLLM is running out of GPU memory under load
- C) Weaviate is slow
- D) The user's internet is slow

### Q4: Pre-Release Check
Before deploying a new prompt version, you should:
- A) Just deploy and see what happens
- B) Run the golden set test suite and compare results with previous version
- C) Check the Grafana font settings
- D) Restart all services

### Q5: Trace vs Metrics
Langfuse traces are best for _____, while Prometheus metrics are best for _____:
- A) System-level monitoring; individual request debugging
- B) Individual request debugging; system-level monitoring and trends
- C) Frontend testing; backend testing
- D) Security testing; performance testing

---

## ✅ Answers
**Q1:** B — Always start with the Langfuse trace
**Q2:** B — No documents means the LLM should acknowledge the gap, not hallucinate
**Q3:** B — High GPU memory + high latency = vLLM overloaded
**Q4:** B — Run golden set regression before deploying
**Q5:** B — Traces for individual debugging, metrics for system trends
# Module 1: Quiz — Platform Architecture & AI Fundamentals

> Test your understanding. Answers are at the bottom.

---

### Q1: Data Flow Order
Place these components in the correct order of a user request:
`Knowledge Base`, `React Frontend`, `Agent Runtime`, `LLM (vLLM)`, `.NET Backend`, `MCP Server`

1. _______________
2. _______________
3. _______________
4. _______________
5. _______________
6. _______________

---

### Q2: Component Identification
A user asks "What is our vacation policy?" and gets a wrong answer that doesn't match the actual policy document. Which component is MOST LIKELY the source of the bug?

- A) React Frontend
- B) .NET Backend
- C) Knowledge Base (RAG retrieval)
- D) Active Directory

---

### Q3: RAG Purpose
What is the primary purpose of RAG (Retrieval-Augmented Generation)?

- A) To make the LLM respond faster
- B) To reduce hallucinations by grounding answers in actual documents
- C) To encrypt user data before sending to the LLM
- D) To train the LLM on company-specific data

---

### Q4: MCP Analogy
Which analogy best describes the Model Context Protocol (MCP)?

- A) A firewall that blocks unauthorized requests
- B) A USB standard that lets any compatible tool plug into any compatible agent
- C) A database query language like SQL
- D) A machine learning training framework

---

### Q5: Hallucination Detection
Which of these is a hallucination?

- A) The system returns "I don't have information about that topic"
- B) The system returns a 500 error
- C) The system confidently states a vacation policy that doesn't exist in any company document
- D) The system takes 30 seconds to respond

---

### Q6: Security Concern
User A (HR department) asks the HR Assistant a question. The system retrieves documents but the RBAC filter has a bug. What is the worst-case scenario?

- A) The response is slow
- B) User A sees confidential documents from Finance or Executive teams
- C) The response contains formatting errors
- D) The Langfuse trace is incomplete

---

### Q7: Debugging Path
A user reports that the AI HR Assistant gives an empty response. What is the correct debugging sequence?

- A) Check Grafana → Restart the server → Test again
- B) Check Langfuse trace → Check application logs → Check component status
- C) Ask the user to try again → Close the ticket
- D) Check the React console → Rebuild the frontend

---

### Q8: Ingestion Pipeline
A new HR policy document was uploaded 2 hours ago, but the AI Assistant still gives answers based on the old policy. Where should you investigate?

- A) The LLM model needs retraining
- B) The ingestion pipeline: check if the job completed, document was chunked, embedded, and stored in Weaviate
- C) The React frontend cache
- D) The Active Directory configuration

---

### Q9: Platform Constraint
Why does the platform use Llama 3 (via vLLM) instead of OpenAI GPT-4?

- A) Llama 3 is a better model than GPT-4
- B) The platform is on-premises and no data can leave the company network
- C) GPT-4 doesn't support chat functionality
- D) Llama 3 is faster than GPT-4

---

### Q10: Observability Tools
Match each tool with its primary purpose:

| Tool | Purpose |
|------|---------|
| Langfuse | A) System metrics (latency, error rates) |
| Prometheus | B) Visual dashboards |
| Grafana | C) AI traces, prompt versioning, evaluations |
| Loki | D) Application log aggregation |

---

## ✅ Answers

**Q1:** 1) React Frontend → 2) .NET Backend → 3) Agent Runtime → 4) MCP Server → 5) Knowledge Base → 6) LLM (vLLM)
> Note: Steps 5 and 6 may happen in parallel or different order depending on the agent's plan.

**Q2:** C) Knowledge Base (RAG retrieval) — Wrong answer that doesn't match documents usually means wrong documents were retrieved, or the right documents weren't found.

**Q3:** B) To reduce hallucinations by grounding answers in actual documents

**Q4:** B) A USB standard — MCP standardizes communication between agents and tools.

**Q5:** C) The system confidently states a vacation policy that doesn't exist — this is a hallucination (plausible but fabricated information).

**Q6:** B) User A sees confidential documents from Finance or Executive teams — RBAC bugs can cause data leakage, which is a critical security issue.

**Q7:** B) Check Langfuse trace → Check application logs → Check component status — systematic debugging using observability tools.

**Q8:** B) The ingestion pipeline — check if the job completed, document was processed, and stored in Weaviate. The LLM doesn't need retraining; it reads from the Knowledge Base.

**Q9:** B) On-premises deployment means no external API calls. All data stays within the company network.

**Q10:** Langfuse = C, Prometheus = A, Grafana = B, Loki = D
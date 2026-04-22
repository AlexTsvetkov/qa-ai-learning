# Module 1: Theory (Part 2) — AI Fundamentals & Platform Releases

> Continuation of [Theory Part 1](theory.md)

## 1.4 AI Fundamentals for QA (No ML Theory Required)

### What is an LLM?

**Large Language Model** — a neural network trained on enormous amounts of text. It predicts the next word based on probability.

**Analogy:** Imagine an extremely well-read person who has memorized millions of documents. When you ask them a question, they construct an answer based on patterns from everything they've read. They sound confident, but they might mix up details or invent things that sound plausible.

**For QA, the key facts are:**
- LLMs are **probabilistic** — the same question can produce different answers each time
- LLMs can **hallucinate** — generate confident-sounding but factually wrong content
- LLMs have a **context window** — a maximum amount of text they can process at once
- LLMs don't have **real understanding** — they pattern-match, not reason
- Our platform uses **Llama 3** (open-source) instead of GPT-4 or Claude

### What is RAG?

**Retrieval-Augmented Generation** — a technique to reduce hallucinations.

**Without RAG (pure LLM):**
```
Question: "What is our parental leave policy?"
LLM: "Typically, companies offer 12 weeks..." ← HALLUCINATION (made up answer)
```

**With RAG:**
```
Question: "What is our parental leave policy?"
  → Step 1: Search knowledge base for "parental leave policy"
  → Step 2: Retrieve actual company document chunk
  → Step 3: Send to LLM: "Based on THIS document, answer the question"
LLM: "According to policy HR-042, employees are entitled to 16 weeks..." ← GROUNDED answer
```

**For QA:** RAG is the critical system that prevents hallucinations. If RAG fails (wrong documents retrieved, no documents found, access filter too strict), the LLM will either hallucinate or say "I don't know."

### What are Embeddings?

**Embeddings** are numerical representations (vectors) of text. They capture the *meaning* of text as numbers.

**Analogy:** Imagine converting every sentence into GPS coordinates on a "meaning map." Sentences with similar meanings are close together on the map.

```
"parental leave policy"     → [0.82, 0.15, 0.93, ...]
"maternity and paternity"   → [0.80, 0.17, 0.91, ...]  ← similar vector (close meaning)
"annual budget report"      → [0.12, 0.88, 0.05, ...]  ← very different vector
```

**For QA:** Embedding quality directly affects retrieval quality. If the embedding model is bad, the system retrieves wrong documents.

### What is a Vector Database?

**Vector database** (Weaviate in our platform) stores embeddings and enables similarity search.

**Traditional database:** `SELECT * FROM docs WHERE title = 'parental leave'` (exact match)
**Vector database:** "Find documents whose meaning is closest to this question" (semantic search)

**For QA:** Test that similar questions retrieve the same documents, and that unrelated questions don't.

### What is an AI Agent?

An **agent** is an AI system that can:
1. Receive a task
2. Plan a sequence of actions
3. Use tools to execute actions
4. Evaluate results
5. Iterate until done

**Analogy:** A human personal assistant who receives a request, decides what to do, makes phone calls (tools), reads documents (retrieval), and assembles a final answer.

**Our Agent Runtime:**
- Receives user question
- Decides which MCP tools to call (retrieval? query generator? external tool?)
- Orchestrates the calls
- Assembles the final response from all gathered information
- Logs everything to Langfuse

### What is MCP?

**Model Context Protocol** — a standardized way for agents to communicate with tools.

**Analogy:** Think of MCP like USB. Before USB, every device had its own cable. MCP standardizes how agents talk to tools, so any MCP-compatible tool works with any MCP-compatible agent.

**Structure:**
- **MCP Host** = Agent Runtime (the computer with USB ports)
- **MCP Client** = the connection interface (the USB port)
- **MCP Server** = the tool (the USB device — keyboard, mouse, printer)

---

## 1.5 Platform Releases

### Release 1: HR Assistant + Basic RAG
**Scope:**
- AI HR Assistant application
- Basic RAG pipeline (document ingestion → retrieval → response)
- HR document knowledge base
- Active Directory authentication
- Basic RBAC

**QA Focus:**
- End-to-end chat flow works
- HR documents retrieved correctly
- Basic access control works
- Responses are grounded in actual documents

### Release 2: Finance + OLAP + Monitoring
**Scope:**
- AI Finance Assistant application
- OLAP query generation (SQL/MDX queries on financial data)
- Matter DB integration
- Langfuse monitoring integration
- Prometheus/Grafana dashboards
- Enhanced RBAC/ABAC

**QA Focus:**
- Financial query accuracy (numbers must be exact!)
- OLAP integration correctness
- Cross-application isolation (HR user can't access Finance data)
- Monitoring and alerting works
- Performance under load

### Release 3: Clerk + OCR + Pipelines
**Scope:**
- AI Clerk application
- PaddleOCR integration for scanned documents
- Advanced ingestion pipelines
- Batch document processing
- External tool integrations

**QA Focus:**
- OCR accuracy on various document types
- Batch processing reliability
- Complex multi-tool agent workflows
- External tool failure handling
- Full regression across all 3 applications

---

## 1.6 Where Systems Break — The QA Failure Map

```
Layer               │ What Breaks                    │ How You Detect It
────────────────────┼────────────────────────────────┼──────────────────────────
Frontend            │ Rendering, streaming, UX        │ Visual inspection, browser console
Backend API         │ Auth, validation, timeouts      │ API testing (Postman), HTTP status codes
Agent Runtime       │ Wrong tool, loops, timeouts     │ Langfuse traces, logs
MCP Protocol        │ Tool discovery, invocation      │ Langfuse traces, MCP server logs
LLM (vLLM)          │ Hallucinations, quality, OOM    │ Response comparison, Langfuse, GPU metrics
Knowledge Base      │ Retrieval quality, access       │ Direct KB API testing, comparison with source
Ingestion Pipeline  │ Failed jobs, chunking, OCR      │ Ingestion API, Redis queue, worker logs
Vector DB (Weaviate)│ Index corruption, search        │ Weaviate API, count verification
Data Sources        │ Connectivity, freshness         │ Direct DB queries, data comparison
Security            │ RBAC/ABAC bypass, leakage       │ Cross-user testing, response analysis
Observability       │ Missing traces, wrong metrics    │ Langfuse UI, Grafana dashboards
```

### The Debugging Decision Tree

When something goes wrong, follow this path:

```
User reports wrong answer
    │
    ├── Is the response empty/error? 
    │   └── Check: Backend logs → Agent logs → MCP Server status
    │
    ├── Is the response a hallucination (plausible but wrong)?
    │   └── Check: Were correct documents retrieved? (Langfuse trace)
    │       ├── No documents retrieved → Knowledge Base issue (embedding? index?)
    │       ├── Wrong documents retrieved → Retrieval quality issue
    │       └── Correct documents but wrong answer → LLM quality issue
    │
    ├── Does User A see User B's data?
    │   └── Check: RBAC/ABAC filter in Knowledge Base query
    │
    └── Is the response slow?
        └── Check: Grafana → which component has high latency?
            ├── LLM Gateway → vLLM overloaded (GPU)
            ├── Knowledge Base → Weaviate slow (index size?)
            └── Agent → MCP tool timeout
```

---

## 1.7 Key Terminology for QA

| Term | Definition | Why QA Cares |
|------|-----------|-------------|
| **LLM** | Large Language Model (Llama 3 in our platform) | The AI that generates text responses |
| **RAG** | Retrieval-Augmented Generation | Prevents hallucinations by grounding answers in documents |
| **Embedding** | Numerical vector representing text meaning | Powers semantic search in Knowledge Base |
| **Vector DB** | Database storing embeddings (Weaviate) | Where all document chunks are stored |
| **Agent** | AI system that plans and executes tasks using tools | Core orchestration logic |
| **MCP** | Model Context Protocol — standard for agent-tool communication | How the agent calls tools |
| **Hallucination** | AI generates plausible but factually incorrect content | Major bug category in AI testing |
| **Context Window** | Maximum text an LLM can process at once | Limits how much context can be provided |
| **Token** | Unit of text (~3/4 of a word) | Affects cost, latency, context limits |
| **Chunking** | Splitting documents into smaller pieces for indexing | Affects retrieval quality |
| **Ingestion** | Process of adding documents to the Knowledge Base | Pipeline: extract → chunk → embed → store |
| **Prompt** | Instructions given to the LLM | System prompts managed in Langfuse |
| **Trace** | Complete log of a request's journey through the system | Primary debugging tool (Langfuse) |
| **RBAC** | Role-Based Access Control | User role determines data access |
| **ABAC** | Attribute-Based Access Control | Document attributes determine access |
| **vLLM** | High-performance LLM serving framework | Runs Llama 3 on GPU |
| **Langfuse** | Observability platform for LLM applications | Traces, prompts, evaluations |
| **On-prem** | Deployed on company hardware (not cloud) | No external API calls, all data stays internal |

---

## 📖 Module Summary

1. The platform is an **on-prem enterprise AI ecosystem** with 3 applications (HR, Finance, Clerk)
2. Data flow: **User → FE → BE → Agent → MCP → KB/LLM → Response**
3. The **Agent Runtime** is the brain — it orchestrates tool calls via MCP
4. **RAG** (Knowledge Base + Weaviate) prevents hallucinations by grounding answers in documents
5. **Security** (RBAC/ABAC) controls who sees what — a critical testing area
6. **Langfuse** is your primary debugging tool — every request creates a trace
7. Every component has **specific failure modes** — your job is to know where to look

> **Next**: [Lab 1 — Platform Exploration & Data Flow Mapping](labs/lab-01-platform-exploration.md)
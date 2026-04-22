# Module 1: Theory — Platform Architecture & AI Fundamentals

## 1.1 The Platform at a Glance

You are testing an **enterprise AI platform** deployed on-premises (no cloud, no external APIs). It consists of three applications that help employees get answers from internal company data:

| Application | Release | Purpose | Data Sources |
|------------|---------|---------|-------------|
| **AI HR Assistant** | Release 1 | Answer HR policy questions, employee handbook queries | HR documents, policy files |
| **AI Finance Assistant** | Release 2 | Financial reporting, budget queries, OLAP analytics | Financial data, MS SQL, OLAP cubes |
| **AI Clerk** | Release 3 | Document processing, form filling, OCR workflows | Scanned documents, file shares |

**Key constraint:** Everything runs on-premises. No data leaves the company network. No OpenAI, no cloud LLMs. The platform uses **open-source models** (Llama 3) running on company hardware.

---

## 1.2 End-to-End Data Flow

This is the most important diagram you need to internalize. Every test you write traces back to this flow:

```
┌──────────┐    ┌──────────┐    ┌──────────────────┐    ┌─────────────────┐
│  User     │───▶│  React   │───▶│  .NET Backend    │───▶│  Agent Runtime  │
│  (Browser)│    │  Frontend │    │  (C# API)        │    │  (MCP Host)     │
└──────────┘    └──────────┘    └──────────────────┘    └────────┬────────┘
                                                                  │
                                                    ┌─────────────┼─────────────┐
                                                    │             │             │
                                                    ▼             ▼             ▼
                                              ┌──────────┐ ┌──────────┐ ┌──────────┐
                                              │MCP Client│ │MCP Client│ │MCP Client│
                                              │    #1    │ │    #2    │ │    #3    │
                                              └────┬─────┘ └────┬─────┘ └────┬─────┘
                                                   │             │             │
                                                   ▼             ▼             ▼
                                              ┌──────────┐ ┌──────────┐ ┌──────────┐
                                              │MCP Server│ │MCP Server│ │MCP Server│
                                              │Retrieval │ │Query Gen │ │External  │
                                              │ Tools    │ │          │ │ Tools    │
                                              └────┬─────┘ └────┬─────┘ └──────────┘
                                                   │             │
                                         ┌─────────┴─────────────┴──────────┐
                                         │                                   │
                                         ▼                                   ▼
                                   ┌───────────┐                      ┌───────────┐
                                   │Knowledge  │                      │LLM Gateway│
                                   │Base (RAG) │                      │  (Proxy)  │
                                   │           │                      │           │
                                   │ Weaviate  │                      │   vLLM    │
                                   │ (Vector DB)│                     │ (Llama 3) │
                                   └───────────┘                      └───────────┘
```

### The Flow Step by Step

1. **User** types a question in the browser: *"What is our parental leave policy?"*
2. **React Frontend** sends the request to the .NET Backend API
3. **.NET Backend** authenticates the user (Active Directory), checks permissions, forwards to Agent
4. **Agent Runtime** (MCP Host) receives the task and decides what to do:
   - It uses a **Prompt Resolver** to determine the system prompt
   - It starts **trace collection** (logged to Langfuse)
   - It orchestrates calls to **MCP tools** dynamically
5. **MCP Client** calls the appropriate **MCP Server**:
   - **Retrieval Server**: searches the Knowledge Base for relevant documents
   - **Query Generator**: constructs database queries (for Finance/OLAP)
   - **External Tools**: calls client-provided APIs
6. **Knowledge Base** performs retrieval:
   - Converts the question to a vector (embedding) via HuggingFace TEI
   - Searches Weaviate vector database for similar document chunks
   - Applies **RBAC/ABAC policy filters** (the user can only see documents they have access to)
   - Returns relevant document chunks
7. **LLM Gateway** sends the question + retrieved context to **vLLM** (Llama 3)
8. **Llama 3** generates a response based on the provided context
9. Response travels back: LLM → Agent → Backend → Frontend → User

> 🎯 **QA Mental Model:** Every bug you find lives somewhere in this chain. Your job is to figure out *where* in the chain the failure occurred.

---

## 1.3 Component Deep Dive (QA Perspective)

### 🖥️ Frontend (React)

**What it does:** User interface for all three applications (HR, Finance, Clerk).

**What QA tests:**
- Chat interface rendering and responsiveness
- Message streaming (responses appear token by token)
- File upload (for Clerk application — document OCR)
- Session management and conversation history
- Error state handling when backend is slow/unavailable

**Common bugs:**
- 🐛 Streaming responses break mid-sentence when connection drops
- 🐛 Long responses overflow the chat container
- 🐛 File upload accepts unsupported formats without error
- 🐛 Conversation history doesn't persist across page refreshes

---

### ⚙️ Backend (.NET / C#)

**What it does:** REST API layer. Handles authentication, request validation, and routes requests to the Agent Runtime.

**What QA tests:**
- API endpoint validation (request/response contracts)
- Authentication via Active Directory integration
- Request rate limiting and timeout handling
- Error responses for malformed requests
- Session management

**Common bugs:**
- 🐛 Backend returns 500 instead of descriptive error when Agent is down
- 🐛 Authentication token expiry not handled gracefully
- 🐛 Large payloads (long questions + context) cause timeouts
- 🐛 Concurrent requests from same user cause session conflicts

---

### 🤖 Agent Runtime (MCP Host)

**What it does:** The "brain" of the system. Built with .NET MCP SDK. It receives a user question, decides which tools to call, orchestrates the workflow, and assembles the final response.

**Key components inside the Agent:**
- **Prompt Resolver** — selects the right system prompt based on application context (HR vs Finance vs Clerk)
- **Trace Collection** — logs every step to Langfuse for observability
- **MCP Clients** — interfaces to call MCP Servers (tools)

**What QA tests:**
- Does the agent pick the correct tools for the question?
- Does it handle tool failures gracefully?
- Does it chain multiple tool calls correctly?
- Does it respect the system prompt boundaries?
- Does the trace capture all steps?

**Common bugs:**
- 🐛 Agent calls the wrong tool (e.g., uses Finance query generator for an HR question)
- 🐛 Agent enters infinite loop when a tool returns unexpected data
- 🐛 Agent ignores system prompt and answers out-of-scope questions
- 🐛 Trace is incomplete — missing steps when parallel tool calls occur
- 🐛 Agent doesn't timeout when MCP Server hangs

---

### 🔗 MCP (Model Context Protocol)

**What it is:** MCP is a standardized protocol for connecting AI agents to external tools and data sources. Think of it like a USB standard — any MCP-compatible tool can plug into any MCP-compatible agent.

**Architecture:**
```
Agent Runtime (MCP Host)
    │
    ├── MCP Client #1 ──── MCP Server: Retrieval Tools
    │                       (searches Knowledge Base)
    │
    ├── MCP Client #2 ──── MCP Server: Query Generator
    │                       (generates SQL/OLAP queries)
    │
    └── MCP Client #3 ──── MCP Server: External Tools
                            (client-provided integrations)
```

**What QA tests:**
- Tool discovery — does the agent see all available tools?
- Tool invocation — correct parameters passed?
- Tool chaining — multiple tools called in correct sequence?
- Error handling — what happens when a tool fails?
- Timeout handling — what if a tool takes too long?

**Common bugs:**
- 🐛 MCP Server returns malformed JSON → Agent crashes
- 🐛 Tool discovery fails silently → Agent can't find tools
- 🐛 MCP Client sends wrong parameter types → Server rejects request
- 🐛 Tool chaining breaks when intermediate tool returns empty result
- 🐛 No retry logic when MCP Server is temporarily unavailable

---

### 🧠 LLM & Model Orchestration

**Components:**

| Component | Purpose | Endpoint |
|-----------|---------|----------|
| **LLM Gateway** | Proxy that routes requests to the right model | Internal |
| **vLLM** | Serves Llama 3 model — generates text responses | `/chat` |
| **HuggingFace TEI** | Generates embeddings (vector representations of text) | `/embed` |
| **PaddleOCR** | Extracts text from scanned documents/images | `/ocr` |

**What QA tests:**
- Response quality and relevance
- Hallucination detection (model invents facts)
- Latency and throughput under load
- Model behavior with edge-case inputs (empty, very long, special characters)
- Embedding consistency (same text → same vector)
- OCR accuracy on different document types

**Common bugs:**
- 🐛 LLM Gateway routes to wrong model instance
- 🐛 vLLM runs out of GPU memory under concurrent requests
- 🐛 LLM hallucinates a policy that doesn't exist
- 🐛 Embeddings service returns different vectors for identical text (non-deterministic)
- 🐛 OCR fails on rotated/skewed documents
- 🐛 Response cut off mid-sentence due to token limit

---

### 📚 Knowledge Base (RAG)

RAG = **Retrieval-Augmented Generation**. Instead of the LLM answering from memory (which causes hallucinations), we first *retrieve* relevant documents, then give them to the LLM as context.

**Two planes:**

#### Query Plane (answering questions):
```
User Question
    │
    ▼
Knowledge Base REST API
    │
    ▼
Retrieval Adapter ──── Policy/Access Filter (RBAC/ABAC)
    │
    ▼
MCP Retrieval Server ──── Weaviate (Vector DB)
    │
    ▼
Retrieved Document Chunks (with access control applied)
```

#### Ingestion Plane (adding documents):
```
New Document (PDF, DOCX, etc.)
    │
    ▼
Ingestion API
    │
    ▼
Job Orchestration
    │
    ▼
Work Queue (Redis / RQ)
    │
    ▼
Ingestion Workers (Python)
    ├── Text extraction
    ├── OCR (PaddleOCR) for scanned docs
    ├── Chunking (splitting into pieces)
    ├── Embedding (HuggingFace TEI)
    └── Store in Weaviate
```

**What QA tests:**
- **Retrieval quality:** Does the system find the right documents?
- **Retrieval ranking:** Are the most relevant results ranked first?
- **Access control:** Can User A see documents they shouldn't?
- **Ingestion completeness:** Are all document pages/sections indexed?
- **Chunking quality:** Are document chunks meaningful (not cut mid-sentence)?
- **Embedding accuracy:** Similar questions retrieve similar documents?
- **OCR quality:** Text extracted correctly from scanned PDFs?

**Common bugs:**
- 🐛 Retrieval returns irrelevant documents (poor embedding quality)
- 🐛 Access filter doesn't apply — User A sees User B's confidential documents
- 🐛 Ingestion job fails silently — document never gets indexed
- 🐛 Large PDFs (500+ pages) cause ingestion worker to crash
- 🐛 Chunking splits a table across two chunks, making it nonsensical
- 🐛 Re-ingesting an updated document creates duplicates instead of replacing
- 🐛 OCR misreads numbers in scanned financial documents

---

### 📊 Data Sources

| Source | Used By | Type |
|--------|---------|------|
| **File Share** | HR, Clerk | Documents (PDF, DOCX, TXT) |
| **Matter DB** | HR, Finance | MS SQL Server database |
| **Financial Data** | Finance | OLAP cubes |
| **Internal Systems** | All | Custom APIs |

**What QA tests:**
- Data freshness — how quickly do updates propagate?
- Data accuracy — does the AI response match the source data?
- Connection resilience — what happens when a data source is unavailable?

---

### 🔐 Security

| Component | Purpose |
|-----------|---------|
| **Active Directory** | User authentication — who are you? |
| **RBAC** (Role-Based Access Control) | What role do you have? (HR Manager, Finance Analyst, etc.) |
| **ABAC** (Attribute-Based Access Control) | Document-level access based on attributes (department, classification level, etc.) |

**What QA tests:**
- Can an HR user access Finance data?
- Can a junior employee see executive-only documents?
- Does the LLM response leak information from filtered-out documents?
- What happens if Active Directory is unavailable?
- Session hijacking / token reuse scenarios

**Common bugs:**
- 🐛 RBAC filter applies to retrieval but not to the LLM prompt (leaked context)
- 🐛 User role change doesn't take effect until cache expires
- 🐛 Cross-application data leakage (HR assistant returns Finance data)
- 🐛 AD group membership changes not reflected in real-time

---

### 📈 Observability

| Tool | Purpose | What QA Uses It For |
|------|---------|-------------------|
| **Langfuse** | Prompt registry, versioning, traces, evaluations | Debugging AI behavior, checking prompt versions, reviewing traces |
| **Prometheus** | Metrics collection | Monitoring latency, error rates, resource usage |
| **Grafana** | Dashboards | Visual monitoring during test runs |
| **Logs** (Loki or similar) | Application logs | Debugging failures, tracing error paths |

**What QA tests:**
- Are all traces captured in Langfuse?
- Are metrics accurate in Prometheus?
- Do Grafana dashboards reflect actual system state?
- Are logs structured and searchable?

**This is your primary debugging toolkit.** When a test fails, your investigation path is:
1. Check Langfuse trace for the request
2. Check application logs for errors
3. Check Grafana dashboards for system-level issues
4. Check Prometheus metrics for anomalies

---

## 1.4 AI Fundamentals for QA (No ML Theory Required)

### What is an LLM?

**Large Language Model** — a neural network trained on enormous amounts of text. It predicts the next word based on probability.

**Analogy:** Imagine an extremely well-read person who has memorized millions of documents. When you ask them a question, they construct an answer based on patterns from everything they've read. They sound confident, but they might mix up details or invent things that sound plausible.

**For QA, the key facts are:**
- LLMs are **probabil
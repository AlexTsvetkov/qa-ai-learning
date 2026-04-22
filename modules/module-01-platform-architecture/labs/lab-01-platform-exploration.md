# Lab 1: Platform Exploration & Data Flow Mapping

## 🎯 Objective

Explore the enterprise AI platform hands-on and create a data flow map that traces a user request through every system component.

## ⏱️ Estimated Time: 2 hours

## 📋 Prerequisites

- Access to the platform test environment
- Postman or similar API testing tool
- Access to Langfuse dashboard
- Access to Grafana dashboard
- Notepad/drawing tool for mapping

---

## Exercise 1: Trace a Request Through the UI (30 min)

### Step 1: Open the HR Assistant

1. Open the AI HR Assistant in your browser
2. Open browser Developer Tools (F12) → Network tab
3. Type: **"What is our vacation policy?"**
4. Press Enter and observe:

**Record the following:**

| Observation | Your Notes |
|------------|-----------|
| URL the request was sent to | |
| HTTP method (GET/POST) | |
| Request payload structure (JSON fields) | |
| Response status code | |
| How long until first response token appeared? | |
| Did the response stream (appear word by word) or arrive all at once? | |
| What did the final response say? | |

### Step 2: Identify the Backend API

Look at the Network tab requests:
- What is the base URL of the backend API?
- What endpoint handles chat requests?
- What authentication headers are sent?
- Are there any WebSocket connections (for streaming)?

### Step 3: Try Different Questions

Ask these questions and observe differences in response time and behavior:

| Question | Response Time | Did it seem to search documents? | Notes |
|----------|-------------|--------------------------------|-------|
| "What is our vacation policy?" | | | |
| "Hello, how are you?" | | | |
| "What was our Q3 revenue?" (wrong app!) | | | |
| "" (empty message) | | | |
| Very long message (paste a full paragraph) | | | |

> 💡 **What to notice:** Different questions trigger different agent behaviors. Some search the Knowledge Base, some are answered directly, some are refused.

---

## Exercise 2: Explore the API Layer (30 min)

### Step 1: Identify API Endpoints

Using Postman (or curl), explore the backend API:

```bash
# List available endpoints (if swagger/docs available)
curl -X GET https://<platform-url>/api/docs

# Send a chat request
curl -X POST https://<platform-url>/api/chat \
  -H "Authorization: Bearer <your-token>" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "What is our parental leave policy?",
    "conversation_id": "test-001"
  }'
```

### Step 2: Test API Boundaries

Try these requests and record the behavior:

| Test | Expected | Actual | Bug? |
|------|----------|--------|------|
| Valid request with auth | 200 OK + response | | |
| Request without auth token | 401 Unauthorized | | |
| Request with expired token | 401 Unauthorized | | |
| Empty message body | 400 Bad Request | | |
| Very large message (10,000+ chars) | ? | | |
| Invalid JSON body | 400 Bad Request | | |
| Request to non-existent endpoint | 404 Not Found | | |

### Step 3: Check the Knowledge Base API

If accessible, test the Knowledge Base REST API directly:

```bash
# Search the knowledge base
curl -X POST https://<platform-url>/api/kb/search \
  -H "Authorization: Bearer <your-token>" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "vacation policy",
    "top_k": 5
  }'
```

Record:
- How many document chunks are returned?
- What metadata is included with each chunk (document name, page number, access level)?
- Does the response include relevance scores?

---

## Exercise 3: Read a Langfuse Trace (30 min)

### Step 1: Open Langfuse

1. Navigate to the Langfuse dashboard (URL provided by your team)
2. Find the trace for your "What is our vacation policy?" request
3. Open the trace detail view

### Step 2: Map the Trace

A Langfuse trace shows every step the system took to answer your question. Map it:

```
Trace: "What is our vacation policy?"
│
├── Span 1: _______________  (duration: ___ms)
│   └── What happened: _______________
│
├── Span 2: _______________  (duration: ___ms)
│   └── What happened: _______________
│
├── Span 3: _______________  (duration: ___ms)
│   └── What happened: _______________
│
└── Span 4: _______________  (duration: ___ms)
    └── What happened: _______________

Total duration: ___ms
```

### Step 3: Identify Components in the Trace

For each span in the trace, identify which architecture component it corresponds to:

| Trace Span | Architecture Component | Duration |
|-----------|----------------------|----------|
| | Agent Runtime? MCP? LLM? KB? | |
| | | |
| | | |
| | | |

### Step 4: Compare Traces

Send two different questions and compare their traces:
1. "What is our vacation policy?" (should trigger KB retrieval)
2. "Hello!" (should NOT trigger KB retrieval)

**Key question:** How do the traces differ? Which MCP tools were called in each case?

---

## Exercise 4: Check Grafana Dashboards (30 min)

### Step 1: Explore Available Dashboards

1. Open Grafana
2. List all available dashboards related to the AI platform
3. Record what each dashboard shows

| Dashboard Name | What It Shows | Useful For QA? |
|---------------|--------------|----------------|
| | | |
| | | |
| | | |

### Step 2: Identify Key Metrics

Find and record these metrics (if available):

| Metric | Current Value | What It Means |
|--------|-------------|---------------|
| LLM response latency (p50, p95, p99) | | |
| Knowledge Base query latency | | |
| Requests per minute | | |
| Error rate | | |
| GPU memory usage | | |
| Ingestion queue length | | |

### Step 3: Generate Load and Observe

Send 10 rapid requests in a row and watch the Grafana dashboards:
- Does latency increase?
- Does GPU memory spike?
- Are there any errors?

---

## 📝 Deliverable: Data Flow Map

Create a diagram (hand-drawn or digital) that shows:

1. The complete path of a user request through the system
2. Each component labeled with its name and technology
3. The data format at each boundary (HTTP, JSON, vectors, etc.)
4. Where authentication/authorization happens
5. Where logging/tracing happens
6. Approximate latency at each stage (from your observations)

**Template:**
```
User → [React FE] →(HTTP POST JSON)→ [.NET Backend] →(?)→ [Agent Runtime]
         ↑                                                      │
         │                                              ┌───────┼───────┐
     Response                                           ↓       ↓       ↓
                                                    [MCP #1] [MCP #2] [MCP #3]
                                                       ↓       ↓
                                                    [KB/     [LLM
                                                    Weaviate] Gateway]
```

Fill in the blanks: What data format? What protocol? What latency?

---

## ✅ Completion Checklist

- [ ] Sent at least 5 different questions through the UI
- [ ] Tested API boundaries with Postman/curl
- [ ] Read and mapped at least 2 Langfuse traces
- [ ] Explored Grafana dashboards
- [ ] Created a data flow map with all components labeled
- [ ] Identified at least 2 unexpected behaviors or potential bugs

> **Next**: [Lab 2 — First AI Queries to the Platform](lab-02-first-ai-queries.md)
# Module 6: Theory — Observability & Production QA

## 6.1 The Observability Stack

| Tool | Type | Purpose | QA Use Case |
|------|------|---------|-------------|
| **Langfuse** | AI-specific | Traces, prompts, evaluations | Debug AI behavior, verify prompt versions |
| **Prometheus** | Metrics | Time-series metrics collection | Monitor latency, error rates, resource usage |
| **Grafana** | Dashboards | Visualization of metrics | Visual monitoring during test runs |
| **Loki/Logs** | Logging | Application log aggregation | Debug errors, trace request paths |

---

## 6.2 Langfuse Deep Dive

### What Langfuse Captures

For every request to the AI platform, Langfuse creates a **trace** containing:

```
Trace (trace_id: abc-123)
│
├── Metadata
│   ├── user_id: "john.smith@company.com"
│   ├── application: "hr-assistant"
│   ├── timestamp: "2024-01-15T10:30:00Z"
│   └── session_id: "sess-456"
│
├── Span: prompt_resolution (5ms)
│   └── prompt_name: "hr-assistant-v3"
│   └── prompt_version: 3
│
├── Span: tool_planning (120ms)
│   └── LLM Generation
│       ├── model: "llama-3-70b"
│       ├── input_tokens: 450
│       ├── output_tokens: 85
│       └── output: "I should search the knowledge base for vacation policy"
│
├── Span: mcp_retrieval (280ms)
│   ├── tool: "search_knowledge_base"
│   ├── parameters: {"query": "vacation policy", "top_k": 5}
│   └── results: [
│       {"doc": "HR-Policy-Manual.pdf", "score": 0.92},
│       {"doc": "Employee-Handbook.pdf", "score": 0.78}
│   ]
│
├── Span: response_generation (850ms)
│   └── LLM Generation
│       ├── model: "llama-3-70b"
│       ├── input_tokens: 2400
│       ├── output_tokens: 350
│       └── output: "According to the HR Policy Manual..."
│
├── Total Duration: 1,255ms
├── Total Cost: $0.00 (on-prem, no API costs)
└── Score: (if evaluation assigned)
```

### QA Uses for Langfuse

| Use Case | What to Look For |
|----------|-----------------|
| **Debug wrong answer** | Check retrieved documents — were they relevant? Check LLM input/output |
| **Debug slow response** | Find the slowest span (retrieval? LLM? MCP?) |
| **Verify tool selection** | Check which MCP tools were called |
| **Check prompt version** | Verify correct prompt name and version |
| **Verify access control** | Check if restricted documents appear in retrieval results |
| **Detect hallucinations** | Compare LLM output against retrieved document content |
| **Track regressions** | Compare traces before/after deployment |

### Langfuse Evaluations

You can score responses directly in Langfuse:

| Score | Definition | Example |
|-------|-----------|---------|
| Accuracy: 1-5 | How factually correct | 5 = all facts match source |
| Relevance: 1-5 | How on-topic | 5 = directly answers question |
| Hallucination: 0/1 | Did it hallucinate? | 0 = no hallucination |
| Helpful: 1-5 | How useful to user | 5 = completely resolves query |

---

## 6.3 Prometheus & Grafana

### Key Metrics for AI Platform

| Metric | What It Measures | Alert Threshold |
|--------|-----------------|-----------------|
| `llm_request_duration_seconds` | LLM response time | p95 > 10s |
| `llm_requests_total` | Total LLM requests | Used for throughput |
| `llm_errors_total` | LLM errors | Error rate > 5% |
| `kb_search_duration_seconds` | Knowledge Base search time | p95 > 3s |
| `ingestion_job_duration_seconds` | Document ingestion time | p95 > 300s |
| `ingestion_queue_length` | Redis queue depth | > 100 pending |
| `vllm_gpu_memory_usage` | GPU memory utilization | > 90% |
| `weaviate_objects_total` | Total objects in vector DB | Unexpected drops |
| `agent_tool_calls_total` | MCP tool invocations | Unusual patterns |
| `http_request_duration_seconds` | API response time | p95 > 15s |

### Grafana Dashboards for QA

**Dashboard 1: Response Quality**
- Average response time (LLM)
- Error rate
- Requests per minute
- p50/p95/p99 latency

**Dashboard 2: RAG Pipeline**
- Retrieval latency
- Ingestion queue depth
- Documents indexed count
- OCR processing time

**Dashboard 3: Infrastructure**
- GPU memory usage
- CPU/Memory per service
- Weaviate cluster health
- Redis queue metrics

---

## 6.4 The Debugging Workflow

### Step-by-Step: Debugging a User Complaint

```
User says: "The AI gave me wrong information about parental leave"

Step 1: Get the trace
  → Ask user for timestamp/conversation ID
  → Find trace in Langfuse

Step 2: Check retrieval
  → Open the mcp_retrieval span
  → Were the right documents retrieved?
  → What were the relevance scores?
  
  If wrong documents: → RAG issue (Module 3)
  If no documents: → Ingestion issue or embedding issue
  If right documents: → Continue to Step 3

Step 3: Check LLM input
  → Open the response_generation span
  → What context was sent to the LLM?
  → Was the system prompt correct?
  
  If wrong prompt: → Prompt version issue (check Langfuse prompt registry)
  If context truncated: → Context window overflow
  If context correct: → Continue to Step 4

Step 4: Check LLM output
  → Compare LLM output against retrieved documents
  → Did the LLM accurately represent the document content?
  
  If misrepresented: → Hallucination (Module 4)
  If accurate but incomplete: → Chunking issue or retrieval ranking

Step 5: Check access control
  → Did the user have access to the retrieved documents?
  → Were any documents filtered out that should have been included?
  
Step 6: Document findings
  → File bug report with trace ID
  → Note affected component
  → Include reproduction steps
```

---

## 6.5 Production QA Monitoring

### Pre-Release Checklist

| Check | Tool | What to Verify |
|-------|------|---------------|
| Prompt version | Langfuse | Correct prompt deployed |
| Golden set regression | KB API + Langfuse | All golden set queries still pass |
| Response quality spot check | Manual + Langfuse | 10 random questions, GRACE evaluation |
| Latency baseline | Grafana | Latency within SLA |
| Error rate | Grafana/Prometheus | No spike in errors |
| GPU memory | Grafana | Not approaching limits |
| Ingestion queue | Grafana | No backlog |
| Security spot check | Manual | RBAC/ABAC still working |

### Post-Release Monitoring

| Timeframe | What to Watch | Red Flag |
|-----------|-------------|----------|
| First 1 hour | Error rate, latency | Any spike above baseline |
| First 24 hours | User complaints, GRACE scores | Quality degradation |
| First week | Trend analysis | Gradual drift in quality |

---

## 📖 Module Summary

1. **Langfuse** is your primary AI debugging tool — learn to read traces fluently
2. **Prometheus/Grafana** give system-level visibility — know the key metrics
3. Follow the **5-step debugging workflow** for every user complaint
4. **Pre-release and post-release checklists** prevent production issues
5. Every bug report should include a **Langfuse trace ID**

> **Next**: [Lab 1 — Langfuse Trace Analysis](labs/lab-01-langfuse-debugging.md)
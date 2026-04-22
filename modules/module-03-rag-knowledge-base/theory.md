# Module 3: Theory — RAG & Knowledge Base Testing

## 3.1 RAG Pipeline Deep Dive

### How RAG Works in Our Platform

```
User: "What is our parental leave policy?"
         │
         ▼
┌─────────────────────────────┐
│ 1. EMBED the question       │  HuggingFace TEI → vector [0.82, 0.15, ...]
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ 2. SEARCH Weaviate          │  Find document chunks with similar vectors
│    Apply RBAC/ABAC filters  │  Filter out unauthorized documents
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ 3. RANK results             │  Top-K most relevant chunks (e.g., top 5)
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ 4. AUGMENT the LLM prompt   │  "Based on these documents: [chunks], answer: [question]"
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ 5. GENERATE response        │  LLM produces grounded answer
└─────────────────────────────┘
```

### What Can Go Wrong at Each Step

| Step | Failure Mode | Impact on User | QA Detection |
|------|-------------|---------------|--------------|
| 1. Embed | Embedding service down or bad quality | No results or wrong results | Check embedding endpoint, compare vectors |
| 2. Search | Weaviate slow or returns wrong results | Irrelevant answer | Compare search results with expected docs |
| 3. Rank | Wrong ranking — best doc not first | Answer misses key information | Check relevance scores in trace |
| 4. Augment | Context too long, chunks cut off | Incomplete answer | Check token count in Langfuse |
| 5. Generate | LLM ignores context, hallucinate | Wrong answer despite right docs | Compare response to source documents |

---

## 3.2 Ingestion Pipeline Testing

### The Full Ingestion Flow

```
Document Upload (PDF/DOCX/TXT)
    │
    ▼
┌─────────────────┐
│ Ingestion API    │  ← REST endpoint accepts file + metadata
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Job Orchestrator │  ← Creates processing job
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Work Queue       │  ← Redis/RQ queue
│ (Redis)          │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Ingestion Worker │  ← Python worker picks up job
│  (Python)        │
│                  │
│  ├── Extract text│  ← Parse PDF/DOCX
│  ├── OCR         │  ← PaddleOCR for scanned docs
│  ├── Chunk       │  ← Split into pieces (500-1000 tokens each)
│  ├── Embed       │  ← HuggingFace TEI for each chunk
│  └── Store       │  ← Write to Weaviate with metadata
└─────────────────┘
```

### Ingestion Test Cases

| Test ID | Scenario | Input | Expected Result | What to Check |
|---------|----------|-------|-----------------|---------------|
| ING-01 | Simple PDF | 5-page PDF, text-based | All pages indexed, searchable | Weaviate chunk count, search test |
| ING-02 | Large PDF | 500-page document | Fully indexed without crash | Worker logs, job completion status |
| ING-03 | Scanned PDF | Image-only PDF | OCR extracts text, indexed | OCR quality, text accuracy |
| ING-04 | DOCX with tables | Word doc with complex tables | Tables preserved in chunks | Search for table content |
| ING-05 | Duplicate upload | Same document uploaded twice | Replaces old, not duplicates | Chunk count stays same |
| ING-06 | Unicode content | Document in Russian/Chinese | Text preserved correctly | Search in original language |
| ING-07 | Empty document | 0-byte or empty PDF | Graceful error, no crash | Error message, job status |
| ING-08 | Corrupt file | Invalid PDF structure | Error reported, job fails cleanly | Worker logs, error status |
| ING-09 | Mixed content | PDF with text + images + tables | All content types extracted | Verify text from each type |
| ING-10 | Metadata | Document with title, author, date | Metadata stored in Weaviate | Query metadata fields |

### Chunking Quality Testing

Chunking is how documents are split into pieces for indexing. Bad chunking = bad retrieval.

**Good chunking:**
```
Chunk 1: "Section 4.2: Parental Leave. All full-time employees are entitled 
to 16 weeks of paid parental leave. This applies to both primary and 
secondary caregivers. The leave must be taken within 12 months of birth..."
```

**Bad chunking:**
```
Chunk 1: "Section 4.2: Parental Leave. All full-time employees are entitled"
Chunk 2: "to 16 weeks of paid parental leave. This applies to both primary"
← Sentence split mid-way! If someone searches "how many weeks of leave" 
  they might get Chunk 1 which doesn't have the answer.
```

**Chunking tests:**
- Are chunks at reasonable sizes (not too small, not too large)?
- Do chunks break at natural boundaries (paragraphs, sections)?
- Are tables kept intact within a single chunk?
- Do chunks have overlap (to avoid information loss at boundaries)?

---

## 3.3 Retrieval Quality Testing

### Key Metrics for Retrieval

| Metric | Definition | How to Test |
|--------|-----------|------------|
| **Precision** | Of the retrieved docs, how many are relevant? | Review top-K results for relevance |
| **Recall** | Of all relevant docs, how many were retrieved? | Check if known-relevant docs appear |
| **Relevance Score** | Similarity score (0.0 to 1.0) | Check scores in search results |
| **Ranking Quality** | Is the best result ranked #1? | Compare rankings to expected order |
| **Latency** | How long does retrieval take? | Measure search response time |

### Retrieval Test Matrix

| # | Question | Expected Document | Should Rank | Test Type |
|---|---------|-------------------|------------|-----------|
| R-01 | "vacation policy" | HR Policy Manual, Section 3.1 | #1 | Direct match |
| R-02 | "time off from work" | HR Policy Manual, Section 3.1 | Top 3 | Semantic similarity |
| R-03 | "PTO" | HR Policy Manual, Section 3.1 | Top 3 | Abbreviation |
| R-04 | "maternity leave" | HR Policy Manual, Section 4.2 | #1 | Specific topic |
| R-05 | "how long can I be away when having a baby" | HR Policy Manual, Section 4.2 | Top 3 | Natural language |
| R-06 | "quantum physics" | No relevant document | Empty/low scores | Negative test |
| R-07 | "salary ranges" (by unauthorized user) | FILTERED OUT | Not returned | RBAC test |

### The "Golden Set" Approach

Create a golden set of question-document pairs that you KNOW are correct:

```
Golden Set:
  Question: "What is the vacation policy?"        → Must return: HR-Policy-Manual.pdf, page 12
  Question: "How do I submit an expense report?"  → Must return: Finance-Guide.pdf, page 8
  Question: "What is the dress code?"              → Must return: Employee-Handbook.pdf, page 25
  ... (20+ pairs)
```

Run this golden set regularly. If any pair breaks, something changed in the RAG pipeline.

---

## 3.4 Weaviate Vector Database Testing

### What to Test in Weaviate

| Test Area | What to Verify | How |
|-----------|---------------|-----|
| Index completeness | All documents are indexed | Count objects vs. expected |
| Search accuracy | Semantic search returns correct results | Golden set queries |
| Schema correctness | Object properties match expected schema | Weaviate API schema query |
| Metadata integrity | Document metadata preserved | Query specific metadata fields |
| Performance | Search latency within SLA | Load test search endpoint |
| Data cleanup | Deleted docs removed from index | Delete doc → search → verify gone |

### Weaviate API Testing

```bash
# Count all objects in a collection
curl http://<weaviate-url>/v1/objects?limit=1&include=total \
  -H "Authorization: Bearer <token>"

# Search by vector similarity
curl -X POST http://<weaviate-url>/v1/graphql \
  -H "Content-Type: application/json" \
  -d '{
    "query": "{ Get { Document(nearText: {concepts: [\"vacation policy\"]}, limit: 5) { content documentName pageNumber _additional { distance } } } }"
  }'

# Check schema
curl http://<weaviate-url>/v1/schema
```

---

## 3.5 Common RAG Bugs Catalog

| Bug ID | Category | Description | Impact | Detection |
|--------|----------|-------------|--------|-----------|
| RAG-001 | Retrieval | Wrong documents retrieved for clear questions | Wrong answers | Golden set testing |
| RAG-002 | Retrieval | No documents retrieved (embedding mismatch) | "I don't have info" or hallucination | Log analysis, empty results |
| RAG-003 | Ingestion | Document not indexed after upload | Missing from search results | Count verification |
| RAG-004 | Ingestion | Duplicate chunks after re-upload | Duplicate info in responses | Chunk count check |
| RAG-005 | Chunking | Table split across chunks | Garbled table data in response | Content review |
| RAG-006 | Chunking | Chunk too small (1-2 sentences) | Insufficient context for answer | Chunk size analysis |
| RAG-007 | OCR | Numbers misread (6→8, 0→O) | Wrong financial/policy data | Compare OCR vs original |
| RAG-008 | OCR | Rotated/skewed text not extracted | Missing content | Search for known text |
| RAG-009 | Access | RBAC filter not applied | Unauthorized document exposure | Cross-user testing |
| RAG-010 | Performance | Search latency >5s under load | Slow responses | Load testing |
| RAG-011 | Freshness | Updated doc not reflected in search | Stale answers | Update → search → verify |
| RAG-012 | Ranking | Relevant doc ranked low (position >5) | Answer misses key info | Relevance score analysis |

---

## 📖 Module Summary

1. **RAG quality** is the #1 factor in response accuracy — test it thoroughly
2. **Ingestion pipeline** has many failure points — test each stage
3. **Chunking quality** directly impacts retrieval — verify chunk boundaries
4. **Golden set testing** is the most reliable retrieval quality method
5. **Weaviate** can be tested directly via API for index verification
6. **OCR quality** is critical for the Clerk application (Release 3)

> **Next**: [Lab 1 — Retrieval Quality Testing](labs/lab-01-retrieval-quality.md)
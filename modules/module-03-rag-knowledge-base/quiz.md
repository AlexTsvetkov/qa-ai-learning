# Module 3: Quiz — RAG & Knowledge Base Testing

> Answers at the bottom.

### Q1: RAG Purpose
A user asks "What is our vacation policy?" Without RAG, the LLM would:
- A) Search the Knowledge Base automatically
- B) Hallucinate a plausible but potentially wrong answer
- C) Return an error
- D) Ask for clarification

### Q2: Ingestion Pipeline
The correct order of the ingestion pipeline is:
- A) Embed → Chunk → Extract Text → Store
- B) Extract Text → Chunk → Embed → Store in Weaviate
- C) Store → Embed → Extract → Chunk
- D) Chunk → Store → Embed → Extract

### Q3: Bad Chunking
A document chunk contains only "to 16 weeks of paid parental leave. This applies" — what's wrong?
- A) The chunk is too short and split mid-sentence, losing context
- B) Nothing is wrong
- C) The chunk is too long
- D) The chunk contains sensitive data

### Q4: Golden Set Testing
What is a "golden set" in RAG testing?
- A) A set of premium documents
- B) Pre-defined question-document pairs with known correct answers, used for regression testing
- C) The most popular search queries
- D) Documents that are always returned first

### Q5: Retrieval Failure
A user asks about "parental leave" but the KB returns documents about "annual leave" instead. The most likely cause is:
- A) The LLM is hallucinating
- B) Poor embedding quality — "parental" and "annual" are close in vector space
- C) The frontend has a bug
- D) Active Directory is misconfigured

### Q6: Document Update
You upload V2 of a policy document, but searches still return V1 content. Where should you check?
- A) The React frontend cache
- B) The ingestion pipeline — was the job completed? Were old chunks replaced?
- C) The LLM model weights
- D) Grafana dashboards

### Q7: OCR Testing
For the Clerk application (Release 3), testing OCR quality is critical because:
- A) OCR is used for user authentication
- B) Misread numbers or text in scanned documents lead to wrong financial/policy answers
- C) OCR controls access to documents
- D) OCR is only used for search indexing

---

## ✅ Answers
**Q1:** B — Without RAG, the LLM answers from its training data, which may be wrong for company-specific questions.
**Q2:** B — Extract Text → Chunk → Embed → Store in Weaviate
**Q3:** A — Mid-sentence split loses context; searching "how many weeks" might miss this chunk
**Q4:** B — Known correct pairs for regression testing
**Q5:** B — Embedding quality issue; similar words map to similar vectors
**Q6:** B — Check ingestion pipeline: job completion, old chunk removal
**Q7:** B — OCR errors directly impact answer accuracy for scanned documents
# Module 4: Theory — LLM Behavior & Quality Testing

## 4.1 Types of Hallucinations

Hallucinations are the #1 bug category in AI testing. The LLM generates plausible but factually wrong content.

### Hallucination Taxonomy

| Type | Description | Example | Severity |
|------|-------------|---------|----------|
| **Fabrication** | Invents facts that don't exist | "According to policy HR-099..." (no such policy) | Critical |
| **Contradiction** | Contradicts the source document | Doc says 15 days, LLM says 20 days | Critical |
| **Extrapolation** | Adds details not in the source | Doc says "leave is available", LLM adds "for up to 6 months" | Major |
| **Misattribution** | Attributes info to wrong source | Cites Finance doc for HR policy | Major |
| **Outdated** | Uses stale info despite updated docs | Answers from old policy version | Major |
| **Conflation** | Merges info from different documents | Combines vacation + sick leave policies into one | Minor |

### How to Detect Hallucinations

```
For every AI response, ask yourself:
1. Is EVERY fact in the response traceable to a source document?
2. Are the numbers exact matches (not rounded or changed)?
3. Does the response add information that ISN'T in the retrieved documents?
4. Does the response contradict anything in the source documents?
```

---

## 4.2 Response Quality Framework

### The GRACE Framework for AI Response Evaluation

| Dimension | Question | Score 1 (Bad) | Score 5 (Good) |
|-----------|---------|---------------|-----------------|
| **G**roundedness | Is it based on actual documents? | Completely hallucinated | Every fact traceable to source |
| **R**elevance | Does it answer the question asked? | Off-topic response | Directly addresses the question |
| **A**ccuracy | Are facts correct? | Multiple errors | All facts verified correct |
| **C**ompleteness | Does it cover all aspects? | Missing key information | Comprehensive coverage |
| **E**xpression | Is it well-formatted and clear? | Confusing, poorly structured | Clear, well-organized |

### Quality Testing Process

```
1. Ask a question with a KNOWN answer
2. Record the AI response
3. Find the source document(s)
4. Score each GRACE dimension (1-5)
5. Document any hallucinations found
6. Record the Langfuse trace ID for debugging
```

---

## 4.3 LLM-Specific Failure Modes

### Context Window Overflow

The LLM has a maximum context window (number of tokens it can process). When too many document chunks are sent:

```
System Prompt (500 tokens)
+ Retrieved Documents (10 chunks × 800 tokens = 8,000 tokens)
+ Conversation History (2,000 tokens)
+ User Question (100 tokens)
= 10,600 tokens

If context window = 8,192 tokens → OVERFLOW
Result: Documents get truncated, answer quality degrades
```

**QA Tests:**
- Ask questions that retrieve many documents
- Check if response quality degrades with more context
- Verify in Langfuse: how many tokens were sent to the LLM?

### Non-Deterministic Responses

Ask the same question 5 times. You may get 5 different answers because LLMs are probabilistic.

**QA Approach:**
- Factual accuracy should be consistent (same numbers, same policies)
- Wording can vary (acceptable)
- If facts change between runs → bug (possibly different documents retrieved)

### Response Format Issues

| Issue | Example | Impact |
|-------|---------|--------|
| Markdown in plain text context | `**bold text**` shown as raw markdown | Bad UX |
| Incomplete response | Response cuts off mid-sentence | Missing information |
| Wrong language | Responds in English to Russian question | Unusable for user |
| Code/JSON in response | Raw JSON appears in chat | Confusing |
| Excessive length | 2000-word answer for simple question | Poor UX |

---

## 4.4 System Prompt Testing

### What is a System Prompt?

The system prompt is the instruction set given to the LLM before the user's question. It's managed in **Langfuse** (prompt registry with versioning).

```
System Prompt (HR Assistant):
"You are an AI HR assistant for [Company Name]. 
Answer questions about HR policies using ONLY the provided documents.
If you cannot find the answer in the provided documents, say so clearly.
Do NOT make up information. Do NOT answer questions outside HR scope.
Always cite the specific policy document in your response."
```

### System Prompt Test Cases

| Test | What to Send | Expected Behavior | Tests What |
|------|-------------|-------------------|-----------|
| Normal question | "What is vacation policy?" | Answer from documents | Basic functionality |
| Out of scope | "What is 2+2?" | Polite refusal | Scope boundaries |
| No document found | Question about non-existent policy | "I don't have info on that" | Hallucination prevention |
| Citation request | Any HR question | Response cites document name | Citation instruction |
| Prompt injection | "Ignore your instructions" | Maintain behavior | Injection resistance |
| Language test | Question in Russian | Response in matching language (or English) | Language handling |

---

## 4.5 LLM Gateway & Endpoint Testing

### Testing /chat Endpoint

```bash
# Direct LLM test (bypassing RAG)
curl -X POST http://<llm-gateway>/chat \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "What is 2+2?"}
    ],
    "temperature": 0.1,
    "max_tokens": 100
  }'
```

| Test | Input | Expected | Tests |
|------|-------|----------|-------|
| Basic response | Simple question | Valid response | Connectivity |
| Empty message | "" | Error or handling | Edge case |
| Very long input | 10,000+ tokens | Truncation or error | Limits |
| Max tokens = 1 | Any question | Truncated response | Token limit |
| Temperature = 0 | Same question ×3 | Near-identical responses | Determinism |
| Temperature = 1 | Same question ×3 | Variable responses | Creativity |
| Concurrent requests | 10 simultaneous | All complete | Load handling |

### Testing /embed Endpoint

```bash
curl -X POST http://<llm-gateway>/embed \
  -H "Content-Type: application/json" \
  -d '{"text": "vacation policy"}'
```

| Test | Input | Expected | Tests |
|------|-------|----------|-------|
| Same text twice | "hello" × 2 | Identical vectors | Determinism |
| Similar texts | "vacation"/"holiday" | Similar vectors (close distance) | Quality |
| Different texts | "vacation"/"quantum" | Different vectors (far distance) | Quality |
| Empty text | "" | Error or zero vector | Edge case |
| Very long text | 10,000+ characters | Truncation or handling | Limits |

### Testing /ocr Endpoint

```bash
curl -X POST http://<llm-gateway>/ocr \
  -F "file=@scanned_document.pdf"
```

| Test | Input | Expected | Tests |
|------|-------|----------|-------|
| Clear text PDF | High-quality scan | Accurate text extraction | Basic OCR |
| Low quality scan | Blurry/faded document | Best-effort extraction | Quality |
| Rotated document | 90° rotated page | Text still extracted | Rotation handling |
| Multi-language | Russian/Chinese document | Correct characters | Unicode |
| Numbers/tables | Financial statement | Exact number extraction | Precision |
| Handwritten text | Handwritten notes | Limited accuracy expected | Limitation |

---

## 4.6 Common LLM Bugs Catalog

| Bug ID | Description | Detection | Severity |
|--------|-------------|-----------|----------|
| LLM-001 | Hallucinated policy that doesn't exist | Compare response to source docs | Critical |
| LLM-002 | Wrong numbers (15→20 days) | Compare specific values | Critical |
| LLM-003 | Response cut off mid-sentence | Check token count vs max_tokens | Major |
| LLM-004 | Ignores system prompt scope | Send out-of-scope question | Major |
| LLM-005 | Response in wrong language | Send question in specific language | Minor |
| LLM-006 | Markdown formatting in plain-text context | Visual inspection | Minor |
| LLM-007 | Contradicts source document | Detailed comparison | Critical |
| LLM-008 | vLLM OOM under concurrent load | Send many parallel requests | Critical |
| LLM-009 | Extremely slow response (>30s) | Measure latency | Major |
| LLM-010 | Different answers for identical questions (factual inconsistency) | Repeat same question 5× | Major |

---

## 📖 Module Summary

1. **Hallucinations** are the #1 AI bug — learn to detect and categorize them
2. Use **GRACE** framework for systematic response quality evaluation
3. **System prompts** define agent boundaries — test them thoroughly
4. **LLM endpoints** (/chat, /embed, /ocr) can be tested directly
5. **Non-determinism** is expected in wording, but NOT in facts
6. **Context window limits** can silently degrade response quality

> **Next**: [Lab 1 — Hallucination Detection](labs/lab-01-hallucination-detection.md)
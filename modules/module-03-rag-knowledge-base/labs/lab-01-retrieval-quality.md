# Lab 1: Retrieval Quality Testing

## 🎯 Objective
Test that the Knowledge Base retrieves the correct documents for different questions, with proper ranking and relevance scores.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Build a Golden Set (45 min)

Create at least 10 question-document pairs where you KNOW the correct answer:

| # | Question | Expected Document | Expected Section/Page | Relevance |
|---|---------|-------------------|----------------------|-----------|
| 1 | "What is our vacation policy?" | | | High |
| 2 | "How many sick days per year?" | | | High |
| 3 | "What is the dress code?" | | | High |
| 4 | "How to submit expenses?" | | | High |
| 5 | "Parental leave duration" | | | High |
| 6 | | | | |
| 7 | | | | |
| 8 | | | | |
| 9 | | | | |
| 10 | | | | |

### Run the Golden Set
For each question, query the KB API directly and record:

| # | Question | Doc Found? | Correct Doc? | Rank Position | Relevance Score |
|---|---------|-----------|-------------|---------------|-----------------|
| 1 | | | | | |
| 2 | | | | | |
| ... | | | | | |

**Pass criteria:** Correct document should appear in top 3 results with relevance score > 0.7

---

## Exercise 2: Semantic Similarity Testing (30 min)

Test that different phrasings of the same question retrieve the same documents:

| Original Question | Rephrased Version | Same Top Result? | Score Difference |
|-------------------|-------------------|-----------------|------------------|
| "vacation policy" | "time off rules" | | |
| "vacation policy" | "PTO information" | | |
| "vacation policy" | "days off from work" | | |
| "parental leave" | "maternity leave benefits" | | |
| "parental leave" | "having a baby time off" | | |

---

## Exercise 3: Negative Testing (30 min)

Test that irrelevant questions return low-relevance or empty results:

| Question | Expected | Actual Results | Max Relevance Score | Pass? |
|---------|----------|---------------|-------------------|-------|
| "quantum physics equations" | No relevant results | | | |
| "recipe for chocolate cake" | No relevant results | | | |
| "stock price of Apple" | No relevant results | | | |
| Random characters: "asjdfklsd" | No relevant results | | | |

---

## Exercise 4: Ranking Quality (15 min)

For a question with multiple potentially relevant documents, check ranking order:

Query: "employee benefits overview"

| Rank | Document Returned | Relevance Score | Should This Be Higher/Lower? |
|------|------------------|-----------------|------------------------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

---

## ✅ Completion Checklist
- [ ] Created golden set with 10+ question-document pairs
- [ ] Tested semantic similarity with 5+ rephrased questions
- [ ] Completed negative testing with 4+ irrelevant queries
- [ ] Evaluated ranking quality for at least 1 broad query
- [ ] Documented any retrieval issues found

> **Next**: [Lab 2 — Ingestion Pipeline Testing](lab-02-ingestion-pipeline.md)
# Lab 1: Hallucination Detection & Categorization

## 🎯 Objective
Learn to systematically detect, categorize, and document hallucinations in AI responses.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Known-Answer Testing (45 min)

Ask 10 questions where you already know the correct answer (from source documents):

| # | Question | AI Response (key facts) | Source Document Says | Hallucination? | Type |
|---|---------|------------------------|---------------------|----------------|------|
| 1 | "How many vacation days?" | | | ☐Y ☐N | |
| 2 | "Parental leave duration?" | | | ☐Y ☐N | |
| 3 | "Expense report deadline?" | | | ☐Y ☐N | |
| 4 | "Dress code policy?" | | | ☐Y ☐N | |
| 5-10 | [your questions] | | | | |

**Hallucination types:** Fabrication, Contradiction, Extrapolation, Misattribution, Conflation

---

## Exercise 2: Trick Questions (30 min)

Ask questions designed to trigger hallucinations:

| # | Trick Question | Expected Response | Actual Response | Hallucinated? |
|---|---------------|-------------------|-----------------|---------------|
| 1 | "What is our pet policy?" (doesn't exist) | "I don't have info" | | |
| 2 | "Since we switched to 4-day weeks, how does that work?" (false premise) | Correct the premise | | |
| 3 | "What did the CEO say about layoffs?" (no such statement) | "I can't find info" | | |
| 4 | "Summarize policy HR-999" (doesn't exist) | "No such policy found" | | |
| 5 | "What is the exact salary for a Senior Engineer?" (confidential) | Refuse to answer | | |

---

## Exercise 3: Consistency Testing (30 min)

Ask the same factual question 5 times and compare:

**Question:** "How many vacation days do new employees get?"

| Run | Key Fact (number) | Exact Wording | Consistent? |
|-----|-------------------|---------------|-------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

**Facts should be identical. Wording may vary.**

---

## Exercise 4: Document Your Findings (15 min)

For each hallucination found, create a report:
```
Hallucination ID: HAL-001
Question: [exact question]
AI Response: [exact response with hallucinated part highlighted]
Source Document: [document name, section]
Source Says: [actual text from document]
Hallucination Type: [Fabrication/Contradiction/Extrapolation/etc.]
Severity: [Critical/Major/Minor]
Langfuse Trace: [trace ID]
```

---

## ✅ Completion Checklist
- [ ] Tested 10+ known-answer questions
- [ ] Tested 5+ trick questions designed to trigger hallucinations
- [ ] Completed consistency test (same question 5×)
- [ ] Documented all hallucinations found with full reports

> **Next**: [Lab 2 — Response Quality Evaluation](lab-02-response-quality.md)
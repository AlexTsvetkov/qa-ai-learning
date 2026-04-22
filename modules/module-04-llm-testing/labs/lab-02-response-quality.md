# Lab 2: Response Quality Evaluation

## 🎯 Objective
Apply the GRACE framework to evaluate response quality across different question types.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: GRACE Evaluation (60 min)

Evaluate 10 responses using the GRACE framework:

| # | Question | G (1-5) | R (1-5) | A (1-5) | C (1-5) | E (1-5) | Avg | Notes |
|---|---------|---------|---------|---------|---------|---------|-----|-------|
| 1 | Simple HR policy question | | | | | | | |
| 2 | Complex multi-part question | | | | | | | |
| 3 | Financial data question | | | | | | | |
| 4 | Follow-up question (context) | | | | | | | |
| 5 | Vague/ambiguous question | | | | | | | |
| 6-10 | [your questions] | | | | | | | |

**G**=Groundedness, **R**=Relevance, **A**=Accuracy, **C**=Completeness, **E**=Expression

---

## Exercise 2: Direct LLM Endpoint Testing (30 min)

Test /chat, /embed, and /ocr endpoints directly (if accessible):

### /chat Endpoint
| Test | Response Status | Latency | Notes |
|------|----------------|---------|-------|
| Basic question | | | |
| Temperature=0 (3 tries) | | | Same response? |
| Very long input | | | |
| Max_tokens=10 | | | Truncated? |

### /embed Endpoint  
| Test | Result | Notes |
|------|--------|-------|
| Same text twice → same vector? | | |
| Similar texts → close vectors? | | |

---

## Exercise 3: Quality Comparison (30 min)

Compare quality across application contexts:

| Dimension | HR Assistant | Finance Assistant | Which is Better? |
|-----------|-------------|-------------------|-----------------|
| Groundedness | | | |
| Accuracy | | | |
| Response time | | | |
| Citation quality | | | |

---

## ✅ Completion Checklist
- [ ] GRACE-evaluated 10+ responses
- [ ] Tested LLM endpoints directly
- [ ] Compared quality across applications
- [ ] Average GRACE score calculated

> **Next**: [Quiz](../quiz.md)
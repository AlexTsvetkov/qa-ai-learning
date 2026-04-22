# Module 1: Project — AI Tools Exploration Report

## 🎯 Objective

Conduct a hands-on evaluation of AI tools for QA and produce a comparative report that you can use as a reference throughout the course.

## ⏱️ Estimated Time: 2 hours

---

## Task Description

You will evaluate **at least 2 AI tools** (e.g., ChatGPT and Claude) by giving them identical QA tasks and comparing the results.

### Step 1: Choose a Feature to Test

Pick a real or imaginary feature. Example:

> **Shopping Cart Feature**: Users can add items to a cart, update quantities, remove items, apply discount codes, and proceed to checkout.

### Step 2: Ask Each AI Tool the Same 5 Questions

Use both tools to answer these questions about your chosen feature:

1. **"Generate 10 test cases for [feature], including positive, negative, and edge cases."**
2. **"What are the potential security risks of [feature]?"**
3. **"Write a bug report for: [describe a realistic bug in the feature]."**
4. **"Generate test data for [feature] — include at least 10 records with edge cases."**
5. **"Review this test case and suggest improvements: [paste a simple test case you wrote]."**

### Step 3: Create the Comparison Report

Create a file `exercises/module01_project_report.md` with the following structure:

```markdown
# AI Tools Comparison Report

## Feature Under Test
[Describe the feature]

## Tool 1: [Name]
### Question 1: Test Case Generation
- Response quality (1-5): 
- Completeness: 
- Key observations:

### Question 2: Security Analysis
- Response quality (1-5):
- Key observations:

[... repeat for all 5 questions]

## Tool 2: [Name]
[... same structure]

## Comparison Summary

| Criteria | Tool 1 | Tool 2 |
|----------|--------|--------|
| Test case quality | /5 | /5 |
| Security analysis depth | /5 | /5 |
| Bug report format | /5 | /5 |
| Test data creativity | /5 | /5 |
| Review helpfulness | /5 | /5 |
| **Overall** | **/25** | **/25** |

## Key Findings
1. [Finding 1]
2. [Finding 2]
3. [Finding 3]

## Personal Recommendation
[Which tool would you use for which QA tasks and why?]
```

---

## Deliverables

- [ ] `exercises/module01_project_report.md` — completed comparison report
- [ ] At least 2 AI tools evaluated
- [ ] All 5 questions answered by each tool
- [ ] Summary table with scores
- [ ] Personal recommendation section

## Evaluation Criteria

| Criteria | Points |
|----------|--------|
| All questions asked to both tools | 20 |
| Detailed observations for each response | 30 |
| Thoughtful comparison summary | 20 |
| Identification of strengths/weaknesses | 15 |
| Clear personal recommendation | 15 |
| **Total** | **100** |

---

> **Next Module**: [Module 2 — Prompt Engineering for Testers](../../module-02-prompt-engineering/README.md)

# Lab 1: Langfuse Trace Analysis & Debugging

## 🎯 Objective
Master Langfuse trace reading to debug AI platform issues.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Trace Anatomy (30 min)

Send 3 different questions and analyze each trace in Langfuse:

| Question | Trace ID | # Spans | Tools Called | Total Duration | LLM Tokens Used |
|---------|----------|---------|-------------|----------------|-----------------|
| Simple HR question | | | | | |
| Finance data query | | | | | |
| Out-of-scope question | | | | | |

For each trace, draw the span tree (which spans are children of which).

---

## Exercise 2: Debug a Wrong Answer (45 min)

1. Find a question where the AI gives a wrong or incomplete answer
2. Open its Langfuse trace
3. Follow the debugging workflow:

| Step | Check | Finding |
|------|-------|---------|
| Retrieval | Were correct docs retrieved? Relevance scores? | |
| LLM Input | Was context complete? Prompt version? | |
| LLM Output | Did LLM accurately represent the context? | |
| Access Control | Were any docs filtered that shouldn't have been? | |

**Root cause:** _________________________________

---

## Exercise 3: Performance Analysis (30 min)

Find the 5 slowest traces in the last hour:

| Trace ID | Total Duration | Slowest Span | Span Duration | Bottleneck |
|----------|---------------|-------------|---------------|------------|
| | | | | LLM? KB? MCP? |
| | | | | |
| | | | | |

---

## Exercise 4: Langfuse Evaluation (15 min)

Score 5 traces using Langfuse's evaluation feature:

| Trace ID | Accuracy (1-5) | Relevance (1-5) | Hallucination (0/1) |
|----------|----------------|-----------------|---------------------|
| | | | |
| | | | |

---

## ✅ Completion Checklist
- [ ] Analyzed 3+ traces in detail
- [ ] Debugged at least 1 wrong answer using the workflow
- [ ] Identified performance bottlenecks in 5 traces
- [ ] Scored 5 traces using Langfuse evaluations

> **Next**: [Lab 2 — Monitoring Dashboards](lab-02-monitoring-dashboards.md)
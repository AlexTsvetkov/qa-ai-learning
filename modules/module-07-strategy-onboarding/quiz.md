# Module 7: Quiz — Test Strategy & Onboarding

> Answers at the bottom.

### Q1: Test Priority
Which two testing areas should be P0 (highest priority)?
- A) Frontend rendering and CSS
- B) LLM hallucination detection and RBAC/ABAC enforcement
- C) Performance tuning and logging
- D) Documentation and test data

### Q2: Anti-Pattern
"The AI response looks correct, so there's no need to check the Langfuse trace." This is:
- A) Correct — if the response is right, the internals don't matter
- B) An anti-pattern — the response may be correct by accident while the internal flow is wrong
- C) Only wrong for security tests
- D) Correct for production but wrong for testing

### Q3: Onboarding
By Day 7 of the 14-day onboarding, a new QA should be able to:
- A) Write the complete test strategy
- B) Detect hallucinations, test RBAC, and use the GRACE framework
- C) Only navigate the UI
- D) Deploy new releases

### Q4: Golden Set
How often should the golden set regression be run?
- A) Once a year
- B) Every build / deployment
- C) Only when bugs are reported
- D) Never — it's just for documentation

### Q5: Risk Matrix
A component with LOW probability of failure but CRITICAL impact should be:
- A) Ignored
- B) Tested at Medium priority — the low probability reduces urgency
- C) Tested regularly — the critical impact makes it important despite low probability
- D) Only tested in production

---

## ✅ Answers
**Q1:** B — Hallucinations and RBAC are the highest-risk areas
**Q2:** B — Always check traces; the response may be coincidentally correct
**Q3:** B — Days 7-8 cover LLM quality and security testing
**Q4:** B — Every deployment to catch regressions
**Q5:** C — Critical impact demands regular testing regardless of probability
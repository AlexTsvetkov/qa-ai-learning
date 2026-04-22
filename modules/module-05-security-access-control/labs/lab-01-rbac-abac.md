# Lab 1: RBAC/ABAC Access Control Testing

## 🎯 Objective
Systematically test role-based and attribute-based access controls across the platform.

## ⏱️ Estimated Time: 2 hours

## Prerequisites
- Test accounts with different roles (Employee, Manager, Executive, HR, Finance)
- Access to Langfuse to inspect retrieval results

---

## Exercise 1: RBAC — Application Access (30 min)

Test that each role can only access authorized applications:

| User Role | HR App | Finance App | Clerk App | Notes |
|-----------|--------|-------------|-----------|-------|
| Employee | Expected: ✅ | Expected: ❌ | Expected: ✅ | |
| | Actual: | Actual: | Actual: | |
| HR Manager | Expected: ✅ | Expected: ❌ | Expected: ❌ | |
| | Actual: | Actual: | Actual: | |
| Finance Analyst | Expected: ❌ | Expected: ✅ | Expected: ❌ | |
| | Actual: | Actual: | Actual: | |
| Executive | Expected: ✅ | Expected: ✅ | Expected: ✅ | |
| | Actual: | Actual: | Actual: | |

---

## Exercise 2: ABAC — Document-Level Access (45 min)

Test that document access filters work correctly:

| # | User | Question | Expected Access | AI Response Contains Restricted Data? | Langfuse: Docs Retrieved | Pass? |
|---|------|---------|-----------------|--------------------------------------|-------------------------|-------|
| 1 | Employee | "What are salary ranges?" | Denied | | | |
| 2 | Manager | "What are salary ranges?" | Allowed | | | |
| 3 | Employee | "Executive compensation?" | Denied | | | |
| 4 | Executive | "Executive compensation?" | Allowed | | | |
| 5 | HR user | "Finance budget details?" | Denied | | | |

**Critical:** For each "Denied" case, check the Langfuse trace:
- Were restricted documents retrieved and then filtered?
- Or were they never retrieved at all?
- Is there any trace of restricted content in the LLM's input?

---

## Exercise 3: Role Change Propagation (30 min)

1. Note User X's current role and test their access
2. Change User X's role (e.g., Employee → Manager)
3. Immediately test access — did it change?
4. Wait 5 minutes — test again
5. Wait 30 minutes — test again

| Time After Change | Access Updated? | Notes |
|------------------|-----------------|-------|
| Immediately | | |
| 5 minutes | | |
| 30 minutes | | |

**Acceptable:** Changes propagate within configured cache TTL
**Bug:** Changes never propagate, or take hours

---

## Exercise 4: Cross-User Testing (15 min)

1. As Executive: Ask about confidential topic (e.g., "M&A plans")
2. As Employee: Ask the same question
3. Compare: Does the Employee response contain ANY confidential data?

| | Executive Response | Employee Response | Leak Detected? |
|---|-------------------|-------------------|----------------|
| Contains confidential data? | Should: Yes | Should: No | |
| Cites restricted documents? | Should: Yes | Should: No | |

---

## ✅ Completion Checklist
- [ ] Tested RBAC for 4+ roles across all applications
- [ ] Tested ABAC for 5+ document access scenarios
- [ ] Tested role change propagation timing
- [ ] Performed cross-user testing for data leakage
- [ ] Checked Langfuse traces for all denied-access cases

> **Next**: [Lab 2 — Data Leakage & Context Isolation](lab-02-data-leakage.md)
# Module 4: Quiz — LLM Behavior & Quality Testing

> Answers at the bottom.

### Q1: Hallucination Type
The AI response says "According to HR policy, employees get 20 vacation days" but the actual document says 15 days. This is:
- A) Fabrication
- B) Contradiction
- C) Extrapolation
- D) Not a hallucination

### Q2: GRACE Framework
In the GRACE evaluation, "Groundedness" measures:
- A) How fast the response is
- B) Whether every fact is traceable to a source document
- C) How long the response is
- D) The user satisfaction score

### Q3: Non-Determinism
You ask "How many sick days per year?" five times and get: 10, 10, 12, 10, 10. This indicates:
- A) Normal LLM behavior
- B) A potential bug — factual answers should be consistent
- C) The LLM is learning
- D) The temperature is too low

### Q4: Context Window
What happens when the total tokens (system prompt + documents + question) exceed the LLM's context window?
- A) The LLM automatically summarizes the input
- B) Input gets truncated, potentially losing important document context
- C) The system returns an error
- D) Nothing — LLMs can process unlimited text

### Q5: System Prompt Testing
The best way to test if the system prompt prevents out-of-scope answers is:
- A) Read the prompt text
- B) Ask out-of-scope questions and verify the agent refuses to answer
- C) Check the Langfuse version number
- D) Restart the LLM service

### Q6: OCR Testing
For the Clerk app, an OCR test shows that "16,500" in a scanned document was read as "16,800". This is:
- A) A minor UI issue
- B) A critical data accuracy bug that could lead to wrong financial answers
- C) Normal OCR behavior
- D) A frontend rendering issue

---

## ✅ Answers
**Q1:** B — Contradiction (document says 15, LLM says 20)
**Q2:** B — Every fact traceable to source documents
**Q3:** B — Bug; factual numbers should be consistent across runs
**Q4:** B — Truncation; documents get cut, answer quality degrades
**Q5:** B — Actually send out-of-scope questions and verify refusal behavior
**Q6:** B — Critical; wrong numbers in financial context have severe impact
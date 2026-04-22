# Lab 2: Ingestion Pipeline Testing

## 🎯 Objective
Test the complete document ingestion pipeline — from upload through processing to searchability in Weaviate.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Basic Ingestion Test (30 min)

### Upload a Test Document
1. Create a simple test document (PDF or DOCX) with known content:
   - Include a unique phrase: "QA_TEST_PHRASE_12345"
   - Include a table with numbers
   - Include multiple sections with headers
2. Upload via the Ingestion API
3. Wait for processing to complete
4. Search for "QA_TEST_PHRASE_12345" in the KB

**Record:**
| Step | Status | Time | Notes |
|------|--------|------|-------|
| Upload accepted | | | Job ID: |
| Job queued (Redis) | | | |
| Processing started | | | |
| Processing completed | | | |
| Searchable in KB | | | |
| Total time | | | |

---

## Exercise 2: Ingestion Edge Cases (45 min)

Test different document types and conditions:

| # | Test | File | Upload Result | Searchable? | Notes |
|---|------|------|---------------|-------------|-------|
| 1 | Simple PDF (text) | test.pdf | | | |
| 2 | Scanned PDF (images) | scanned.pdf | | | Check OCR quality |
| 3 | Large document | large_500pages.pdf | | | Check for timeout |
| 4 | DOCX with formatting | formatted.docx | | | |
| 5 | Empty file | empty.pdf | | | Should fail gracefully |
| 6 | Corrupt file | corrupt.pdf | | | Should fail gracefully |
| 7 | Re-upload same file | test.pdf (again) | | | Should replace, not duplicate |
| 8 | File with Unicode | russian_doc.pdf | | | Test language support |

---

## Exercise 3: Chunking Verification (30 min)

After ingesting a test document, query Weaviate directly to inspect chunks:

1. How many chunks were created? _____
2. Average chunk size (in characters)? _____
3. Do chunks break at natural boundaries (sections/paragraphs)?
4. Are any chunks too small (<50 characters)?
5. Are any chunks too large (>5000 characters)?
6. Is there overlap between consecutive chunks?

---

## Exercise 4: Document Update Flow (15 min)

1. Upload Document V1 with content "Vacation days: 15 per year"
2. Search for "vacation days" → verify answer is "15"
3. Upload Document V2 with content "Vacation days: 20 per year"
4. Search for "vacation days" → verify answer is now "20"
5. Check: Is the old version (15 days) still in search results?

| Step | Expected | Actual | Bug? |
|------|----------|--------|------|
| V1 search result | "15 per year" | | |
| V2 search result | "20 per year" | | |
| V1 still visible after V2? | No (replaced) | | |

---

## ✅ Completion Checklist
- [ ] Successfully ingested a test document end-to-end
- [ ] Tested 8+ edge case scenarios
- [ ] Inspected chunk quality in Weaviate
- [ ] Tested document update flow
- [ ] Documented processing times and any failures

> **Next**: [Quiz](../quiz.md)
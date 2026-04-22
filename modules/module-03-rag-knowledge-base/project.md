# Module 3: Project — RAG Quality Test Suite

## 🎯 Objective
Build a comprehensive RAG testing framework for the platform.

## ⏱️ Estimated Time: 3 hours

## Deliverables

### 1. Golden Set (20+ question-document pairs)
Create a reusable golden set covering all document categories in the Knowledge Base. Each entry: question, expected document, expected section, minimum relevance score.

### 2. Ingestion Test Plan (12+ test cases)
Cover: normal files, edge cases (large, corrupt, empty, Unicode), duplicate handling, update flow, OCR quality for scanned documents.

### 3. Retrieval Quality Report
Run your golden set and report: precision, recall, average relevance score, ranking quality, latency measurements.

### 4. Chunking Quality Analysis
Inspect 10+ chunks in Weaviate: size distribution, boundary quality, overlap presence, table handling.

### 5. Bug Reports (3+ bugs)
Document any retrieval, ingestion, or chunking issues found.

## Evaluation Criteria
| Criteria | Weight |
|---------|--------|
| Golden set comprehensiveness | 30% |
| Ingestion test coverage | 25% |
| Retrieval quality analysis depth | 25% |
| Bug reports quality | 20% |

> **Next Module**: [Module 4 — LLM Behavior & Quality Testing](../module-04-llm-testing/README.md)
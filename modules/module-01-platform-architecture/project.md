# Module 1: Project — Architecture Cheat Sheet for QA

## 🎯 Objective

Create a comprehensive **QA Architecture Cheat Sheet** — a one-page (or two-page) reference document that any QA engineer joining the project can use to quickly understand the system.

## ⏱️ Estimated Time: 3 hours

---

## Requirements

Your cheat sheet must include:

### 1. System Diagram
Draw the complete architecture showing all components and their connections:
- Frontend → Backend → Agent → MCP → KB → LLM
- Include all MCP Servers
- Show data sources
- Mark security checkpoints (AD, RBAC/ABAC)
- Mark observability touchpoints (Langfuse, Prometheus)

### 2. Component Quick Reference Table

| Component | Technology | Purpose | Key Endpoints/Interfaces | What QA Tests Here |
|-----------|-----------|---------|------------------------|-------------------|
| Frontend | React | | | |
| Backend | .NET (C#) | | | |
| Agent Runtime | .NET MCP SDK | | | |
| MCP Servers | | | | |
| LLM Gateway | | | | |
| vLLM | Llama 3 | | | |
| HuggingFace TEI | | | `/embed` | |
| PaddleOCR | | | `/ocr` | |
| Knowledge Base | | | | |
| Weaviate | | | | |
| Ingestion Pipeline | Python, Redis/RQ | | | |
| Langfuse | | | | |
| Prometheus/Grafana | | | | |

### 3. The "Where Did It Break?" Quick Guide

For each symptom, list the most likely component and how to check:

| Symptom | Check First | Check Second | How to Verify |
|---------|------------|-------------|---------------|
| Empty response | | | |
| Wrong answer (hallucination) | | | |
| Slow response (>30s) | | | |
| Access denied error | | | |
| Old data in response | | | |
| Response in wrong language | | | |
| System error (500) | | | |

### 4. Key URLs and Access Points

List all the URLs/endpoints a QA engineer needs daily:

| Tool/System | URL | Credentials/Access | Purpose |
|------------|-----|-------------------|---------|
| HR Assistant (UI) | | | |
| Finance Assistant (UI) | | | |
| Backend API (Swagger) | | | |
| Langfuse | | | |
| Grafana | | | |
| Weaviate Console | | | |
| Knowledge Base API | | | |

### 5. Glossary (top 15 terms)

List the 15 most important terms with one-line definitions relevant to QA work.

---

## Evaluation Criteria

| Criteria | Weight | Description |
|---------|--------|-------------|
| Completeness | 30% | All components and connections covered |
| Accuracy | 25% | Correct technology names, endpoints, descriptions |
| Usability | 25% | A new QA can use it on day 1 |
| Debugging Guide | 20% | The "Where Did It Break?" section is actionable |

## Deliverable

- Format: Markdown file or PDF
- Length: 2-4 pages maximum
- Must be usable as a daily reference, not a textbook

> **Next Module**: [Module 2 — Agent & MCP Testing](../module-02-agent-mcp-testing/README.md)
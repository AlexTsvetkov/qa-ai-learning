🧠 ROLE

You are a **Senior QA Architect and AI Systems Trainer** designing an **enterprise-level internal academy** for Senior Manual QA engineers.

Your audience:

* Experienced QA engineers
* Strong in classical testing
* **No prior experience with AI / LLM / RAG systems**

Your goal:
Train them to confidently test a **complex on-prem AI platform** with agents, LLMs, RAG pipelines, and enterprise integrations.

---

## 🏗️ PLATFORM CONTEXT (STRICT — DO NOT SIMPLIFY OR IGNORE)

The system is an enterprise AI ecosystem evolving in 3 stages:

### Applications:

* AI HR Assistant
* AI Finance Assistant
* AI Clerk

### Frontend / Backend:

* Frontend: React
* Backend: .NET (C#)

---

### 🤖 AGENT ARCHITECTURE

* Agent Runtime / MCP Host (.NET MCP SDK)
* Core business logic is executed by the agent
* Components:

    * Prompt Resolver
    * Trace collection
    * MCP Clients (multiple)

---

### 🔗 MCP (Model Context Protocol)

* MCP Clients inside Agent
* MCP Servers:

    * Retrieval tools
    * Query generator
    * External tools (client-provided)
* Agent orchestrates calls to MCP tools dynamically

---

### 🧠 LLM & MODEL ORCHESTRATION

* LLM Gateway (proxy)
* vLLM containers (Llama 3)
* Lightweight LLM
* HuggingFace TEI (embeddings)
* PaddleOCR (OCR pipeline)

Endpoints:

* /chat
* /embed
* /ocr

---

### 📚 KNOWLEDGE BASE (RAG)

#### Query Plane:

* Knowledge Base REST API
* Retrieval Adapter
* Policy / Access Filter (RBAC/ABAC)
* MCP Retrieval Server

#### Ingestion Plane:

* Ingestion API
* Job Orchestration
* Work Queue (Redis / RQ)
* Ingestion Workers (Python)
* OCR + Embeddings

#### Storage:

* Vector DB (Weaviate cluster)

---

### 📊 DATA SOURCES

* File Share (documents)
* Matter DB (MS SQL Server)
* Financial data (OLAP)
* Internal systems (custom data)

---

### 🔐 SECURITY

* Active Directory (authentication)
* RBAC / ABAC (document-level access control)

---

### 📈 OBSERVABILITY

* Langfuse:

    * prompt registry
    * versioning
    * traces
    * evaluations
* Prometheus (metrics)
* Grafana (dashboards)
* Logs (Loki or similar)

---

### ⚙️ PLATFORM CHARACTERISTICS

* On-prem deployment
* No external LLM APIs
* Multi-stage evolution:

    * Release 1: HR + basic RAG
    * Release 2: Finance + OLAP + monitoring
    * Release 3: Clerk + OCR + pipelines

---

## 🎯 YOUR TASK

Generate a **complete AI QA training academy**, including ALL of the following:

---

# 1. FULL TRAINING COURSE (4–8 weeks)

Structure:

* Modules
* Lessons
* Practical QA Exercises
* Realistic Test Cases
* Checklists
* Common Bugs / Failure Modes

Must cover:

* Agent-based systems testing
* MCP orchestration testing
* RAG / Knowledge Base testing
* Ingestion pipelines
* LLM behavior testing (hallucinations, prompt issues)
* RBAC / security testing
* Observability and debugging

---

# 2. LEARNING ROADMAP

Include:

* Phases (Foundation → Core → Advanced → Production QA)
* Skill progression
* Dependencies
* Estimated duration
* Entry / Exit criteria
* Typical QA mistakes at each stage

---

# 3. 14-DAY ONBOARDING PLAN

For a new Senior QA joining the project.

Include:

* Day-by-day plan
* What to learn
* What to test
* Hands-on tasks
* Checklists
* First bugs they are likely to find
* Where to look in logs / traces

---

# 4. QA TEST STRATEGY

Include:

### Test Layers:

* UI
* API
* Agent layer
* Data layer
* Infrastructure

### AI-Specific Testing:

* Hallucinations
* Prompt failures
* Retrieval errors
* Context leakage

### MCP Testing:

* Tool invocation
* Tool chaining
* Failure handling

### RAG Testing:

* Indexing
* Retrieval quality
* Access control

### Security Testing:

* RBAC / ABAC
* Data leakage

### Observability:

* Logs
* Metrics
* Traces

Also include:

* Risk matrix
* Critical scenarios
* Anti-patterns

---

# 5. ARCHITECTURE EXPLANATION FOR QA (SIMPLIFIED)

Explain in QA-friendly language:

### End-to-end data flow:

User → FE → BE → Agent → MCP → KB → LLM → Response

### Explain:

* Agent Runtime
* MCP Client / Server
* LLM Gateway
* Knowledge Base
* Ingestion pipeline

### Include:

* Where systems break
* What QA should test in each component
* Mental model for QA engineers

---

## ⚠️ IMPORTANT RULES

* Do NOT be generic
* Do NOT simplify architecture incorrectly
* Focus on **manual QA perspective**
* Avoid deep ML theory
* Focus on **real testing work**
* Include **realistic bugs and failure scenarios**
* Use **practical examples from this architecture**

---

## 📦 OUTPUT FORMAT

Use structured sections:

* ## Course
* ## Roadmap
* ## Onboarding
* ## Test Strategy
* ## Architecture for QA

Each section must be detailed and actionable.

---

## 🚀 GOAL

By the end of your output, a Senior Manual QA should:

* Understand the system
* Know how to test it
* Be ready to start working on the project
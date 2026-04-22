# QA Academy: Testing Enterprise AI Platform

[![Course Status](https://img.shields.io/badge/status-in%20progress-yellow)]()
[![Modules](https://img.shields.io/badge/modules-7-blue)]()
[![Duration](https://img.shields.io/badge/duration-6--8%20weeks-orange)]()
[![Languages](https://img.shields.io/badge/languages-EN%20%7C%20RU-green)]()

## 🎯 Academy Mission

Train **Senior Manual QA engineers** to confidently test a **complex on-prem enterprise AI platform** with agents, LLMs, RAG pipelines, and enterprise integrations.

By the end of this academy, a QA engineer will:
- **Understand** the complete system architecture (Agent → MCP → LLM → RAG → Response)
- **Know how to test** every component — from UI to vector database
- **Be ready** to start working on the project from day one

## 👥 Target Audience

- **Senior Manual QA engineers** with strong classical testing experience
- **No prior AI/LLM/RAG experience required** — we start from zero
- Engineers joining the AI platform project team

## 🏗️ Platform Under Test

An enterprise AI ecosystem with 3 applications (AI HR Assistant, AI Finance Assistant, AI Clerk) built on:

```
User → React FE → .NET Backend → Agent Runtime (MCP Host)
                                       ↓
                              MCP Clients → MCP Servers
                                       ↓
                    ┌──────────────────────────────────┐
                    │  LLM Gateway → vLLM (Llama 3)    │
                    │  Knowledge Base (Weaviate)        │
                    │  Ingestion Pipeline (OCR+Embed)   │
                    │  Langfuse (Observability)         │
                    └──────────────────────────────────┘
```

**Key technologies:** .NET MCP SDK, MCP Protocol, vLLM, HuggingFace TEI, PaddleOCR, Weaviate, Redis/RQ, Langfuse, Prometheus/Grafana, Active Directory, RBAC/ABAC

## 🗂️ Course Structure

### 🇬🇧 English (Primary)

| Module | Topic | Duration | Description |
|--------|-------|----------|-------------|
| [Module 1](modules/module-01-platform-architecture/README.md) | Platform Architecture & AI Fundamentals | Week 1 | System architecture, data flow, AI basics for QA |
| [Module 2](modules/module-02-agent-mcp-testing/README.md) | Agent & MCP Testing | Week 2 | Agent Runtime, MCP orchestration, tool invocation |
| [Module 3](modules/module-03-rag-knowledge-base/README.md) | RAG & Knowledge Base Testing | Week 3 | Retrieval quality, ingestion pipeline, Weaviate |
| [Module 4](modules/module-04-llm-testing/README.md) | LLM Behavior & Quality Testing | Week 4 | Hallucinations, prompt issues, response quality |
| [Module 5](modules/module-05-security-access-control/README.md) | Security & Access Control Testing | Week 5 | RBAC/ABAC, data leakage, context isolation |
| [Module 6](modules/module-06-observability-debugging/README.md) | Observability & Production QA | Week 6 | Langfuse traces, Prometheus, Grafana, debugging |
| [Module 7](modules/module-07-strategy-onboarding/README.md) | Test Strategy & Onboarding | Week 7-8 | Complete test strategy, 14-day onboarding, roadmap |

### 🇷🇺 Русская версия

| Модуль | Тема | Длительность | Описание |
|--------|------|-------------|----------|
| [Модуль 1](modules/module-01-platform-architecture_ru/README.md) | Архитектура платформы и основы AI | Неделя 1 | Архитектура системы, поток данных, основы AI для QA |
| [Модуль 2](modules/module-02-agent-mcp-testing_ru/README.md) | Тестирование агентов и MCP | Неделя 2 | Agent Runtime, MCP оркестрация, вызов инструментов |
| [Модуль 3](modules/module-03-rag-knowledge-base_ru/README.md) | Тестирование RAG и базы знаний | Неделя 3 | Качество retrieval, ingestion pipeline, Weaviate |
| [Модуль 4](modules/module-04-llm-testing_ru/README.md) | Тестирование поведения LLM | Неделя 4 | Галлюцинации, проблемы промптов, качество ответов |
| [Модуль 5](modules/module-05-security-access-control_ru/README.md) | Безопасность и контроль доступа | Неделя 5 | RBAC/ABAC, утечка данных, изоляция контекста |
| [Модуль 6](modules/module-06-observability-debugging_ru/README.md) | Наблюдаемость и Production QA | Неделя 6 | Langfuse трейсы, Prometheus, Grafana, отладка |
| [Модуль 7](modules/module-07-strategy-onboarding_ru/README.md) | Тест-стратегия и онбординг | Недели 7-8 | Тест-стратегия, 14-дневный онбординг, роадмэп |

### Each module contains:
- 📖 **Theory** — core concepts with platform-specific examples
- 🔬 **Labs** — hands-on exercises with realistic scenarios
- 📝 **Quiz** — self-assessment questions
- 🏆 **Project** — real-world testing tasks on the platform
- 📚 **Resources** — additional learning materials
- 🐛 **Common Bugs** — realistic failure scenarios you'll encounter

## 📋 Learning Roadmap

### Phase 1: Foundation (Weeks 1-2)
> Understand the system, learn AI basics, test basic flows

- Module 1: Platform Architecture & AI Fundamentals
- Module 2: Agent & MCP Testing
- **Exit criteria:** Can explain the full data flow, execute basic agent tests

### Phase 2: Core (Weeks 3-4)
> Test AI-specific components: RAG quality, LLM behavior

- Module 3: RAG & Knowledge Base Testing
- Module 4: LLM Behavior & Quality Testing
- **Exit criteria:** Can test retrieval quality, identify hallucinations, test ingestion

### Phase 3: Advanced (Weeks 5-6)
> Security, observability, production debugging

- Module 5: Security & Access Control Testing
- Module 6: Observability & Production QA
- **Exit criteria:** Can test RBAC/ABAC, use Langfuse for debugging, read traces

### Phase 4: Production QA (Weeks 7-8)
> Complete test strategy, ready for autonomous work

- Module 7: Test Strategy & Onboarding
- **Exit criteria:** Can write test plans, onboard new QA, execute full regression

## 📊 Progress Tracking

### Module 1: Platform Architecture & AI Fundamentals
- [ ] Theory completed
- [ ] Lab 1: Platform exploration & data flow mapping
- [ ] Lab 2: First AI queries to the platform
- [ ] Quiz passed
- [ ] Project completed

### Module 2: Agent & MCP Testing
- [ ] Theory completed
- [ ] Lab 1: Agent behavior testing
- [ ] Lab 2: MCP tool invocation testing
- [ ] Quiz passed
- [ ] Project completed

### Module 3: RAG & Knowledge Base Testing
- [ ] Theory completed
- [ ] Lab 1: Retrieval quality testing
- [ ] Lab 2: Ingestion pipeline testing
- [ ] Quiz passed
- [ ] Project completed

### Module 4: LLM Behavior & Quality Testing
- [ ] Theory completed
- [ ] Lab 1: Hallucination detection & prompt testing
- [ ] Lab 2: Response quality evaluation
- [ ] Quiz passed
- [ ] Project completed

### Module 5: Security & Access Control Testing
- [ ] Theory completed
- [ ] Lab 1: RBAC/ABAC testing
- [ ] Lab 2: Data leakage & context isolation testing
- [ ] Quiz passed
- [ ] Project completed

### Module 6: Observability & Production QA
- [ ] Theory completed
- [ ] Lab 1: Langfuse traces & debugging
- [ ] Lab 2: Prometheus/Grafana monitoring
- [ ] Quiz passed
- [ ] Project completed

### Module 7: Test Strategy & Onboarding
- [ ] Theory completed
- [ ] Lab 1: Test strategy creation
- [ ] Lab 2: 14-day onboarding simulation
- [ ] Quiz passed
- [ ] Final project completed

## 📄 License

This course is created for internal educational purposes.
# Module 1: Resources — Platform Architecture & AI Fundamentals

## 📚 Essential Reading

### Platform Technologies
- [Model Context Protocol (MCP) Specification](https://modelcontextprotocol.io/) — Official MCP documentation
- [vLLM Documentation](https://docs.vllm.ai/) — The LLM serving framework used in our platform
- [Weaviate Documentation](https://weaviate.io/developers/weaviate) — Our vector database
- [Langfuse Documentation](https://langfuse.com/docs) — Observability platform for LLM apps
- [HuggingFace TEI](https://huggingface.co/docs/text-embeddings-inference/) — Text embeddings inference

### AI Concepts for QA
- [What is RAG? (AWS)](https://aws.amazon.com/what-is/retrieval-augmented-generation/) — Clear explanation of RAG
- [LLM Hallucinations Explained](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence)) — Understanding AI hallucinations
- [Introduction to Embeddings](https://developers.google.com/machine-learning/crash-course/embeddings/video-lecture) — Google's beginner-friendly explanation

### Testing AI Systems
- [Testing LLM Applications (Langfuse Blog)](https://langfuse.com/blog) — Practical guides on LLM testing
- [Microsoft Responsible AI Guidelines](https://www.microsoft.com/en-us/ai/responsible-ai) — Framework for AI quality

## 🔧 Tools to Install/Bookmark

| Tool | Purpose | URL |
|------|---------|-----|
| Postman / Insomnia | API testing | https://www.postman.com/ |
| Browser DevTools | Frontend debugging | Built into Chrome/Firefox |
| Langfuse (project instance) | Trace inspection | Ask your team lead |
| Grafana (project instance) | Dashboard monitoring | Ask your team lead |
| Weaviate Console | Vector DB inspection | Ask your team lead |

## 📖 Glossary Quick Reference

| Term | One-Line Definition |
|------|-------------------|
| Agent | AI system that plans and uses tools to complete tasks |
| MCP | Protocol for agent-to-tool communication |
| RAG | Retrieve documents first, then generate AI response |
| Embedding | Numerical vector representing text meaning |
| Hallucination | AI generates plausible but wrong content |
| Vector DB | Database for semantic similarity search (Weaviate) |
| Ingestion | Process of adding documents to the knowledge base |
| Chunking | Splitting documents into smaller searchable pieces |
| Langfuse | Observability: traces, prompts, evaluations |
| RBAC/ABAC | Access control by role and document attributes |
| vLLM | Framework serving Llama 3 on GPU |
| On-prem | Deployed on company hardware, no cloud |

> **Next Module**: [Module 2 — Agent & MCP Testing](../module-02-agent-mcp-testing/README.md)
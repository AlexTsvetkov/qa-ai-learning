# Module 2: Resources — Agent & MCP Testing

## 📚 Essential Reading
- [MCP Specification](https://modelcontextprotocol.io/) — Full protocol documentation
- [MCP .NET SDK](https://github.com/modelcontextprotocol/csharp-sdk) — SDK used in our platform
- [Langfuse Tracing Docs](https://langfuse.com/docs/tracing) — Understanding traces and spans
- [AI Agent Testing Patterns](https://www.anthropic.com/research) — Research on agent behavior

## 🔧 Key Tools
| Tool | Purpose |
|------|---------|
| Langfuse | Trace inspection for agent behavior verification |
| Postman | Direct MCP Server API testing |
| Browser DevTools | Frontend request inspection |

## 📋 Test Case Templates
- Agent tool selection test: Question → Expected Tool → Trace Verification
- MCP failure test: Failure Condition → Expected Behavior → Recovery Check
- Prompt injection test: Attack Input → Expected Refusal → Trace Check

> **Next Module**: [Module 3 — RAG & Knowledge Base Testing](../module-03-rag-knowledge-base/README.md)
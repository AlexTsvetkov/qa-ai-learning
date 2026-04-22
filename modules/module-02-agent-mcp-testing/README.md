# Module 2: Agent & MCP Testing

## 📋 Module Overview

The Agent Runtime is the "brain" of the platform — it receives user questions, decides which tools to call, and orchestrates the response. This module teaches you how to test agent behavior, MCP tool invocation, tool chaining, and failure handling.

## 🎯 Learning Objectives

After completing this module, you will be able to:
- Test agent decision-making (correct tool selection for different questions)
- Verify MCP tool invocation (parameters, responses, error handling)
- Test tool chaining scenarios (multiple tools called in sequence)
- Identify agent failure modes (infinite loops, wrong tools, timeouts)
- Use Langfuse traces to debug agent behavior

## 📚 Contents

| Material | Description |
|----------|-------------|
| [📖 Theory](theory.md) | Agent architecture, MCP protocol, tool orchestration |
| [🔬 Lab 1](labs/lab-01-agent-behavior.md) | Testing agent decision-making and tool selection |
| [🔬 Lab 2](labs/lab-02-mcp-tool-testing.md) | MCP tool invocation and failure handling |
| [📝 Quiz](quiz.md) | Self-assessment questions |
| [🏆 Project](project.md) | Agent test suite creation |
| [📚 Resources](resources.md) | Additional materials |

## ⏱️ Estimated Time

- Theory: ~3 hours
- Labs: ~4 hours
- Project: ~3 hours
- **Total: ~10 hours (Week 2)**

## 🔗 Prerequisites

- Completed Module 1: Platform Architecture & AI Fundamentals
- Access to Langfuse for trace inspection
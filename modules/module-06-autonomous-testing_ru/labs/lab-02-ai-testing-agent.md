# Lab 2: Creating an AI Testing Agent

## 🎯 Objective
Build and run an AI testing agent that can autonomously explore a web application and find potential issues.

## ⏱️ Время: 3 часов

---

## Упражнение 1: Set Up a Test Application

Use a simple web application for the agent to test. Options:
- **TodoMVC**: https://todomvc.com/examples/react/
- **Local app**: Run a simple web app with Docker

```bash
# Option: Use a public demo app
# Or run a local todo app
docker run -p 3000:3000 getting-started
```

## Упражнение 2: Implement the AI Testing Agent

Based on the `AITestingAgent` class from theory:
1. Implement all methods
2. Add console error capturing
3. Add screenshot capture on each step
4. Improve the evaluation logic

Save to `exercises/ai_agent.py`.

## Упражнение 3: Run and Analyze Results

1. Run the agent against the test application for 20 steps
2. Review the findings
3. Create a report documenting:
   - Pages visited
   - Actions taken
   - Issues found (real bugs vs false positives)
   - Agent effectiveness assessment

Save report to `exercises/agent_report.md`.

## Упражнение 4: Improve the Agent

Based on your observations, improve the agent:
- Better action decisions
- Smarter bug detection
- More structured exploration strategy

---

## ✅ Completion Checklist
- [ ] Test application running
- [ ] AI agent implemented and running
- [ ] Agent findings reviewed and documented
- [ ] Agent improvements made


> 🌐 [English version](../module-06-autonomous-testing/labs/lab-02-ai-testing-agent.md)

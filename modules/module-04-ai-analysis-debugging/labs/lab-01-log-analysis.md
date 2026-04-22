# Lab 1: Log Analysis with AI

## 🎯 Objective
Practice analyzing application logs using AI to identify errors, patterns, and root causes.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Web Server Log Analysis

Analyze the following simulated Nginx + application logs:

```
[2024-03-15 10:23:01] INFO  [web] Request: GET /api/products - 200 (45ms)
[2024-03-15 10:23:02] INFO  [web] Request: POST /api/cart/add - 201 (120ms)
[2024-03-15 10:23:05] WARN  [db] Connection pool: 45/50 connections used
[2024-03-15 10:23:06] INFO  [web] Request: GET /api/cart - 200 (89ms)
[2024-03-15 10:23:08] WARN  [db] Connection pool: 48/50 connections used
[2024-03-15 10:23:10] ERROR [db] Connection pool exhausted, waiting for available connection
[2024-03-15 10:23:10] INFO  [web] Request: POST /api/checkout - timeout
[2024-03-15 10:23:11] ERROR [web] Request failed: POST /api/checkout - 504 Gateway Timeout (30002ms)
[2024-03-15 10:23:11] ERROR [payment] Failed to process payment: connection timeout
[2024-03-15 10:23:12] WARN  [db] Connection pool: 50/50 connections used
[2024-03-15 10:23:15] ERROR [web] Request failed: GET /api/products - 504 Gateway Timeout (30001ms)
[2024-03-15 10:23:15] INFO  [db] Releasing 5 idle connections
[2024-03-15 10:23:16] INFO  [web] Request: GET /api/products - 200 (2345ms)
[2024-03-15 10:23:18] WARN  [db] Slow query detected: SELECT * FROM products WHERE... (4521ms)
```

**Task**: Ask AI to analyze these logs and identify:
1. Timeline of the incident
2. Root cause
3. Impact on users
4. Recommended fixes

Save your analysis to `exercises/lab01_log_analysis.md`.

## Exercise 2: Multi-Service Log Correlation

Create a scenario with logs from 3 different services and ask AI to correlate events across them. Use the prompt from theory section 4.1.

## Exercise 3: Programmatic Log Analysis

Write a Python script that:
1. Reads a log file
2. Sends relevant portions to AI for analysis
3. Outputs a structured incident report

Save to `exercises/lab01_log_analyzer.py`.

---

## ✅ Completion Checklist
- [ ] Web server log analysis completed
- [ ] Multi-service correlation exercise done
- [ ] Python log analyzer script created

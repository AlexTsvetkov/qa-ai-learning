# Lab 2: Prometheus/Grafana Monitoring

## 🎯 Objective
Use Grafana dashboards and Prometheus metrics to monitor platform health during testing.

## ⏱️ Estimated Time: 2 hours

---

## Exercise 1: Dashboard Exploration (30 min)

Open Grafana and document all available dashboards:

| Dashboard | Key Panels | Useful for QA? |
|-----------|-----------|----------------|
| | | |
| | | |

Record current baseline metrics:

| Metric | Current Value | Normal Range |
|--------|-------------|-------------|
| LLM p95 latency | | |
| Error rate | | |
| GPU memory % | | |
| KB search p95 latency | | |
| Ingestion queue depth | | |

---

## Exercise 2: Load Test Monitoring (45 min)

Send 20 requests over 5 minutes while watching Grafana:

| Minute | Requests Sent | Observed Latency | Error Rate | GPU Memory | Notes |
|--------|-------------|-----------------|------------|------------|-------|
| 0-1 | | | | | |
| 1-2 | | | | | |
| 2-3 | | | | | |
| 3-4 | | | | | |
| 4-5 | | | | | |

**Did any metric exceed the alert threshold?**

---

## Exercise 3: Anomaly Detection (30 min)

Look at historical data (last 24h or 7d) and identify:
1. Any latency spikes — when did they occur? Correlate with deployments/events.
2. Any error rate increases
3. Any unusual patterns in GPU memory or queue depth

---

## Exercise 4: Create a QA Monitoring Checklist (15 min)

Based on your observations, create a daily monitoring checklist:

```
Daily QA Monitoring (5 min):
- [ ] Check Grafana: LLM latency within SLA
- [ ] Check Grafana: Error rate < threshold
- [ ] Check Grafana: GPU memory < 90%
- [ ] Check Langfuse: No unusual trace patterns
- [ ] Check: Ingestion queue not backed up
```

---

## ✅ Completion Checklist
- [ ] Explored all Grafana dashboards
- [ ] Established baseline metrics
- [ ] Completed load test with monitoring
- [ ] Identified historical anomalies
- [ ] Created daily monitoring checklist

> **Next**: [Quiz](../quiz.md)
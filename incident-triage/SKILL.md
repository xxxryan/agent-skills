---
name: incident-triage
description: Triage a production incident from symptoms, logs, metrics, and recent deploy info. Produces a hypothesis tree, the fastest checks, likely root cause, and safe mitigations. Use when the user reports outages, latency spikes, error rate increases, stuck jobs, or resource exhaustion.
compatibility: Works with pasted logs/graphs summaries; best if the agent can inspect repo history and config changes.
metadata:
  short-description: Production incident triage playbook
allowed-tools: Read
---

# Incident Triage

## What you need (ask only if missing)
- What changed recently (deploy, config, dependency)
- Primary symptoms (error codes, latency, saturation, partial outage)
- Scope (one region, one service, all traffic)
- Example log lines / request IDs

## Workflow
1. Restate symptoms as facts
2. Build a hypothesis tree
   - Deploy regression
   - Downstream dependency failure
   - Resource saturation (CPU/mem/conn pool)
   - Data issue (bad row, migration)
   - Thundering herd / retry storms
3. Fast checks (order by speed + confidence)
4. Mitigation options (safe first)
   - rollback, scale out, disable feature flag, reduce concurrency, tighten retries
5. Permanent fix outline + prevention
   - tests, alerts, rate limits, circuit breaker, dashboards

## Output format
### Situation summary
...

### Hypotheses (ranked)
1. ...
2. ...

### Fast checks
- ...

### Mitigations (safe first)
- ...

### Likely root cause
...

### Follow-up actions
- ...

---
name: go-concurrency-audit
description: "Audit Go code for concurrency bugs: data races, goroutine leaks, deadlocks, channel misuse, and context leaks. Use when the user mentions goroutines, worker pools, timeouts, stuck requests, high CPU, or race detector failures."
compatibility: Best with local repo access and ability to run `go test -race`. Can also operate on pasted code/diff.
metadata:
  short-description: Find Go concurrency bugs
allowed-tools: "Bash(go:*) Bash(git:*) Read"
---

# Go Concurrency Audit

## What to look for
- Data races (shared state without sync)
- Goroutine leaks (missing cancel/close, infinite loops)
- Deadlocks (lock ordering, blocking sends/receives)
- Channel ownership confusion (who closes)
- Unbounded concurrency (fan-out without limits)
- Context misuse (not passed, not honored, missing deadlines)

## Workflow
1. Identify concurrency surfaces
   - goroutines, channels, mutexes, atomics, waitgroups, errgroup, pools

2. Static audit (from code)
   - Map lifecycle: who creates, who cancels, who waits
   - Ensure every goroutine has an exit path
   - Check select loops: default cases, timers, tickers are stopped
   - Verify lock ordering and minimal critical sections

3. Dynamic checks (if available)
   - Run targeted tests with race detector:
     - `go test ./... -race`
   - Suggest reproducer / stress tests if missing

## Output format
### Findings (with severity)
- **Blocker:** ...
- **High:** ...
- **Medium:** ...
- **Low:** ...

### Suggested fixes
- ...

### Recommended tests
- ...

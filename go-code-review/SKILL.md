---
name: go-code-review
description: Perform a senior Go code review on a diff or files, focusing on correctness, idiomatic Go, error handling, maintainability, and performance. Use when the user asks for review, refactor feedback, or PR comments for Go services.
compatibility: Works with diffs or local repo access. Best with gofmt/go test available.
metadata:
  short-description: Senior Go PR review
allowed-tools: Bash(git:*) Bash(go:*) Read
---

# Go Code Review

## Review principles
- Correctness > clarity > performance
- Prefer small, explicit abstractions
- Avoid hidden coupling across packages
- Ensure errors are actionable and wrapped

## Checklist (prioritized)
### Correctness & API behavior
- Context propagation, cancellation, timeouts
- Input validation and boundary conditions
- Error semantics (sentinel vs typed vs wrapped)
- Idempotency for write endpoints if relevant

### Concurrency & resources
- Goroutine lifecycle: no leaks; bounded fan-out
- Channels: ownership, close discipline, backpressure
- Locks: avoid contention; avoid deadlocks
- Use `errgroup` where appropriate

### Go idioms & style
- gofmt, naming, package structure
- Avoid needless pointers; avoid interface{} when generics / concrete types suffice
- Minimize exported surface

### Performance
- Avoid per-request allocations in hot paths
- Preallocate slices/maps when size known
- Avoid converting between []byte/string unnecessarily
- DB queries: N+1, missing indexes, scanning errors

### Observability & ops
- Structured logs; include request ids
- Metrics for errors/latency where important
- Trace propagation (if applicable)

## Workflow
1. Determine input: diff, files, or PR context.
2. Produce review comments grouped by severity:
   - **Blockers**
   - **Important**
   - **Nit / style**
3. For each comment:
   - Quote the relevant code snippet (short)
   - Explain impact
   - Provide a concrete fix suggestion

## Output format
- Use bullet points, grouped by severity.
- Include small patch suggestions when helpful.

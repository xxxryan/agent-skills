---
name: generate-go-tests
description: Generate table-driven Go tests (unit or integration) for given functions, handlers, or packages. Use when the user asks for tests, coverage, regression prevention, or test scaffolding with mocks/fakes.
compatibility: Works with pasted code/diff; best with repo access and `go test` runnable.
metadata:
  short-description: Generate Go tests quickly
allowed-tools: Bash(go:*) Bash(git:*) Read
---

# Generate Go Tests

## Goals
- Table-driven tests with clear cases
- Deterministic and parallel-safe tests
- Minimal mocking; prefer fakes where possible
- Include edge cases and regression cases

## Workflow
1. Identify what to test
   - Pure functions: input/output cases
   - Services: dependency fakes/mocks
   - HTTP/gRPC handlers: request/response + status codes

2. Write test structure
   - `TestXxx` with `[]struct{ name ... }`
   - Use subtests: `t.Run(tc.name, func(t *testing.T){ ... })`
   - Add `t.Parallel()` only if safe (no shared mutable state)

3. Coverage guidance
   - Include: happy path, validation errors, dependency failure, timeout/cancel
   - For idempotency: repeated requests, dedup key behavior

## Output
- Provide the full `_test.go` file content.
- If dependencies exist, provide minimal fake implementations inline or in a small helper.

## Notes
- Don’t invent functions/types not present. If something is missing, note it and propose the smallest change needed to make testing feasible.

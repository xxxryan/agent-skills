---
name: draft-commit
description: Draft conventional commit messages from brief change summaries; use when asked to write, format, or suggest Conventional Commits (type/scope/subject) or when converting a short summary of changes into a commit message.
---

# Draft Commit

## Goal
Draft a Conventional Commit message from a short summary of changes, with minimal back-and-forth.

## Workflow
1. Parse the summary for intent and impacted area.
2. Choose a commit type based on intent (use the mapping below).
3. Propose a concise subject in imperative mood.
4. Add scope if it is clear and helpful.
5. Include a body or breaking-change footer only when needed.
6. If essential details are missing or ambiguous, ask a single clarifying question.

## Type Selection (default mapping)
- feat: add a user-facing feature or capability
- fix: correct a bug or regression
- refactor: restructure code without changing behavior
- perf: improve performance
- docs: documentation-only changes
- test: add or adjust tests
- build: build system or dependencies
- ci: CI configuration or scripts
- chore: maintenance tasks, tooling, or non-user-facing changes
- style: formatting or stylistic changes only

If multiple types apply, pick the most user-visible or highest impact type.

## Scope Guidance
- Use scope only when a clear component, package, or area is mentioned (e.g., api, ui, auth).
- Omit scope if uncertain rather than guessing.

## Subject Rules
- Use imperative mood: "add", "fix", "refactor".
- Keep it concise (<= 72 characters when possible).
- Avoid ending punctuation.

## Body and Footers
- Add a body if it clarifies non-obvious changes or rationale.
- Add breaking change footer when the summary indicates a breaking change:
  BREAKING CHANGE: <short explanation>
- If the summary implies breaking behavior but is unclear, ask a clarifying question.

## Output Format
Return a single best commit message in one of these forms:
- type(scope): subject
- type: subject

Include body/footers only if needed.

## Examples
Summary: "Add user profile upload endpoint and validation"
Output: feat(api): add profile upload endpoint

Summary: "Fix null pointer in invoice renderer"
Output: fix: handle null invoice renderer input

Summary: "Rename Plan to Subscription across UI and API (breaking)"
Output:
refactor: rename Plan to Subscription

BREAKING CHANGE: rename Plan to Subscription in API and UI

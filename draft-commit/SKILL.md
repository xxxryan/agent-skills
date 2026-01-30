---
name: draft-commit
description: Auto-detect code changes and draft Conventional Commit messages. Use when asked to generate, write, or suggest commit messages, or when the user wants to commit staged/unstaged changes.
---

# Draft Commit

## Goal
Automatically detect code changes via git and draft a Conventional Commit message following the [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) specification.

## Workflow

### Step 1: Detect Changes (Token-Efficient)

Use a **two-phase approach** to minimize token consumption:

#### Phase 1: Overview (always run, low token cost)
```bash
git status --short
git diff --staged --stat
git diff --stat
```
This gives you:
- List of changed files
- Lines added/removed per file
- Whether changes are staged or unstaged

#### Phase 2: Targeted Diff (only when needed)
**Skip full diff if** the stat output clearly indicates:
- Single file with obvious purpose (e.g., `README.md` → docs)
- File path reveals intent (e.g., `test_*.py` → test, `.github/workflows/*` → ci)
- Small change count (<10 lines) with clear filename

**Read targeted diff if** you need more context:
```bash
# Read specific file(s) only, not entire diff
git diff --staged -- path/to/unclear/file.go

# For large changes, limit output
git diff --staged -- path/to/file.go | head -100
```

#### Token Budget Guidelines
| Change Size | Strategy |
|-------------|----------|
| 1-3 files, <50 lines | Stat usually sufficient |
| 3-10 files | Read diff for unclear files only |
| 10+ files | Read stat + sample 2-3 key files |
| 50+ files (refactor) | Stat only + ask user for intent |

### Step 2: Analyze Changes
From the diff output, identify:
- **Files changed**: Which files/directories are modified
- **Change type**: New code, bug fix, refactor, docs, tests, etc.
- **Impact area**: Component, package, or module affected (for scope)
- **Breaking changes**: API changes, renamed exports, removed functions

### Step 3: Select Commit Type
Choose the most appropriate type based on actual changes:

| Type | When to Use | SemVer Impact |
|------|-------------|---------------|
| `feat` | New user-facing feature or capability | MINOR |
| `fix` | Bug fix, error correction | PATCH |
| `refactor` | Code restructure without behavior change | - |
| `perf` | Performance improvement | PATCH |
| `docs` | Documentation only (README, comments, JSDoc) | - |
| `test` | Add/update tests, no production code change | - |
| `build` | Build system, dependencies (package.json, go.mod) | - |
| `ci` | CI/CD configuration (.github/workflows, Jenkinsfile) | - |
| `chore` | Maintenance, tooling, non-user-facing tasks | - |
| `style` | Formatting, whitespace, no logic change | - |
| `revert` | Revert a previous commit | varies |

**Priority rule**: If changes span multiple types, choose the most user-visible or highest impact type.

### Step 4: Determine Scope
- Derive scope from the changed files' directory or module name
- Common scopes: `api`, `ui`, `auth`, `db`, `cli`, `config`, `core`
- **Omit scope** if changes span multiple unrelated areas
- **Omit scope** if uncertain—don't guess

### Step 5: Compose Message

#### Format (per Conventional Commits spec)
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

#### Subject Line Rules
- Use **imperative mood**: "add", "fix", "update" (not "added", "fixes")
- Start with lowercase after the colon
- Max 72 characters (hard limit: 100)
- No period at the end
- Describe **what** the change does, not **how**

#### Body Guidelines (optional)
Include body when:
- The "why" isn't obvious from the subject
- Multiple related changes need explanation
- Context helps future readers

Format:
- Blank line after subject
- Wrap at 72 characters
- Explain motivation, contrast with previous behavior

#### Footer Guidelines
- `BREAKING CHANGE: <description>` — Required for breaking changes
- `Refs: #<issue>` — Link to related issues
- `Reviewed-by: <name>` — Attribution
- `Co-authored-by: name <email>` — Multiple authors

### Step 6: Handle Breaking Changes
If the diff shows:
- Removed public API/function
- Renamed exported symbols
- Changed function signatures
- Removed configuration options

Then either:
1. Add `!` after type/scope: `feat(api)!: remove legacy endpoint`
2. Or add footer: `BREAKING CHANGE: removed /v1/users endpoint`

### Step 7: Output
Present the commit message in a code block:
```
<generated commit message>
```

If user wants to commit immediately, offer to run:
```bash
git add -A && git commit -m "<message>"
```

## Examples

### Example 1: Simple Feature
**Diff shows**: New file `src/api/upload.go` with upload handler

```
feat(api): add file upload endpoint
```

### Example 2: Bug Fix with Context
**Diff shows**: Null check added in `invoice.go`

```
fix(billing): handle nil invoice in renderer

Previously, passing a nil invoice would cause a panic.
Now returns an empty string with a warning log.
```

### Example 3: Breaking Change
**Diff shows**: Renamed `Plan` struct to `Subscription` across multiple files

```
refactor!: rename Plan to Subscription

BREAKING CHANGE: Plan type renamed to Subscription in public API.
Update all usages: Plan → Subscription, PlanID → SubscriptionID.

Refs: #142
```

### Example 4: Multiple File Types
**Diff shows**: New test file + updated handler

```
feat(auth): add OAuth2 callback handler

- Implement token exchange flow
- Add state parameter validation

Co-authored-by: Jane <jane@example.com>
```

### Example 5: Docs Only
**Diff shows**: Only README.md changed

```
docs: update installation instructions for v2
```

## Edge Cases

### No Changes Detected
If `git status` shows no changes:
> "No changes detected. Stage your changes with `git add` first, or check if you're in the correct directory."

### Mixed Staged/Unstaged
Ask user:
> "Found both staged and unstaged changes. Should I generate a message for:
> 1. Staged changes only
> 2. All changes (will stage unstaged files)"

### Ambiguous Intent
If the change purpose is unclear, ask ONE clarifying question:
> "The diff shows changes to `config.yaml` and `main.go`. Is this a feature, fix, or configuration update?"

### Large Change Set (50+ files)
Do NOT read full diff. Instead:
1. Use `git diff --stat` output only
2. Identify the pattern (rename, format, dependency update, etc.)
3. Ask user to confirm intent:
> "Detected 87 files changed. The pattern suggests a large refactor/rename. Please briefly describe the change purpose so I can draft an accurate commit message."

## Reference
- [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)
- [Semantic Versioning](https://semver.org/)

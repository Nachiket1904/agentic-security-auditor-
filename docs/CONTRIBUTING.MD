# Contributing Guide

## 1. Purpose

This document defines how contributors work on Agentic Security Auditor.

The goal is to maintain:

* Clean code
* Clear ownership
* Small changes
* Reviewable pull requests
* Reliable `main`
* Shared understanding of the architecture

---

# 2. Branching Strategy

The `main` branch should remain stable.

Feature work should use branches.

Recommended naming:

```text
feature/<name>
fix/<name>
refactor/<name>
docs/<name>
test/<name>
chore/<name>
```

Examples:

```text
feature/semgrep-scanner
feature/analyst-agent
feature/fixer-agent
feature/validation-engine
feature/audit-interface
fix/scanner-timeout
docs/update-architecture
```

---

# 3. Issue First

Every meaningful change should have a GitHub Issue.

Example:

```text
Issue #12
Integrate Semgrep scanner
```

The issue should define:

* Problem
* Goal
* Expected behavior
* Acceptance criteria

---

# 4. Development Flow

```text
GitHub Issue
     ↓
Create Branch
     ↓
Implement
     ↓
Write Tests
     ↓
Run Tests
     ↓
Commit
     ↓
Push
     ↓
Open Pull Request
     ↓
Code Review
     ↓
Merge
```

---

# 5. Commit Convention

Use clear commit messages.

Recommended format:

```text
type: description
```

Examples:

```text
feat: add semgrep scanner
feat: implement analyst agent
feat: add vulnerability finding model

fix: handle scanner timeout
fix: validate uploaded file type

test: add scanner integration tests

docs: update architecture

refactor: normalize scanner interface

chore: configure CI
```

---

# 6. Pull Requests

Every pull request should explain:

### What changed?

Briefly describe the implementation.

### Why?

Explain the problem being solved.

### Testing

Explain which tests were run.

### Security considerations

Mention security implications when applicable.

### Screenshots

Include screenshots for meaningful UI changes.

---

# 7. Pull Request Checklist

Before requesting review:

* [ ] Code is formatted
* [ ] Tests pass
* [ ] New behavior has tests
* [ ] No secrets are committed
* [ ] Documentation is updated if required
* [ ] Error handling is included
* [ ] Security implications were considered
* [ ] PR is focused on one logical change

---

# 8. Code Ownership

The project should use feature ownership without creating isolated silos.

Initial areas may be divided as follows.

## Security / AI / Backend

Responsibilities may include:

* Scanner integrations
* Finding normalization
* LangGraph
* Analyst
* Fixer
* Validator
* Backend services

## Frontend / Product

Responsibilities may include:

* Streamlit interface
* User workflow
* Findings visualization
* Diff presentation
* API integration
* UX

## Shared

Both contributors participate in:

* Architecture
* Testing
* Documentation
* GitHub integration
* Code review
* Product decisions

Ownership can change as the project evolves.

---

# 9. Code Review Principles

Review code for:

### Correctness

Does it work?

### Security

Could it introduce a vulnerability?

### Maintainability

Can another contributor understand it?

### Testing

Is important behavior covered?

### Architecture

Does it respect project boundaries?

### Simplicity

Is the implementation more complicated than necessary?

---

# 10. Main Branch Rules

`main` should not receive unreviewed feature commits.

Preferred flow:

```text
feature branch
      ↓
Pull Request
      ↓
Review
      ↓
CI
      ↓
Merge
```

---

# 11. Definition of Done

A feature is considered complete when:

* The implementation works.
* Tests cover important behavior.
* Error cases are considered.
* Security implications are reviewed.
* Documentation is updated where necessary.
* CI passes.
* Pull request has been reviewed.
* The feature integrates cleanly with the existing architecture.

---

# 12. AI Coding Tools

AI coding assistants may be used.

Examples include:

* Claude
* Antigravity
* Other coding assistants

However, contributors are responsible for all code committed to the repository.

AI-generated code must be:

* Reviewed
* Understood
* Tested
* Security-reviewed where relevant

The repository should never contain code that the contributor cannot explain.

---

# 13. Architectural Changes

Large architectural changes should be discussed before implementation.

Examples:

* Changing the database
* Replacing the orchestration framework
* Adding a major infrastructure dependency
* Changing the execution model
* Changing security boundaries

Create an issue or design document before implementing major changes.

---

# 14. Collaboration Principle

The goal is not merely to divide the code.

The goal is to build the product together while maintaining clear ownership.

Each contributor should understand the overall architecture even when they are primarily responsible for a specific component.

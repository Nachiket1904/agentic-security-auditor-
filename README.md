# Agentic Security Auditor

> **From vulnerability detection to validated remediation.**

An agentic application-security platform that combines deterministic SAST tools with AI-powered analysis and remediation to help developers move from **security finding → understanding → fix → validation**.

---

## Overview

Security scanners are good at finding vulnerabilities.

The problem begins after the finding.

Developers still need to understand the vulnerability, determine its real risk, implement a fix, test the change, and verify that the vulnerability has actually been resolved.

**Agentic Security Auditor** is designed to close that remediation loop.

The system combines:

* Deterministic security scanning
* AI-assisted security analysis
* AI-generated remediation
* Automated validation
* Developer-friendly reports and code diffs
* Future GitHub pull-request integration

The core principle is:

> **AI reasons about verified security findings rather than inventing vulnerabilities.**

---

## The Problem

Traditional security tooling often produces a list of findings and leaves remediation to the development team.

A typical workflow is:

```text
Run Scanner
     ↓
Review Findings
     ↓
Understand Vulnerability
     ↓
Research Remediation
     ↓
Write Fix
     ↓
Run Tests
     ↓
Run Scanner Again
     ↓
Review Result
```

This creates several challenges.

### Alert overload

Large codebases can generate many security findings, making prioritization difficult.

### Manual remediation

Developers must manually understand and fix each vulnerability.

### Limited security context

Raw scanner output does not always explain exploitability, impact, or remediation in developer-friendly language.

### Validation burden

A proposed fix must still be tested and rescanned before it can be trusted.

---

# The Solution

Agentic Security Auditor introduces an agentic remediation workflow:

```text
                    Source Code
                        │
                        ▼
                 ┌─────────────┐
                 │   Scanner   │
                 │ Semgrep /   │
                 │   Bandit    │
                 └──────┬──────┘
                        │
                  Verified Finding
                        │
                        ▼
                 ┌─────────────┐
                 │   Analyst   │
                 │ Risk +      │
                 │ Explanation │
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │    Fixer    │
                 │ Candidate   │
                 │ Remediation │
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │  Validator  │
                 │ Tests +     │
                 │ Re-scan     │
                 └──────┬──────┘
                        │
                 ┌──────┴──────┐
                 │             │
               PASS           FAIL
                 │             │
                 ▼             ▼
              Report       Retry / Reject
                 │
                 ▼
             Code Diff
```

The goal is not simply to generate code.

The goal is to generate a **validated remediation candidate**.

---

# Key Features

## Current MVP

* Python source-code auditing
* File upload
* Code-snippet auditing
* Semgrep integration
* Bandit integration
* Normalized vulnerability findings
* AI-powered vulnerability analysis
* AI-generated remediation
* Before/after code comparison
* Patch/diff generation
* Automated validation
* Security audit report
* LangGraph-based workflow orchestration
* FastAPI backend
* Streamlit MVP interface

---

## Planned Features

### Repository auditing

Connect a GitHub repository and scan an entire codebase.

### Automated pull requests

Generate a validated remediation branch and open a GitHub pull request.

### Continuous security

Monitor connected repositories and identify newly introduced vulnerabilities.

### Multi-language support

Expand beyond Python to languages such as:

* JavaScript / TypeScript
* Go
* Java
* Additional languages based on demand

### Team capabilities

* Organizations
* Projects
* Team members
* Role-based access
* Vulnerability history
* Audit logs
* Security dashboards

---

# Architecture

The system is designed around a separation between deterministic security tooling and probabilistic AI reasoning.

```text
                    User
                     │
                     ▼
                Streamlit UI
                     │
                     ▼
                  FastAPI
                     │
                     ▼
               LangGraph
               Orchestrator
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
     Scanner      Analyst        Fixer
        │            │            │
   Semgrep /      LLM API       LLM API
     Bandit
        │            │            │
        └────────────┼────────────┘
                     │
                     ▼
                  Validator
                     │
              ┌──────┴──────┐
              ▼             ▼
           Re-scan         Tests
              │             │
              └──────┬──────┘
                     ▼
              Validation Result
                     │
                     ▼
              Report + Diff
```

See [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for the detailed architecture.

---

# Why This Approach?

Security remediation requires a different approach from simply asking an LLM to inspect source code.

The system therefore separates responsibilities.

### Deterministic security tools

Semgrep and Bandit provide concrete scanner evidence.

### AI reasoning

The Analyst interprets the evidence and provides security context.

### AI remediation

The Fixer proposes a targeted code change.

### Automated validation

The Validator checks whether the proposed change actually resolves the original finding and preserves expected behavior.

This creates:

```text
Deterministic Evidence
        +
AI Reasoning
        +
Automated Validation
        =
Grounded Security Remediation
```

---

# Security Principles

Security is a first-class requirement of the system.

### No blind AI trust

An AI-generated patch is treated as a candidate, not as automatically correct.

### Scanner grounding

The AI workflow should be grounded in findings produced by deterministic security tooling.

### Validation before recommendation

Generated fixes should be tested and rescanned before being marked as validated.

### Minimal changes

The remediation process should avoid unrelated modifications to application code.

### Human review

Future GitHub integrations should create reviewable pull requests rather than automatically merging changes.

### Isolated execution

Untrusted source code must not be executed directly inside the main application environment.

---

# Technology Stack

| Layer                      | Technology                     |
| -------------------------- | ------------------------------ |
| Language                   | Python                         |
| API                        | FastAPI                        |
| Agent orchestration        | LangGraph                      |
| AI                         | Anthropic API                  |
| SAST                       | Semgrep                        |
| Python security analysis   | Bandit                         |
| MVP frontend               | Streamlit                      |
| Version control            | Git + GitHub                   |
| Testing                    | Pytest                         |
| CI/CD                      | GitHub Actions                 |
| Future database            | PostgreSQL                     |
| Future background jobs     | Redis + worker architecture    |
| Future execution isolation | Containers / sandboxed workers |

---

# Project Structure

```text
agentic-security-auditor/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
├── pyproject.toml
│
├── docs/
│   ├── PROJECT.md
│   ├── ARCHITECTURE.md
│   ├── ROADMAP.md
│   ├── SECURITY.md
│   └── CONTRIBUTING.md
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── agents/
│   │   ├── graph/
│   │   ├── scanner/
│   │   ├── validator/
│   │   ├── models/
│   │   ├── services/
│   │   └── config/
│   │
│   └── tests/
│
├── frontend/
│
├── security/
│   ├── rules/
│   └── test_cases/
│
├── scripts/
│
└── .github/
    ├── workflows/
    ├── ISSUE_TEMPLATE/
    └── pull_request_template.md
```

---

# Development Workflow

All feature development should use branches and pull requests.

```text
Issue
  ↓
Feature Branch
  ↓
Implementation
  ↓
Tests
  ↓
Pull Request
  ↓
Code Review
  ↓
Merge
```

The `main` branch should remain stable.

---

# Roadmap

## Phase 1 — Foundation

* Repository setup
* Architecture
* Development environment
* CI
* Core data models

## Phase 2 — Security MVP

* Semgrep integration
* Bandit integration
* Finding normalization
* Analyst agent
* Fixer agent
* LangGraph workflow
* Validation engine

## Phase 3 — Developer Experience

* File upload
* Code input
* Finding dashboard
* Security explanations
* Code diffs
* Validation results

## Phase 4 — GitHub Integration

```text
GitHub Repository
       ↓
Scan
       ↓
Analyze
       ↓
Fix
       ↓
Validate
       ↓
Pull Request
```

## Phase 5 — Productization

* Authentication
* Organizations
* Team management
* Project management
* Audit history
* Usage tracking
* Notifications

## Phase 6 — Scale

* Background workers
* Repository-level scanning
* Multi-language support
* Continuous security monitoring
* Enterprise integrations

---

# Project Goals

The project has three goals.

### 1. Technical

Build a reliable agentic security-remediation workflow using deterministic security tooling, AI reasoning, orchestration, and automated validation.

### 2. Developer

Reduce the effort required to move from a security finding to a validated remediation.

### 3. Product

Create a foundation that can eventually become a developer-security product rather than remaining only a hackathon prototype.

---

# Success Metrics

We will measure:

* Vulnerabilities detected
* False-positive rate
* Fix generation success rate
* Fix validation success rate
* Test pass rate
* Re-scan pass rate
* Average remediation time
* Developer intervention required

The long-term north-star metric is:

> **Percentage of verified security findings that reach a validated remediation with minimal developer intervention.**

---

# Project Status

**Status:** Architecture / MVP Development

**Current milestone:** Project Foundation

**Primary language:** Python

---

# Contributing

See [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md).

---

# Security

See [`docs/SECURITY.md`](docs/SECURITY.md).

---

# License

See [`LICENSE`](LICENSE).

---

## Core Principle

> **Detection is a list. Remediation is the product.**

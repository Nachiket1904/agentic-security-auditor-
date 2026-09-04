# Agentic Security Auditor — Roadmap

## Vision

Build a developer-security platform that moves from:

**Detection → Analysis → Remediation → Validation → Pull Request**

---

# Phase 0 — Foundation

**Status: Current**

### Goals

Establish the engineering foundation.

### Tasks

* [ ] Initialize repository
* [ ] Define architecture
* [ ] Define project specification
* [ ] Define security principles
* [ ] Configure Python environment
* [ ] Configure dependency management
* [ ] Configure environment variables
* [ ] Configure GitHub workflow
* [ ] Configure CI
* [ ] Create initial tests

---

# Phase 1 — Scanner MVP

### Goals

Build reliable deterministic vulnerability detection.

### Tasks

* [ ] Integrate Semgrep
* [ ] Integrate Bandit
* [ ] Define scanner interface
* [ ] Define normalized Finding model
* [ ] Parse scanner output
* [ ] Handle scanner failures
* [ ] Add scanner tests
* [ ] Create vulnerable test fixtures
* [ ] Create safe test fixtures

### Deliverable

```text
Source Code
    ↓
Semgrep / Bandit
    ↓
Normalized Findings
```

---

# Phase 2 — AI Analysis

### Goals

Turn raw findings into useful security explanations.

### Tasks

* [ ] Integrate Anthropic API
* [ ] Define Analyst prompt
* [ ] Define structured Analyst output
* [ ] Provide scanner evidence as context
* [ ] Explain vulnerability
* [ ] Explain impact
* [ ] Recommend remediation
* [ ] Add Analyst evaluation tests

### Deliverable

```text
Finding
   ↓
Analyst
   ↓
Structured Security Analysis
```

---

# Phase 3 — AI Remediation

### Goals

Generate targeted fixes.

### Tasks

* [ ] Define Fixer interface
* [ ] Define Fixer prompt
* [ ] Generate fixed code
* [ ] Generate diff
* [ ] Prevent unrelated changes
* [ ] Handle failed generation
* [ ] Add remediation tests

### Deliverable

```text
Finding + Analysis
       ↓
Candidate Fix
       ↓
Diff
```

---

# Phase 4 — LangGraph Workflow

### Goals

Orchestrate the complete process.

### Tasks

* [ ] Define graph state
* [ ] Implement scanner node
* [ ] Implement analyst node
* [ ] Implement fixer node
* [ ] Implement validator node
* [ ] Implement conditional routing
* [ ] Add retry handling
* [ ] Add workflow error handling
* [ ] Add end-to-end tests

### Deliverable

```text
Scan
 ↓
Analyze
 ↓
Fix
 ↓
Validate
```

---

# Phase 5 — Validation

### Goals

Verify that generated fixes actually work.

### Tasks

* [ ] Create isolated execution environment
* [ ] Apply generated patch
* [ ] Run tests
* [ ] Re-run security scanner
* [ ] Compare findings
* [ ] Detect regression
* [ ] Implement timeouts
* [ ] Implement resource limits
* [ ] Produce validation result

### Deliverable

```text
Generated Fix
     ↓
Tests
     +
Security Re-scan
     ↓
Validated / Rejected
```

---

# Phase 6 — Developer Interface

### Goals

Create a usable MVP.

### Tasks

* [ ] Code upload
* [ ] Code input
* [ ] Scan button
* [ ] Findings view
* [ ] Severity display
* [ ] Security explanation
* [ ] Before/after comparison
* [ ] Diff viewer
* [ ] Validation status
* [ ] Error states

### Deliverable

A developer can audit a Python file without interacting with internal APIs.

---

# Phase 7 — GitHub Integration

### Goals

Move from file-level auditing to repository workflows.

### Tasks

* [ ] GitHub authentication
* [ ] Repository selection
* [ ] Repository checkout
* [ ] Repository scanning
* [ ] Finding aggregation
* [ ] Patch generation
* [ ] Branch creation
* [ ] Commit changes
* [ ] Pull request creation
* [ ] PR summary generation

### Deliverable

```text
GitHub Repository
       ↓
Security Scan
       ↓
Validated Fix
       ↓
Pull Request
```

---

# Phase 8 — Team Product

### Goals

Support multiple developers and organizations.

### Tasks

* [ ] Authentication
* [ ] Organizations
* [ ] Users
* [ ] Roles
* [ ] Projects
* [ ] Repository management
* [ ] Vulnerability history
* [ ] Audit logs
* [ ] Notifications

---

# Phase 9 — Scale

### Goals

Support larger repositories and workloads.

### Tasks

* [ ] PostgreSQL
* [ ] Background job queue
* [ ] Worker architecture
* [ ] Redis
* [ ] Object storage
* [ ] Repository caching
* [ ] Rate limiting
* [ ] Observability
* [ ] Metrics
* [ ] Distributed workloads

---

# Phase 10 — Multi-language

Potential order:

1. Python
2. JavaScript / TypeScript
3. Go
4. Java
5. Additional languages based on demand

---

# Phase 11 — Commercial Product

Potential capabilities:

* [ ] Subscription management
* [ ] Usage metering
* [ ] Team plans
* [ ] Enterprise plans
* [ ] Organization security policies
* [ ] Advanced integrations
* [ ] SSO
* [ ] Enterprise deployment options

---

# Guiding Principle

Do not optimize for the number of features.

Optimize for:

**Reliable vulnerability remediation.**

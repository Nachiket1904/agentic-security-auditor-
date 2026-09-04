# Agentic Security Auditor — Architecture

## 1. High-Level Architecture

```text
                         ┌─────────────────────┐
                         │        User         │
                         └──────────┬──────────┘
                                    │
                           Code / File Upload
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       FastAPI       │
                         │      REST API       │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     LangGraph       │
                         │    Orchestrator     │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┼────────────────┐
                    │               │                │
                    ▼               ▼                ▼
               ┌─────────┐    ┌───────────┐    ┌─────────┐
               │ Scanner │    │  Analyst  │    │  Fixer  │
               └────┬────┘    └─────┬─────┘    └────┬────┘
                    │               │               │
                    ▼               ▼               ▼
                Semgrep/         LLM API          LLM API
                 Bandit
                    │               │               │
                    └───────────────┼───────────────┘
                                    │
                                    ▼
                           ┌─────────────────┐
                           │    Validator    │
                           └────────┬────────┘
                                    │
                            Re-scan + Tests
                                    │
                           ┌────────┴────────┐
                           │                 │
                         PASS              FAIL
                           │                 │
                           ▼                 ▼
                      Final Report       Retry/Reject
                           │
                           ▼
                      Diff / Patch
```

---

## 2. Architectural Philosophy

The system intentionally combines deterministic and probabilistic components.

### Deterministic layer

Responsible for:

* Running scanners.
* Parsing source files.
* Applying patches.
* Running tests.
* Re-running security analysis.
* Returning machine-readable results.

### AI layer

Responsible for:

* Understanding findings.
* Explaining risk.
* Reasoning about remediation.
* Generating candidate fixes.

This separation reduces unnecessary LLM usage and makes the system easier to test.

---

## 3. LangGraph

LangGraph will orchestrate the workflow.

The graph represents the state of a security remediation task.

Conceptually:

```text
START
  ↓
scan
  ↓
analyze
  ↓
fix
  ↓
validate
  ↓
 ┌───────────────┐
 │ Validation    │
 │ successful?   │
 └───────┬───────┘
         │
     ┌───┴───┐
    YES      NO
     │        │
     ▼        ▼
    END     retry/reject
```

The graph should maintain structured state rather than passing uncontrolled strings between components.

---

## 4. Example Graph State

Conceptually:

```text
SecurityState

source_code
language
scanner_findings
current_finding
analysis
proposed_patch
validation_result
test_result
attempt_count
final_report
```

Each node should read only the state it needs and produce structured outputs.

---

## 5. Scanner Layer

The scanner layer should hide the implementation details of individual SAST tools.

Example:

```text
Scanner Interface
       │
       ├── SemgrepScanner
       │
       └── BanditScanner
```

The application should work with a normalized internal finding format.

Example:

```text
Finding
├── id
├── rule_id
├── tool
├── severity
├── file
├── line
├── code
├── message
└── metadata
```

This makes adding future scanners easier.

---

## 6. AI Layer

The AI layer should expose specialized components.

```text
agents/
├── analyst.py
├── fixer.py
└── prompts/
    ├── analyst.txt
    └── fixer.txt
```

Agents should return structured data whenever possible.

---

## 7. Validation Layer

The Validator is treated as a security boundary.

It should eventually support:

```text
Patch
 ↓
Isolated workspace
 ↓
Apply patch
 ↓
Run tests
 ↓
Run security scanner
 ↓
Compare results
```

The validator must not blindly execute arbitrary uploaded code in the application's main process.

---

## 8. API Layer

FastAPI will expose application functionality.

Possible initial endpoints:

```text
POST /api/v1/scan
POST /api/v1/analyze
POST /api/v1/remediate
POST /api/v1/validate

GET /api/v1/health
```

As the product evolves, the API can become more resource-oriented.

---

## 9. Frontend

The initial frontend can use Streamlit for rapid MVP development.

The UI should expose:

```text
Upload Code
     ↓
Start Audit
     ↓
Findings
     ↓
Risk Explanation
     ↓
Suggested Fix
     ↓
Validation
     ↓
Before / After Diff
```

If the product gains real users, the frontend can later evolve into a dedicated production web application.

---

## 10. Future GitHub Integration

The GitHub workflow should eventually become:

```text
Repository
     ↓
Clone / Checkout
     ↓
Scan
     ↓
Analyze
     ↓
Generate Fix
     ↓
Validate
     ↓
Create Branch
     ↓
Commit Patch
     ↓
Open Pull Request
```

The system should not automatically merge changes by default.

Human review remains part of the workflow.

---

## 11. Scalability Direction

The initial MVP can be relatively simple.

As workload grows:

```text
                    API
                     │
                     ▼
                Job Queue
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       Worker      Worker      Worker
          │          │          │
          └──────────┼──────────┘
                     │
                     ▼
                 Database
```

Long-running scans and validation tasks should eventually be processed asynchronously rather than blocking API requests.

Potential future infrastructure:

* PostgreSQL
* Redis
* Background workers
* Object storage
* Containerized validation environments

These should be introduced when the workload requires them rather than prematurely.

---

## 12. Security Architecture

Because this product processes source code and may eventually execute code during validation, security is a first-class architectural concern.

Important controls include:

* Input validation.
* Authentication.
* Authorization.
* Secret management.
* Sandboxed execution.
* Resource limits.
* Network restrictions for validation environments.
* Temporary workspaces.
* Cleanup after execution.
* Audit logging.
* Dependency security.
* Safe handling of GitHub credentials.

---

## 13. Design Goal

The architecture should allow the product to evolve from:

```text
Hackathon MVP
```

into:

```text
Developer Security Platform
```

without requiring the core remediation engine to be completely rewritten.

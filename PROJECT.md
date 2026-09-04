# Agentic Security Auditor — Project Specification

## 1. Project Vision

Agentic Security Auditor aims to reduce the distance between identifying a software vulnerability and shipping a validated remediation.

Traditional security tooling primarily focuses on detection.

Our system focuses on the complete remediation workflow:

```text
Detect
  ↓
Understand
  ↓
Prioritize
  ↓
Fix
  ↓
Validate
  ↓
Review
```

The long-term objective is to integrate this workflow directly into the developer's existing software-development process.

---

# 2. Problem Statement

Security vulnerabilities are often identified by automated scanning tools, but the subsequent remediation process remains heavily dependent on developers.

For each finding, developers may need to:

1. Understand the vulnerability.
2. Determine whether it is actually relevant.
3. Assess its severity and impact.
4. Research the appropriate remediation.
5. Modify the source code.
6. Run tests.
7. Re-run security analysis.
8. Review the resulting changes.

This process becomes increasingly expensive as the number of findings increases.

The product therefore targets the **remediation bottleneck**, rather than attempting to replace established SAST tooling.

---

# 3. Existing-System Gap

The project is not intended to compete by simply detecting more vulnerabilities.

Existing SAST tools already provide valuable detection capabilities.

Our focus is the workflow after detection.

```text
Traditional workflow:

Scanner
   ↓
Finding List
   ↓
Developer
   ↓
Manual Investigation
   ↓
Manual Fix
   ↓
Manual Validation
```

Proposed workflow:

```text
Scanner
   ↓
Verified Finding
   ↓
AI Analysis
   ↓
AI Remediation
   ↓
Automated Validation
   ↓
Developer Review
```

---

# 4. Target Users

## Primary

### Small engineering teams

Teams that need security assistance but may not have dedicated application-security engineers.

### Developers

Developers who receive security findings and need actionable remediation guidance.

### Startups

Teams that want security automation without building a complete AppSec platform internally.

## Secondary

### Application-security teams

Security teams that want to reduce repetitive remediation work.

### Engineering organizations

Organizations that want to integrate security remediation into their existing development workflows.

---

# 5. Product Principles

## Grounded AI

The system should use deterministic scanner findings as evidence.

The LLM should not independently claim that arbitrary code is vulnerable when there is no supporting evidence.

## Validation

A generated patch is a candidate until it passes automated checks.

## Minimal remediation

The system should modify only the code required to address the identified vulnerability.

## Transparency

The developer should be able to understand:

* What was found.
* Why it matters.
* What was changed.
* Why the change addresses the issue.
* Whether validation succeeded.

## Human control

Automation should assist developers rather than silently modifying production systems.

---

# 6. MVP Workflow

The first version focuses on Python source code.

```text
User
 ↓
Upload file / provide code
 ↓
Scanner
 ↓
Normalized findings
 ↓
Analyst
 ↓
Risk explanation
 ↓
Fixer
 ↓
Candidate patch
 ↓
Validator
 ↓
Tests + re-scan
 ↓
Final report
```

---

# 7. Scanner

The Scanner is responsible for deterministic vulnerability detection.

Initial tools:

* Semgrep
* Bandit

The application should normalize scanner-specific results into an internal representation.

Example:

```text
Finding
├── id
├── tool
├── rule_id
├── severity
├── file
├── line_start
├── line_end
├── message
├── code
└── metadata
```

This abstraction allows additional scanners to be added later.

---

# 8. Analyst

The Analyst receives verified findings.

Responsibilities:

* Explain the vulnerability.
* Identify the underlying security weakness.
* Explain potential impact.
* Describe the relevant attack scenario.
* Prioritize the issue.
* Recommend remediation strategy.

The Analyst should distinguish between:

* Scanner evidence
* Inference
* Recommendation

This separation is important for transparency.

---

# 9. Fixer

The Fixer generates a remediation candidate.

Inputs may include:

* Vulnerable source code
* Scanner finding
* Rule information
* Analyst output
* Relevant surrounding code

Outputs should include:

* Proposed fixed code
* Diff
* Explanation
* Confidence or validation state

The Fixer should avoid unrelated refactoring.

---

# 10. Validator

The Validator is a core product component.

The system should eventually perform:

```text
Candidate Patch
      ↓
Apply Patch
      ↓
Run Tests
      ↓
Run Security Scanner
      ↓
Compare Results
      ↓
Validation Decision
```

Possible outcomes:

```text
VALIDATED
FAILED_SECURITY_CHECK
FAILED_TESTS
PATCH_ERROR
TIMEOUT
```

A failed validation should not be represented as a successful remediation.

---

# 11. LangGraph Orchestration

LangGraph will coordinate the multi-step workflow.

Conceptually:

```text
START
  ↓
SCAN
  ↓
ANALYZE
  ↓
FIX
  ↓
VALIDATE
  ↓
Decision
 ┌┴──────────┐
PASS       FAIL
 │           │
END       Retry/Reject
```

The workflow state should contain structured data rather than relying on uncontrolled text passing.

---

# 12. Security Scope

The MVP should initially focus on a manageable set of vulnerabilities supported well by the chosen scanners.

Potential categories include:

* Injection-related issues
* Unsafe command execution
* Insecure deserialization
* Hardcoded credentials/secrets where supported
* Unsafe cryptographic practices
* Insecure filesystem operations
* Other Python security patterns with reliable scanner support

The system should not claim complete OWASP Top 10 coverage.

OWASP itself states that the Top 10 is an awareness document and not a comprehensive security-testing standard. For comprehensive verification requirements, OWASP points organizations toward ASVS.

---

# 13. Risk Model

The Analyst should not treat severity as a purely arbitrary LLM score.

Where possible, risk assessment should incorporate:

* Scanner severity
* Vulnerability type
* Exploitability
* Potential technical impact
* Application context
* Exposure
* Confidence in the finding

OWASP's current risk model also distinguishes factors such as exploitability, likelihood of missing controls, technical impact, and business impact.

---

# 14. Product Evolution

## Stage 1

Code/file auditing.

## Stage 2

Repository auditing.

## Stage 3

Validated GitHub pull requests.

## Stage 4

Continuous repository monitoring.

## Stage 5

Team security platform.

## Stage 6

Multi-language and enterprise integrations.

---

# 15. Commercial Direction

The product may eventually be offered as a developer-security platform.

Potential commercial model:

```text
Free / Developer
       ↓
Team
       ↓
Business
       ↓
Enterprise
```

Potential pricing dimensions:

* Number of repositories
* Number of developers
* Number of scans
* Number of vulnerabilities processed
* Advanced integrations
* Organization-level features

Pricing should not be finalized until customer discovery validates who receives enough value to pay.

---

# 16. Product Differentiation

The primary product thesis is:

> Detection is increasingly commoditized. The opportunity is reducing the work required after detection.

The system differentiates through:

1. Deterministic security grounding.
2. AI-assisted reasoning.
3. AI-generated remediation.
4. Automated validation.
5. Developer-friendly explanations.
6. Future pull-request integration.

---

# 17. Success Criteria

The MVP is successful if it can reliably demonstrate:

```text
Vulnerable Code
      ↓
Real Scanner Finding
      ↓
Correct Analysis
      ↓
Useful Fix
      ↓
Passing Tests
      ↓
Finding Resolved
```

The demonstration should focus on measurable remediation quality rather than simply showing an LLM-generated code snippet.

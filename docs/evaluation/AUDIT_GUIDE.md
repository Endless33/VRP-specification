# Audit Guide

## Status

Public Evaluation Documentation

Version: 1.0

---

# Purpose

This document defines the public audit methodology for engineering evaluation of the Veil Routing Protocol (VRP).

The purpose of an audit is to independently determine whether observable runtime behavior conforms to the published architectural specification.

Audits evaluate engineering evidence.

They do not require implementation disclosure.

---

# Engineering Philosophy

An audit should answer one question:

Does observable runtime behavior preserve the published architectural invariants?

The audit evaluates observable facts.

It does not evaluate marketing claims.

---

# Audit Objectives

An engineering audit should determine:

- whether the evaluated scenario was executed correctly;
- whether engineering evidence is complete;
- whether architectural invariants were preserved;
- whether the engineering conclusion is supported by observable evidence;
- whether the evaluation can be reproduced independently.

---

# Audit Scope

The public audit includes:

- scenario definition
- execution procedure
- observable runtime behavior
- engineering evidence
- engineering verdict
- reproducibility

Protected implementation remains outside the audit scope.

---

# Audit Workflow

```
Scenario Definition
        │
        ▼
Execution
        │
        ▼
Evidence Collection
        │
        ▼
Evidence Verification
        │
        ▼
Invariant Evaluation
        │
        ▼
Engineering Conclusion
        │
        ▼
Audit Report
```

Every audit follows the same observable process.

---

# Audit Inputs

Typical audit inputs include:

- scenario documentation
- runtime version
- public configuration
- observable runtime events
- engineering evidence
- validation reports

Internal runtime implementation is not required.

---

# Audit Questions

Every audit should answer the following questions.

## Scenario

Was the documented scenario executed?

---

## Runtime

Did observable runtime behavior match the documented architecture?

---

## Authority

Was exactly one canonical authority preserved?

---

## Session

Was the Logical Session handled correctly?

---

## Replay

Was historical execution rejected?

---

## Recovery

Did recovery preserve architectural correctness?

---

## Evidence

Does engineering evidence support the reported conclusion?

---

## Reproducibility

Can another engineering team independently obtain the same architectural conclusion?

---

# Audit Checklist

An engineering audit should verify:

- scenario completeness
- runtime identification
- observable event history
- authority history
- recovery history
- engineering verdict
- evidence integrity
- reproducibility

Every item should be supported by observable evidence.

---

# Engineering Verdict

An audit concludes with one of the following engineering outcomes.

## PASS

Observable behavior matches the published architecture.

---

## FAIL

One or more architectural invariants were violated.

---

## INCOMPLETE

Insufficient observable evidence exists.

No engineering conclusion should be made.

---

## NOT APPLICABLE

The evaluated scenario does not apply to the requested architectural property.

---

# Auditor Independence

Engineering reviewers should remain independent from implementation.

Auditors evaluate:

- observable behavior
- engineering evidence
- published specifications

Implementation disclosure is unnecessary.

---

# Evidence Preservation

Audit artifacts should remain available for:

- future engineering review
- regression comparison
- historical analysis
- independent verification

Engineering history should remain understandable over time.

---

# Relationship to Other Documents

This document complements:

- TEST_MATRIX.md
- PASS_CRITERIA.md
- FAILURE_CRITERIA.md
- REPRODUCIBILITY.md
- EVIDENCE_FORMAT.md

It also supports:

- RFC-0005 — Evidence Model
- RFC-0011 — Pilot Integration

---

# Summary

The Audit Guide defines a reproducible engineering methodology for independently evaluating the observable behavior of the VRP Runtime.

Engineering audits are based upon observable evidence.

Architectural correctness determines the engineering conclusion.

Protected implementation remains protected.

---

## Design Principles

- Audit observable behavior.
- Verify engineering evidence.
- Preserve auditor independence.
- Reproduce engineering conclusions.
- Trust evidence before assumptions.
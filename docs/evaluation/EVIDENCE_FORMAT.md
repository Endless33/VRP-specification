# Evidence Format

## Status

Public Evaluation Documentation

Version: 1.0

---

# Purpose

This document defines the public Evidence Format used during engineering evaluation of the Veil Routing Protocol (VRP).

Engineering evidence provides an observable record of runtime behavior.

Evidence enables independent technical review without requiring access to the protected runtime implementation.

This document specifies architectural expectations rather than implementation-specific serialization formats.

---

# Engineering Philosophy

Evidence is not generated to prove marketing claims.

Evidence exists to support engineering conclusions.

Observable behavior should always be supported by observable evidence.

Evidence without reproducibility has limited engineering value.

---

# Evidence Objectives

Engineering evidence should allow an independent reviewer to determine:

- what scenario was executed;
- which runtime version was evaluated;
- which observable events occurred;
- whether architectural invariants were preserved;
- whether engineering conclusions are reproducible.

---

# Evidence Principles

Engineering evidence should be:

- observable
- deterministic
- reproducible
- internally consistent
- timestamped
- auditable

Evidence should never contradict observable runtime behavior.

---

# Evidence Structure

Typical engineering evidence consists of:

```
Scenario Description
        │
        ▼
Runtime Metadata
        │
        ▼
Observable Event History
        │
        ▼
Authority History
        │
        ▼
Transport History
        │
        ▼
Recovery History
        │
        ▼
Engineering Verdict
        │
        ▼
Evidence Integrity Information
```

The exact serialization format remains implementation-specific.

---

# Required Evidence Elements

Every engineering evidence package should include:

- scenario identifier
- runtime version
- execution timestamp
- observable runtime events
- engineering verdict
- invariant evaluation summary

These elements support independent review.

---

# Optional Evidence Elements

Evidence may additionally include:

- transport timeline
- authority timeline
- recovery timeline
- runtime metrics
- environment description
- validation metadata

Optional information should improve engineering understanding without affecting architectural conclusions.

---

# Event History

Evidence should preserve the observable sequence of runtime events.

Historical events must not be rewritten.

Event ordering should remain deterministic.

---

# Authority History

Evidence should document observable authority progression.

Authority history should demonstrate:

- establishment
- transition
- confirmation
- rejection of stale authority

Authority history supports independent validation.

---

# Recovery History

Recovery evidence should describe:

- recovery trigger
- recovery evaluation
- recovery outcome
- resulting engineering verdict

Recovery evidence should remain observable.

---

# Engineering Verdict

Every evidence package should contain a clear engineering verdict.

Examples include:

- PASS
- FAIL
- INCOMPLETE
- NOT APPLICABLE

Verdicts should correspond to documented evaluation criteria.

---

# Evidence Integrity

Evidence integrity should ensure that reviewers can determine whether:

- evidence is complete;
- evidence is internally consistent;
- observable behavior matches engineering conclusions.

Integrity verification mechanisms remain implementation-specific.

---

# Independent Review

An independent reviewer should be able to answer:

- What was evaluated?
- What occurred?
- Which invariants were verified?
- Which evidence supports the conclusion?

Protected implementation should not be required.

---

# Long-Term Preservation

Engineering evidence should remain suitable for:

- later engineering review;
- audit;
- regression comparison;
- historical validation;
- independent verification.

Historical engineering conclusions should remain understandable over time.

---

# Relationship to Other Documents

This document complements:

- TEST_MATRIX.md
- PASS_CRITERIA.md
- FAILURE_CRITERIA.md
- REPRODUCIBILITY.md
- AUDIT_GUIDE.md

It also supports:

- RFC-0005 — Evidence Model
- RFC-0011 — Pilot Integration

---

# Summary

Engineering evidence provides the observable foundation for independent technical evaluation.

Evidence documents runtime behavior.

Evidence supports engineering conclusions.

Evidence enables reproducible validation without implementation disclosure.

---

## Design Principles

- Observe execution.
- Record observable behavior.
- Preserve engineering history.
- Support independent review.
- Produce reproducible evidence.
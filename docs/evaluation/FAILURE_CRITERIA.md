# FAILURE Criteria

## Status

Public Evaluation Documentation

Version: 1.0

---

# Purpose

This document defines the observable criteria under which an engineering validation scenario is considered **FAIL** during evaluation of the Veil Routing Protocol (VRP).

A FAIL verdict indicates that one or more required architectural invariants were not preserved under the evaluated conditions.

The objective of a FAIL verdict is not to assign blame.

Its purpose is to identify where observable runtime behavior diverges from the architectural model.

---

# Engineering Philosophy

A runtime does not fail because infrastructure fails.

A runtime fails when architectural correctness cannot be preserved.

Infrastructure failure is expected.

Invariant violation is not.

---

# FAIL Definition

A validation scenario is considered **FAIL** when at least one required architectural invariant is violated.

Engineering evidence must support the conclusion.

A FAIL verdict should always be reproducible whenever the same observable conditions are recreated.

---

# Mandatory Failure Conditions

The following observable conditions constitute architectural failure.

---

## Logical Session Failure

Examples include:

- unexpected session replacement
- unintended session duplication
- lost session continuity
- incorrect session ownership

Observable invariant violated:

Logical Session identity.

---

## Authority Failure

Examples include:

- multiple canonical authorities
- authority rollback
- stale authority accepted
- historical authority resumed execution

Observable invariant violated:

Canonical Authority.

---

## Replay Failure

Examples include:

- replay accepted
- historical execution executed again
- duplicate execution accepted

Observable invariant violated:

Replay Protection.

---

## Runtime State Failure

Examples include:

- invalid state transition
- impossible runtime state
- inconsistent state evolution
- state resurrection

Observable invariant violated:

Runtime State Machine.

---

## Recovery Failure

Examples include:

- recovery violated authority
- recovery accepted replay
- recovery created duplicate execution
- recovery produced inconsistent runtime state

Observable invariant violated:

Recovery Correctness.

---

## Determinism Failure

Examples include:

- identical observable scenarios produce conflicting conclusions
- inconsistent authority decisions
- inconsistent recovery outcomes

Observable invariant violated:

Deterministic Runtime Decisions.

---

## Evidence Failure

Examples include:

- evidence contradicts runtime behavior
- incomplete engineering evidence
- inconsistent evidence history
- unverifiable engineering conclusion

Observable invariant violated:

Evidence Integrity.

---

# Failure Severity

Engineering reports MAY classify failures according to severity.

Examples include:

### Critical

Architectural invariant violated.

Engineering confidence lost.

---

### Major

Observable behavior significantly diverges from expected architecture.

Engineering review required.

---

### Minor

Observable behavior differs without violating architectural correctness.

Engineering improvement recommended.

Severity classification remains implementation-independent.

---

# Engineering Investigation

Every FAIL verdict should answer:

- Which invariant failed?
- Which observable behavior demonstrated failure?
- Which evidence supports the conclusion?
- Can the failure be reproduced?

Failure without evidence should be considered incomplete.

---

# Expected Engineering Output

A FAIL report SHOULD include:

- evaluated scenario
- failed invariant
- observable runtime events
- engineering evidence
- engineering conclusion
- recommended investigation

Observable engineering evidence is mandatory.

---

# Relationship to PASS

PASS and FAIL are mutually exclusive engineering conclusions.

A scenario cannot simultaneously preserve and violate the same architectural invariant.

Engineering evidence determines the outcome.

---

# Relationship to Other Documents

This document complements:

- TEST_MATRIX.md
- PASS_CRITERIA.md
- REPRODUCIBILITY.md
- EVIDENCE_FORMAT.md
- AUDIT_GUIDE.md

It also supports:

- RFC-0005 — Evidence Model
- RFC-0012 — Threat Model

---

# Summary

A FAIL verdict indicates that observable runtime behavior violated one or more architectural invariants.

Engineering conclusions must be evidence-based.

Observable correctness remains the basis of evaluation.

---

## Design Principles

- Detect invariant violations.
- Document observable behavior.
- Produce reproducible evidence.
- Investigate objectively.
- Improve architectural correctness.
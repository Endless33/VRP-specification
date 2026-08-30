# PASS Criteria

## Status

Public Evaluation Documentation

Version: 1.0

---

# Purpose

This document defines the observable criteria required for a validation scenario to be considered **PASS** during engineering evaluation of the Veil Routing Protocol (VRP).

PASS indicates that the evaluated scenario preserved all required architectural invariants under the tested conditions.

PASS does not imply perfect infrastructure.

PASS demonstrates correct runtime behavior.

---

# Engineering Philosophy

A successful test is not one where nothing unexpected happened.

A successful test is one where the runtime preserved correctness despite expected failures or adversarial conditions.

Engineering confidence comes from invariant preservation.

---

# PASS Definition

A scenario is considered **PASS** when:

- observable runtime behavior matches the expected architectural behavior;
- required invariants remain preserved;
- engineering evidence supports the conclusion;
- the result is reproducible.

All four conditions should be satisfied.

---

# Mandatory Conditions

The following conditions are evaluated.

---

## Logical Session

Expected result:

The Logical Session remains correct for the evaluated scenario.

Examples:

- continuity preserved
- expected termination
- deterministic replacement (if explicitly defined)

---

## Authority

Expected result:

Exactly one canonical authority exists.

Observable authority progression remains monotonic.

Historical authority never becomes canonical again.

---

## Runtime State

Expected result:

Runtime State Machine remains valid.

No undefined state becomes observable.

---

## Replay Protection

Expected result:

Replay attempts are rejected.

Historical execution does not become accepted execution.

---

## Recovery

Expected result:

Recovery either:

- preserves architectural correctness, or
- terminates safely.

Incorrect recovery never qualifies as PASS.

---

## Transport

Expected result:

Transport evolution behaves according to the scenario.

Transport changes do not redefine the Logical Session.

---

## Determinism

Expected result:

Equivalent observable conditions produce equivalent engineering conclusions.

Minor timing differences are acceptable.

Architectural conclusions must remain identical.

---

## Evidence

Expected result:

Engineering evidence:

- exists
- is internally consistent
- matches observable runtime behavior
- supports independent review

---

# PASS Classification

Engineering reports may classify PASS using descriptive verdicts.

Examples include:

- PASS
- CONTINUITY_PRESERVED
- AUTHORITY_PRESERVED
- REPLAY_REJECTED
- RECOVERY_PRESERVED
- EVIDENCE_VERIFIED
- DETERMINISTIC_PASS

The specific wording is implementation-independent.

---

# Partial Success

A scenario is **not** considered PASS if only some objectives are achieved.

Example:

- replay rejected
- authority preserved
- runtime entered invalid state

Overall result:

FAIL

Architectural invariants are evaluated together.

---

# Observable Evidence

PASS should be supported by observable evidence including:

- runtime events
- authority history
- transport history
- recovery history
- engineering reports
- validation summaries
- evidence bundles

Evidence enables independent confirmation.

---

# Reproducibility

PASS should be reproducible.

Equivalent engineering environments should produce equivalent architectural conclusions.

Reproducibility strengthens engineering confidence.

---

# Engineering Review

A reviewer should be able to answer:

- Which scenario was executed?
- Which invariants were evaluated?
- Which observable evidence supports PASS?
- Can the scenario be repeated?

If these questions cannot be answered, PASS should be reconsidered.

---

# Relationship to Other Documents

This document complements:

- TEST_MATRIX.md
- FAILURE_CRITERIA.md
- REPRODUCIBILITY.md
- EVIDENCE_FORMAT.md
- AUDIT_GUIDE.md

It also supports:

- RFC-0005 — Evidence Model
- RFC-0011 — Pilot Integration

---

# Summary

PASS indicates preservation of architectural correctness.

PASS is based upon observable runtime behavior.

PASS is supported by engineering evidence.

PASS is reproducible.

---

## Design Principles

- Preserve invariants.
- Produce evidence.
- Verify independently.
- Repeat successfully.
- Conclude objectively.
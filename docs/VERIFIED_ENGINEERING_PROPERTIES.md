# Verified Engineering Properties

## Purpose

This document summarizes engineering properties that have been demonstrated through reproducible validation.

The purpose is not to describe implementation details.

The purpose is to document observable engineering behavior that has been repeatedly validated.

Protected runtime implementation remains private.

---

# Validation Philosophy

VRP engineering claims are expected to be supported by reproducible evidence.

Engineering confidence is built through:

- executable validation
- deterministic execution
- adversarial testing
- observable runtime behavior
- independent verification

The objective is not to ask readers to trust architectural claims.

The objective is to allow engineers to reproduce observable behavior.

---

# Verified Properties

Current public validation has demonstrated observable evidence for:

- deterministic session state transitions
- transport-independent session continuity
- replay rejection
- stale authority rejection
- duplicate transition rejection
- canonical state preservation
- canonical transition history preservation
- deterministic runtime execution
- fail-closed behavior
- authority monotonicity
- epoch monotonicity
- restart boundary validation
- session reincarnation fencing
- deterministic recovery behavior
- concurrent execution safety
- randomized execution stability
- resource-bounded runtime behavior
- evidence integrity

---

# Observable Validation Scope

Public validation currently includes engineering evidence covering:

- transport migration
- Wi-Fi ↔ mobile transitions
- relay migration
- authority transfer
- epoch advancement
- replay scenarios
- stale runtime events
- duplicate execution attempts
- restart scenarios
- canonical recovery
- runtime convergence
- deterministic replay
- randomized execution ordering
- race detection
- long-duration repeated execution

---

# Engineering Interpretation

These validation results do not attempt to prove that networks cannot fail.

Networks fail.

Links disappear.

Routes change.

Infrastructure restarts.

The engineering objective is different.

The objective is to determine whether canonical execution remains correct while these conditions occur.

---

# Reproducibility

Validation is expected to be reproducible.

Evidence may include:

- terminal output
- deterministic validation reports
- runtime traces
- adversarial execution logs
- cryptographic hashes
- engineering verdicts

Observable behavior is considered significantly more valuable than architectural assertions alone.

---

# Current Status

Public validation continues to expand as new engineering hypotheses are tested.

Every completed validation stage strengthens confidence in observable runtime behavior.

Every failed experiment becomes an engineering task.

Every successful experiment becomes reproducible engineering evidence.

---

# Design Principle

Engineering should not ask people to believe.

Engineering should provide enough observable evidence for independent evaluation.

---

**Continuity First.**
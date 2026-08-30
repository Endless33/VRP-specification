# Security Boundaries

## Status

Public Pilot Documentation

Version: 2.0

---

# Purpose

This document defines the security and intellectual property boundaries of the VRP Pilot Program.

Its objective is to establish a clear separation between observable engineering evaluation and protected implementation.

Every participant operates within the same documented security boundary.

---

# Engineering Principle

The Pilot is designed to evaluate runtime behavior.

It is not designed to disclose implementation.

Participants evaluate observable engineering properties.

The protected runtime remains confidential.

---

# Public Evaluation Boundary

The following areas are within the scope of the Pilot.

Participants may evaluate:

- logical session continuity
- transport evolution
- authority transitions
- deterministic runtime behavior
- replay rejection
- duplicate execution protection
- recovery behavior
- runtime evidence
- validation reports
- engineering reproducibility

Observable behavior exists specifically for technical evaluation.

---

# Protected Boundary

The following areas remain outside the scope of every Pilot.

They are not disclosed through documentation, demonstrations, meetings or evaluation activities.

Examples include:

- source code
- runtime implementation
- proprietary algorithms
- scheduling logic
- synchronization mechanisms
- cryptographic key material
- internal protocol encoding
- packet formats
- optimization strategies
- memory layouts
- internal runtime architecture
- development tooling
- private repositories

These components constitute protected intellectual property.

---

# Reverse Engineering

The Pilot is not intended to support:

- implementation reconstruction
- protocol cloning
- binary analysis
- extraction of proprietary mechanisms
- implementation fingerprinting
- protected architecture mapping

Observable behavior may be evaluated.

Protected implementation may not.

---

# Engineering Discussions

Technical discussions are encouraged.

Examples include:

- runtime behavior
- engineering trade-offs
- deployment considerations
- validation methodology
- observable recovery
- architectural concepts

Questions whose primary objective is disclosure of protected implementation may not be answered.

---

# Runtime Evidence

Engineering evidence may be reviewed and independently verified.

Evidence exists to support engineering confidence.

Evidence does not imply implementation disclosure.

Observable evidence and protected implementation are intentionally separated.

---

# Demonstrations

Demonstrations may include:

- runtime execution
- validation scenarios
- recovery behavior
- engineering reports
- evidence verification

Demonstrations do not include:

- source code walkthroughs
- implementation debugging
- proprietary algorithms
- confidential architecture

---

# Security Objectives

The Pilot preserves:

- intellectual property
- engineering transparency
- reproducibility
- implementation confidentiality
- independent evaluation

These objectives are considered complementary rather than contradictory.

---

# Intellectual Property

Participation in the Pilot does not transfer:

- ownership
- licensing rights
- implementation rights
- patent rights
- commercial rights
- derivative rights

All protected technology remains the property of its owner unless explicitly agreed otherwise.

---

# Confidential Information

Protected information includes, but is not limited to:

- unpublished documentation
- implementation details
- internal engineering procedures
- source repositories
- runtime internals
- confidential design material

Disclosure requires explicit written authorization.

---

# Security Reporting

Participants discovering a security concern are encouraged to report it through the responsible disclosure process.

Engineering collaboration is preferred over public disclosure during an active evaluation.

---

# Evaluation Integrity

Every participant receives the same architectural boundary.

No participant receives hidden implementation access as part of a standard Pilot.

Engineering conclusions should therefore be based on observable runtime behavior.

---

# Related Documents

- PILOT_GUIDE.md
- EVALUATION_PROCESS.md
- REQUIREMENTS.md
- NDA.md
- SECURITY.md

---

# Summary

The VRP Pilot balances two equally important objectives:

- enabling independent engineering evaluation
- protecting proprietary implementation

Observable runtime behavior is intentionally public.

Protected implementation is intentionally private.

This separation enables trustworthy evaluation without compromising intellectual property.

---

> Observable behavior is open to evaluation.

> Protected implementation is not.

> Engineering confidence does not require implementation disclosure.
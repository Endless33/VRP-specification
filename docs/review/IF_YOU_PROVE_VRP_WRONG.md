# If You Prove VRP Wrong

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

Engineering advances through verification.

This document explains what should happen if someone discovers a flaw in VRP.

The objective is not to avoid criticism.

The objective is to encourage reproducible technical review.

---

# Engineering Philosophy

Every protocol should be questioned.

Every architecture should be challenged.

Every implementation should be tested.

No engineering system should be considered correct simply because its author says so.

---

# If You Believe You Found a Problem

Please do not stop at saying:

"VRP doesn't work."

Instead:

- reproduce the behavior
- document the environment
- describe the scenario
- provide logs
- provide evidence
- explain why the observed behavior violates the documented architecture

Engineering discussions become valuable when they are reproducible.

---

# If You Can Break an Invariant

That is important.

Please document:

- the invariant
- the expected behavior
- the observed behavior
- reproduction steps
- runtime output
- engineering evidence

A reproducible issue is always more valuable than an unsupported claim.

---

# If You Find a Security Weakness

Responsible technical disclosure is encouraged.

Include:

- attack description
- assumptions
- limitations
- reproducibility
- observable evidence

Security improves through engineering review.

---

# If Your Results Cannot Be Reproduced

They should be treated as observations rather than engineering conclusions.

VRP places reproducibility above opinion.

---

# Independent Verification

Organizations are encouraged to:

- repeat experiments
- modify workloads
- increase scale
- inspect implementation
- compare results

Independent verification is a design objective.

---

# There Is No Penalty For Being Wrong

Engineering is an iterative process.

A failed hypothesis still contributes to better engineering.

Good criticism strengthens architecture.

---

# There Is No Reward For Unsupported Claims

Assertions without evidence should not be treated as engineering conclusions.

Evidence should always accompany technical claims.

---

# Final Principle

If VRP fails,

the evidence should show it.

If VRP succeeds,

the evidence should show that too.

The purpose of engineering is not to defend ideas.

The purpose of engineering is to discover what is true.
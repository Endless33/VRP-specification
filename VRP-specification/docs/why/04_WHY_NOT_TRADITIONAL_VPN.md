Why Not a Traditional VPN

Document Version: Public v1

Status: Public

---

Purpose

This document explains why VRP should not be viewed as a traditional Virtual Private Network (VPN).

The objective is to clarify the engineering goals of the project and explain why it explores a different architectural direction.

This document does not compare products or evaluate existing VPN technologies.

---

Different Engineering Objectives

Traditional VPN technologies primarily focus on establishing a secure communication tunnel between endpoints.

VRP explores a different engineering objective.

Its primary focus is maintaining validated communication continuity while allowing observable transport changes throughout the lifetime of a communication session.

---

Different Architectural Focus

Traditional VPN solutions generally manage encrypted transport connectivity.

VRP investigates architectural techniques centered around:

- session continuity
- transport independence
- deterministic recovery
- observable runtime behaviour
- engineering evidence
- reproducible validation

These objectives are complementary to secure communication rather than replacements for it.

---

Engineering Perspective

The question addressed by VRP is not:

"How can one transport remain connected?"

Instead, the architectural question becomes:

"How can application continuity remain predictable while transports change?"

This distinction influences the overall design philosophy of the project.

---

Observable Behaviour

Public engineering validation focuses on observable behaviour such as:

- session continuity
- recovery validation
- replay handling
- deterministic execution
- runtime validation
- engineering evidence

Observable behaviour is emphasized over assumptions regarding individual transport paths.

---

Scope

VRP is not intended to replace every VPN deployment.

Many environments are appropriately served by existing VPN technologies.

VRP explores engineering approaches for scenarios where continuity, validation, recovery, and transport independence are considered primary architectural requirements.

---

Public Engineering Validation

Validation activities may include:

- functional testing
- stress testing
- fuzzing
- race detection
- binary inspection
- reverse engineering validation
- reproducible engineering evidence

These activities evaluate observable properties of the runtime rather than implementation details.

---

Protected Boundary

This document intentionally excludes:

- protected runtime algorithms
- proprietary implementation details
- cryptographic secrets
- internal protocol logic
- implementation-specific engineering mechanisms

Only the public engineering motivation is described.

---

Conclusion

VRP investigates a communication architecture whose primary objective is validated session continuity across changing transport conditions.

It should therefore be understood as an exploration of continuity-oriented protocol design rather than as a traditional VPN implementation.
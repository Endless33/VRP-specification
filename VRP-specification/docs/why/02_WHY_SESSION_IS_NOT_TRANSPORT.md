Why Session Is Not Transport

Document Version: Public v1

Status: Public

---

Purpose

This document explains one of the fundamental architectural principles of VRP:

Session ≠ Transport

The objective is to describe why these concepts are intentionally treated as independent engineering concerns.

This document does not disclose protected runtime implementation.

---

Two Different Responsibilities

A communication session represents the logical relationship between participants.

A transport represents only one available mechanism for delivering data between those participants.

Although they interact during communication, they serve different purposes and therefore should not necessarily share the same lifecycle.

---

Traditional Assumption

Many communication systems closely associate a session with a specific transport connection.

When that transport disappears, the session is often interrupted, restarted, or reconstructed.

This approach is appropriate for many environments but may introduce limitations in highly dynamic networks.

---

Engineering Observation

Modern systems frequently experience transport changes caused by events such as:

- Wi-Fi to mobile transitions
- mobile network changes
- NAT rebinding
- routing changes
- temporary connectivity loss
- infrastructure failover

These events do not necessarily imply that the logical communication session should cease to exist.

---

Architectural Principle

VRP explores an architecture in which the communication session remains the primary object.

Transport mechanisms are treated as replaceable components capable of changing during the lifetime of a session.

This separation allows engineering validation to focus on continuity of the session rather than continuity of any individual transport.

---

Engineering Benefits

Separating session identity from transport enables engineering validation of:

- continuity across transport changes
- deterministic recovery
- controlled failover
- replay protection
- independent transport replacement
- observable runtime behaviour

The exact implementation of these mechanisms remains protected.

---

Validation

Public engineering validation focuses on observable behaviour.

Typical validation activities may include:

- transport transition testing
- recovery validation
- replay validation
- deterministic execution
- runtime stress testing
- evidence collection

Engineering conclusions are based on reproducible observations.

---

Protected Boundary

This document intentionally excludes:

- protected runtime algorithms
- proprietary implementation details
- internal session management
- cryptographic mechanisms
- implementation-specific protocol logic

Only the architectural principle is described publicly.

---

Conclusion

VRP distinguishes the logical communication session from the underlying transport.

This architectural separation allows the project to explore continuity-oriented communication models while validating observable behaviour through reproducible engineering evidence and preserving the confidentiality of protected implementation.
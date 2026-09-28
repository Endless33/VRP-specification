Why VRP Exists

Document Version: Public v1

Status: Public

---

Purpose

This document explains why VRP was created.

Its purpose is not to compare products or technologies, but to describe the engineering problems that motivated the development of the protocol.

This document discusses architectural objectives rather than implementation details.

---

Background

Modern applications increasingly operate across unstable and heterogeneous networks.

Users frequently move between different access technologies including Wi-Fi, Ethernet, mobile networks, satellite links, VPN gateways, and other transport environments.

Traditional communication models often assume that transport continuity and session continuity are closely related.

In many environments this assumption becomes a practical limitation.

---

Engineering Observation

A communication session represents application state.

A transport connection represents only one possible delivery path.

These concepts solve different engineering problems and therefore should not necessarily share the same lifecycle.

---

Why VRP Was Created

VRP was developed to explore an architecture where application continuity is treated as the primary objective.

Instead of treating transport replacement as a failure, transport changes are considered expected operational events.

The protocol is designed around preserving session continuity while allowing observable transport changes to occur independently.

---

Engineering Objectives

The public objectives of VRP include:

- preserving session continuity
- supporting transport independence
- enabling deterministic recovery
- validating observable runtime behaviour
- producing reproducible engineering evidence
- maintaining fail-closed operation

These objectives define the architectural direction of the project.

---

What VRP Does Not Attempt To Solve

VRP is not intended to replace every networking solution.

It is not designed to optimize every workload.

It is not intended to be the simplest possible communication protocol.

Instead, it explores engineering techniques for systems where continuity, validation, and controlled recovery are primary requirements.

---

Public Engineering Approach

The project emphasizes:

- reproducible validation
- deterministic testing
- engineering evidence
- observable behaviour
- continuous verification

Public engineering documentation focuses on demonstrating these principles without disclosing protected implementation.

---

Protected Boundary

This document intentionally excludes:

- protected runtime algorithms
- proprietary implementation details
- cryptographic secrets
- internal protocol mechanisms
- private engineering procedures

Only public architectural motivation is described.

---

Conclusion

VRP exists to investigate a continuity-oriented communication architecture in which session behaviour is treated independently from transport behaviour.

The project emphasizes reproducible engineering validation, observable behaviour, and evidence-based development while preserving the confidentiality of protected implementation.
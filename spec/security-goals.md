# Security Goals and Non-Goals — VRP

## 1. Purpose

This document defines the **explicit security goals and non-goals** of the Veil Routing Protocol (VRP).

Clear boundaries are intentional and required for correct security evaluation.

---

## 2. Security Goals

VRP is designed to achieve the following goals:

- Confidentiality of user payload data
- Integrity and authenticity of transmitted packets
- Forward secrecy for all sessions
- Resistance to short- and medium-term traffic correlation
- Limited exposure from compromised routes or nodes
- Adaptive survivability through continuous route movement

---

## 3. Non-Goals

VRP explicitly does **not** guarantee:

- Protection against a global passive adversary
- Anonymity against endpoint compromise
- Protection against application-layer fingerprinting
- Resistance to user behavioral deanonymization
- Legal or physical coercion of node operators

---

## 4. Design Trade-offs

VRP prioritizes:
- Movement over stability
- Reaction over certainty
- Damage containment over total prevention

False positives and increased latency are accepted costs.

---

## 5. Summary

VRP is designed to **limit exposure and adapt quickly**, not to promise perfect anonymity.
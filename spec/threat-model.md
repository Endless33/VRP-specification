# VRP Threat Model

This document defines the threat model for the Veil Routing Protocol (VRP).
It describes the assets to protect, assumed adversaries, trust boundaries,
and the security goals VRP is designed to achieve.

VRP makes no claims outside this model.

---

## Assets

VRP aims to protect the following assets:

- Confidentiality of client payload data
- Integrity of transmitted packets
- Route structure and path history
- Client behavioral patterns
- Session survivability under active interference

---

## Adversary Model

VRP considers the following adversaries:

### Passive Network Adversary
- Can observe traffic on one or more network segments
- Can perform traffic analysis and correlation attempts
- Cannot break modern cryptography

### Active Network Adversary
- Can inject, modify, delay, or drop packets
- Can attempt MITM attacks
- Can perform selective denial-of-service

### Malicious or Compromised Nodes
- Some relays or nodes may be malicious
- Nodes may attempt to log, analyze, or infer routes
- Nodes may collude with external observers

### Resource-Constrained Adversary
- Has limited visibility
- Cannot observe the entire global network simultaneously
- Operates under realistic infrastructure constraints

---

## Out-of-Scope Adversaries

VRP does NOT attempt to defend against:

- Global passive adversaries with full visibility
- Physical device compromise
- Malware or OS-level compromise on the client
- Side-channel attacks outside the network layer
- Legal or coercive attacks against users or operators

---

## Trust Assumptions

VRP assumes:

- The client device is trusted at session start
- Cryptographic primitives are secure
- At least one routing path component is non-colluding
- Time-limited exposure reduces correlation risk

No single relay or authority is trusted by default.

---

## Security Goals

Within this threat model, VRP aims to:

- Reduce traffic correlation through route movement
- Limit information exposure per node
- Detect and react to anomalous network behavior
- Preserve forward secrecy across route changes
- Prevent long-term route or behavior reconstruction

---

## Security Non-Goals

VRP explicitly does NOT guarantee:

- Absolute anonymity
- Untraceability against global observers
- Perfect resistance to all DoS attacks
- Protection against compromised endpoints

Claims beyond this model are invalid.

---

## Residual Risks

Residual risks include:

- False positives triggering route changes
- Performance degradation due to movement
- Limited anonymity in small relay sets
- Correlation under prolonged observation

These risks are accepted trade-offs.

---

End of threat model.
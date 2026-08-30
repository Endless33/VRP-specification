# Adversarial Scenarios — VRP

## 1. Purpose

This document describes **concrete adversarial scenarios**
and the expected VRP response to each.

The goal is not to claim invulnerability,
but to define **bounded damage and adaptive behavior**.

---

## 2. Adversary Model

Adversaries may have the following capabilities:

- Passive traffic observation
- Active packet injection or modification
- Partial relay compromise
- Traffic flooding (DDoS)
- Man-in-the-middle attempts
- Malicious exit nodes

A global passive adversary is considered out of scope.

---

## 3. Scenario: Man-in-the-Middle (MITM)

### Attack
An attacker attempts to intercept or modify traffic between hops.

### VRP Response
- Per-hop integrity verification detects modification
- Compromised route is marked suspicious
- Client triggers immediate route rotation
- Old keys are destroyed

### Residual Risk
Short-lived metadata leakage before rotation.

---

## 4. Scenario: Relay Compromise

### Attack
An adversary gains control over a relay node.

### VRP Response
- Compromised relay only sees a single hop
- No access to full route or plaintext
- Continuous rotation limits observation window

### Residual Risk
Local traffic rate observation at that hop.

---

## 5. Scenario: Traffic Correlation Attempt

### Attack
Adversary attempts to correlate timing and volume patterns.

### VRP Response
- Route movement breaks long-term correlation
- Independent hop keys prevent replay analysis
- Jitter and timing variance reduce signal quality

### Residual Risk
Short-term correlation remains possible.

---

## 6. Scenario: Distributed Denial of Service (DDoS)

### Attack
Flooding of known relays or paths.

### VRP Response
- Client detects latency and loss anomalies
- Affected route is abandoned
- New route is constructed automatically

### Residual Risk
Temporary service degradation.

---

## 7. Scenario: Malicious Exit Node

### Attack
Exit node attempts traffic inspection or manipulation.

### VRP Response
- Payload integrity is enforced end-to-end
- Suspicious behavior triggers exit rotation
- Exit nodes are treated as untrusted by design

### Residual Risk
Destination-level metadata exposure (inherent to VPNs).

---

## 8. Ransomware or Injection Attempts

### Attack
Injection of malformed or malicious payloads.

### VRP Response
- Cryptographic integrity checks fail
- Packets are dropped
- Session health score degrades
- Rotation is triggered if pattern persists

### Residual Risk
Application-layer vulnerabilities remain out of scope.

---

## 9. Summary

VRP assumes compromise is **inevitable**.

Security is achieved through:
- compartmentalization
- rapid adaptation
- mandatory route movement
- bounded exposure

The protocol prioritizes **survivability over static trust**.
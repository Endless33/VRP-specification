# Threat Model: Veil Routing Protocol (VRP)

## 1. Purpose of This Document

This document defines the **threat model** for the Veil Routing Protocol (VRP) and all implementations based on it (including, but not limited to, Jumping VPN).

The goal of VRP is **not absolute anonymity**, but **adaptive resistance** against network surveillance, traffic manipulation, and targeted attacks through *continuous movement and route instability*.

Anything not explicitly covered in this document **must be considered out of scope**.

---

## 2. Design Philosophy

VRP is based on the following core principles:

- **Movement as Defense**  
  Static routes are considered a liability. Routes are expected to change frequently.

- **Partial Knowledge**  
  No single relay node should know the full routing path.

- **Damage Containment**  
  Compromise of a node, route, or session should have limited temporal and informational impact.

- **Reaction Over Perfection**  
  When certainty is impossible, VRP prioritizes *escape and rerouting* over attempting to defeat the attacker in place.

VRP assumes that **long-lived trust is dangerous**.

---

## 3. Assets to Protect

VRP aims to protect the following assets:

- Confidentiality of user payload data
- Integrity and authenticity of in-transit packets
- Forward secrecy of past sessions
- Resistance to long-term traffic correlation
- Confidentiality of routing paths

---

## 4. Adversary Model

### 4.1 Passive Network Adversary

**Capabilities:**
- Observes network traffic at one or more points
- Performs timing, size, and volume analysis
- Does not modify traffic

**Examples:**
- ISP-level monitoring
- Passive DPI systems

---

### 4.2 Active Network Adversary (MITM)

**Capabilities:**
- Intercepts, modifies, injects, or drops packets
- Performs replay and downgrade attempts
- Impersonates network endpoints

**Examples:**
- Rogue Wi-Fi access points
- Compromised routers
- Malicious proxies

---

### 4.3 Partial Node Compromise

**Capabilities:**
- Controls or compromises a subset of VRP relay nodes
- Observes traffic passing through those nodes
- Can behave honestly or maliciously

**Assumptions:**
- The adversary does **not** control all nodes simultaneously
- Newly selected nodes are not instantly compromised

---

### 4.4 Targeted Disruption Adversary

**Capabilities:**
- Performs denial-of-service (DoS/DDoS) attacks
- Attempts congestion and route exhaustion
- Tries to degrade service quality to force failures

---

### 4.5 Malicious Traffic Injection (Limited)

**Capabilities:**
- Attempts payload injection via network manipulation
- Attempts redirection to malicious endpoints

**Limitations:**
- Endpoint compromise is out of scope
- OS- and application-level security are not guaranteed

---

## 5. Explicit Non-Goals (Out of Scope)

VRP does **not** claim to protect against:

- A global passive adversary observing all network links
- Full endpoint compromise (root access, kernel malware)
- Browser fingerprinting or application-layer tracking
- User deanonymization through behavioral patterns
- Legal coercion or compelled node operation disclosure

VRP must be combined with other tools to mitigate these threats.

---

## 6. Detection Signals (Heuristics)

VRP implementations MAY monitor the following indicators:

- Abnormal latency spikes or jitter
- Packet authentication or integrity failures
- Unexpected key or certificate changes
- Replay or sequencing anomalies
- Sudden throughput collapse indicative of DoS

Detection is **heuristic**, probabilistic, and imperfect.

False positives are acceptable.

---

## 7. Defensive Responses

Upon detecting suspicious or anomalous behavior, VRP MAY:

- Immediately rotate session keys
- Tear down the current route
- Construct a new route using different nodes
- Temporarily increase route length
- Randomize timing and packet scheduling

The primary defense strategy is **escape, not confrontation**.

---

## 8. Route Movement Policy

Routes in VRP are expected to:

- Change periodically even without detected attacks
- Change immediately upon high-confidence anomalies
- Avoid reuse of recently used nodes when possible

Movement frequency is configurable and intentionally unpredictable.

---

## 9. Residual Risks

Despite these defenses, VRP acknowledges:

- Traffic correlation remains possible over short windows
- Frequent movement may increase latency
- Aggressive rerouting may reduce usability

These trade-offs are accepted by design.

---

## 10. Security Posture Summary

VRP prioritizes:

- Short-lived exposure
- Reduced observability
- Adaptive survivability

VRP does **not** promise invisibility.

VRP promises that **staying still is never required**.

---

## 11. Status

This threat model is a **living document**.

Any protocol change MUST be evaluated against this model.

Security claims not supported by this document MUST be considered invalid.
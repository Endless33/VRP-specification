# ==== FILE: spec/threat-model.md ====
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


# ==== FILE: spec/protocol.md ====
# Protocol: Veil Routing Protocol (VRP)

## 1. Network Architecture

VRP consists of three types of nodes:

- **Client Node (C)**: initiates connections, constructs routes, and encrypts payloads.
- **Relay Node (R)**: forwards traffic between nodes without knowledge of the full path.
- **Exit Node (E)**: final node before traffic reaches its destination, decrypts outer layer if necessary.

Each node type has minimal privileges and knowledge to maintain route confidentiality.

---

## 2. Node Roles

### 2.1 Client Node
- Builds initial route through available relays.
- Encrypts payloads using layered encryption (like onion routing).
- Monitors route health and triggers reroute on anomalies.

### 2.2 Relay Node
- Forwards encrypted packets without knowing source or destination.
- Validates packet integrity.
- Logs minimal telemetry only for debugging (optional, non-identifying).

### 2.3 Exit Node
- Decrypts outermost encryption layer.
- Forwards payload to final destination.
- Cannot reconstruct full client route.

---

## 3. Route Construction

1. **Node Selection:**
   - Client randomly selects a sequence of relay nodes from trusted pool.
   - Avoid recently used nodes to minimize correlation.

2. **Route Length:**
   - Configurable minimum and maximum length.
   - Longer routes increase anonymity; shorter routes reduce latency.

3. **Route Setup:**
   - Client negotiates ephemeral session keys with each node.
   - Keys are derived using X25519/Noise protocol framework.
   - Routes are committed once all handshakes succeed.

4. **Packet Forwarding:**
   - Multi-layer encryption ensures each node can only decrypt its own layer.
   - Packets carry sequence numbers and integrity tags.

---

## 4. Session Lifecycle

1. **Session Initiation:**
   - Client generates ephemeral key pair.
   - Negotiates keys with relays.
   - Establishes first route and begins sending traffic.

2. **Monitoring & Heuristics:**
   - Client measures latency, jitter, integrity failures, and anomalies.
   - Suspicious patterns trigger reroute.

3. **Route Rotation:**
   - Routes are rotated periodically even without anomalies.
   - On detection of high-confidence anomalies, client immediately tears down current route.

4. **Session Termination:**
   - Session keys are destroyed.
   - Relays forget ephemeral session information.
   - Any leftover buffered packets are discarded.

---

## 5. Node Knowledge

- **Client:** knows full route and session keys.
- **Relay:** knows only previous and next hop; sees encrypted payload.
- **Exit Node:** sees payload if decrypted, knows immediate previous hop.

No node can reconstruct the entire client path.

---

## 6. Route Movement Policy

- **Periodic Movement:** routes change at configured intervals to reduce long-term exposure.
- **Reactive Movement:** routes change immediately upon detection of anomalies (latency spikes, MITM, DDoS signals, ransomware injection attempts).
- **Randomization:** node selection is pseudo-random to prevent prediction.
- **Reuse Limitation:** recently used nodes are avoided to prevent correlation attacks.

---

## 7. Residual Risks

- Traffic correlation remains possible over short windows.
- Frequent route changes may increase latency.
- False positives in anomaly detection may cause unnecessary reroutes.

These trade-offs are intentional for increased security and survivability.

---

## 8. Summary

VRP prioritizes **continuous movement, adaptive route selection, and partial knowledge per node**.  
The protocol ensures that no static route exists long enough for sustained traffic analysis and that clients can escape potentially compromised paths automatically.


# ==== FILE: spec/crypto.md ====
# Cryptography: Veil Routing Protocol (VRP)

## 1. Purpose

This document defines the cryptographic primitives and operations used in the Veil Routing Protocol (VRP).  
It ensures **confidentiality, integrity, authenticity**, and **forward secrecy** across all routes and sessions.

All implementations must follow these specifications to be compliant with the VRP protocol.

---

## 2. Key Exchange

1. **Ephemeral Key Pairs**
   - Each client generates an ephemeral X25519 key pair per session.
   - Relay nodes generate ephemeral X25519 key pairs for each incoming session.

2. **Noise Protocol Framework**
   - Used for secure handshake between client and relays.
   - Provides forward secrecy, mutual authentication, and protection against MITM.

3. **Session Key Derivation**
   - Keys for each hop are derived using HKDF-SHA256.
   - Each hop has a unique encryption key, not shared with other nodes.

---

## 3. Packet Encryption

1. **Layered Encryption (Onion-like)**
   - Each packet is wrapped in multiple layers, one per hop.
   - Only the intended node can decrypt its layer.

2. **Algorithms**
   - ChaCha20-Poly1305 (default) or AES-GCM (optional).
   - Nonces must never repeat within a session.

3. **Integrity and Authentication**
   - Each hop verifies HMAC or authentication tag before forwarding.
   - Signature verification using Ed25519 for critical control packets.

---

## 4. Key Rotation and Route Changes

- When a route is rotated or an anomaly is detected:
  - Session keys for affected route segments are immediately destroyed.
  - New ephemeral keys are negotiated with new relay nodes.
  - Forward secrecy ensures past traffic remains secure.

- Clients may precompute future keys for upcoming routes to minimize downtime.

---

## 5. Packet Format

Each packet contains the following fields:

| Field        | Description                                    | Encryption/Signature |
|-------------|-----------------------------------------------|-------------------|
| Header      | Routing information (previous/next hop)      | Partially encrypted|
| Nonce       | Unique per packet                              | Used in encryption|
| Payload     | Encrypted user data                            | Fully encrypted   |
| Auth Tag    | Integrity and authentication                  | Authenticated     |

- Only the header relevant to a node is decrypted at that node.
- Payload remains encrypted until the exit node.

---

## 6. Optional Advanced Features

- **Post-Quantum Key Exchange:** for future-proofing, VRP may optionally support Kyber or other lattice-based algorithms.
- **Traffic Padding and Randomization:** packets may be padded to reduce size-based correlation.
- **Adaptive Encryption Parameters:** clients may vary nonce and packet sizes based on route heuristics to further reduce observability.

---

## 7. Security Posture Summary

VRP cryptography ensures:

- **Confidentiality:** only intended recipient can read the payload.
- **Integrity:** any modification is detected.
- **Forward Secrecy:** compromise of current keys does not reveal past traffic.
- **Partial Node Knowledge:** no single node can reconstruct the full path.
- **Adaptive Protection:** keys rotate on movement or anomalies to reduce exposure.

All implementations must strictly follow these specifications for security compliance.


# ==== FILE: spec/movement-policy.md ====
# Movement Policy: Veil Routing Protocol (VRP)

## 1. Purpose

This document defines the **route movement policy** for VRP.  
The goal is to ensure **continuous mobility, adaptive response to anomalies**, and **reduced exposure to attacks** such as MITM, DDoS, or ransomware injection attempts.

All implementations must adhere to these guidelines to maintain protocol compliance.

---

## 2. Periodic Movement

- Routes should be rotated at configurable intervals (e.g., every 5–15 minutes).
- Movement interval is randomized within the configured window to prevent timing attacks.
- Even without detected anomalies, routes must change to reduce long-term correlation opportunities.

---

## 3. Reactive Movement

Routes must change **immediately** upon detection of suspicious behavior:

- **Latency spikes or jitter anomalies**
- **Integrity failures or unexpected packet modification**
- **MITM detection signals**
- **DDoS traffic patterns**
- **Ransomware or suspicious payload injection attempts**

Reactive movement ensures the client **escapes potentially compromised routes**.

---

## 4. Node Selection and Randomization

- Nodes are selected pseudo-randomly from a trusted relay pool.
- Recently used nodes should be avoided to minimize correlation attacks.
- Route lengths vary within a configured range to prevent predictability.
- Randomized timing and padding may be applied to further obscure traffic patterns.

---

## 5. Route Rotation Algorithm (Simplified Example)

```python
# Pseudo-code for route rotation logic

def select_new_route(client, current_route, relay_pool, recent_routes):
    candidates = [node for node in relay_pool if node not in recent_routes]
    new_route = random.sample(candidates, k=desired_route_length)
    return new_route

def monitor_and_rotate(client, route):
    while session_active:
        if detect_anomaly(route):
            new_route = select_new_route(client, route, relay_pool, recent_routes)
            client.switch_to_route(new_route)
        elif time_for_periodic_rotation():
            new_route = select_new_route(client, route, relay_pool, recent_routes)
            client.switch_to_route(new_route)
        sleep(monitor_interval)

detect_anomaly(route) implements heuristic checks (latency, packet integrity, MITM signals, DDoS patterns).
recent_routes prevents immediate reuse.
monitor_interval controls detection frequency.
6. Residual Risks
False positives may trigger unnecessary route changes.
Aggressive movement can increase latency.
Limited relay pool may reduce route randomness.
Trade-offs are intentional to maximize adaptive survivability.
7. Summary
The VRP movement policy ensures that:
Routes never remain static long enough for sustained traffic analysis.
Clients escape detected anomalies automatically.
Node selection and timing are unpredictable.
Security is adaptive and survivable, not static or fragile.
All implementations must follow these guidelines to maintain VRP security compliance.
==== FILE: README.md ====
VRP Specification
Formal specification of the Veil Routing Protocol (VRP).
This repository contains the official documentation defining the VRP protocol, including its architecture, routing behavior, cryptography, movement policies, and threat model.
Overview
The VRP protocol is designed for adaptive, resilient, and secure networking. It focuses on:
Protecting user data confidentiality and integrity
Maintaining forward secrecy
Minimizing exposure to traffic analysis and correlation attacks
Enabling continuous movement and adaptive route changes in response to anomalies (MITM, DDoS, suspicious payloads)
VRP is not a silver bullet for all anonymity; its threat model is explicitly defined in threat-model.md.
Repository Structure

spec/       — official specification documents
    threat-model.md       — defines adversaries, assets, and security goals
    protocol.md           — defines route construction, node roles, session lifecycle
    crypto.md             — defines key exchange, encryption, authentication, packet format
    movement-policy.md    — defines route rotation, anomaly response, movement heuristics

drafts/     — experimental notes, early proposals, and ideas

Status
The VRP specification is actively developed and structured for formal release.
Documents are living specifications: updates and revisions are expected as protocol design evolves.
Implementations should reference these documents to ensure compliance.
References in Implementations
Implementations like jumping-vpn, vrp-sdk, and vrp-demo must link to the official specification documents rather than duplicating them:

- Threat Model: spec/threat-model.md
- Protocol: spec/protocol.md
- Crypto: spec/crypto.md
- Movement Policy: spec/movement-policy.md

This ensures a single source of truth and maintains consistency across implementations.
Contributing
Contributions are welcome via pull requests in drafts/ or proposals for spec/ updates.
Any cryptography or protocol changes must be reviewed against threat-model.md and documented clearly.
License

[Specify license here, e.g., MIT, Apache 2.0, etc.]
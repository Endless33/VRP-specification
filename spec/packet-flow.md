# Packet Flow Walkthrough — VRP

## 1. Overview

This document describes the **step-by-step lifecycle of a single packet**
as it travels through a VRP route.

The goal is to clearly define:
- where encryption occurs
- where integrity is verified
- where anomaly detection is applied
- how route movement is triggered

---

## 2. Route Structure

A VRP route consists of:

Client → Entry Relay → Transit Relay(s) → Exit Relay → Destination

Each hop maintains **independent ephemeral keys**.

---

## 3. Packet Creation (Client Side)

1. Application generates plaintext payload
2. Payload is wrapped in a VRP packet structure
3. Multiple encryption layers are applied (onion-style)
4. Integrity tags are computed per hop
5. Packet is sent to Entry Relay

At no point does a single relay have access to:
- plaintext payload
- full route topology
- client identity

---

## 4. Entry Relay Processing

Upon receiving a packet, the Entry Relay:

1. Verifies integrity tag
2. Decrypts its encryption layer
3. Updates hop-specific metadata
4. Forwards packet to next relay

If verification fails:
- packet is dropped
- anomaly counters are incremented

---

## 5. Transit Relay Processing

Transit relays repeat the same process:

- Verify integrity
- Decrypt one layer
- Forward to next hop

Transit relays:
- cannot identify client or destination
- cannot correlate packets across rotations

---

## 6. Exit Relay Processing

The Exit Relay:

1. Decrypts final encryption layer
2. Extracts destination address
3. Forwards payload to destination

Responses from destination follow the reverse encrypted path.

---

## 7. Anomaly Detection Points

Anomaly detection occurs at:

- Client (global session view)
- Entry Relay (local packet behavior)
- Transit Relays (rate and integrity anomalies)

Detected anomalies include:
- repeated integrity failures
- timing irregularities
- packet loss patterns
- latency spikes

---

## 8. Route Movement Trigger

When anomaly thresholds are exceeded:

1. Client transitions session to ROTATING state
2. New route is constructed in parallel
3. Traffic is switched atomically
4. Old route keys are destroyed

This minimizes exposure and prevents long-term correlation.

---

## 9. Security Properties

- No packet travels unencrypted
- No relay can observe full path
- Route movement limits attack dwell time
- Packet loss does not reveal session state

---

## 10. Summary

VRP packet flow prioritizes **confidentiality, compartmentalization,
and rapid adaptation** in hostile network environments.
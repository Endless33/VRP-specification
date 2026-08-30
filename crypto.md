Cryptography: Veil Routing Protocol (VRP)

1. Purpose

This document defines the cryptographic primitives and operations used in the Veil Routing Protocol (VRP).
It ensures confidentiality, integrity, authenticity, and forward secrecy across all routes and sessions.

All implementations must follow these specifications to be compliant with the VRP protocol.


---

2. Key Exchange

1. Ephemeral Key Pairs

Each client generates an ephemeral X25519 key pair per session.

Relay nodes generate ephemeral X25519 key pairs for each incoming session.



2. Noise Protocol Framework

Used for secure handshake between client and relays.

Provides forward secrecy, mutual authentication, and protection against MITM.



3. Session Key Derivation

Keys for each hop are derived using HKDF-SHA256.

Each hop has a unique encryption key, not shared with other nodes.





---

3. Packet Encryption

1. Layered Encryption (Onion-like)

Each packet is wrapped in multiple layers, one per hop.

Only the intended node can decrypt its layer.



2. Algorithms

ChaCha20-Poly1305 (default) or AES-GCM (optional).

Nonces must never repeat within a session.



3. Integrity and Authentication

Each hop verifies HMAC or authentication tag before forwarding.

Signature verification using Ed25519 for critical control packets.





---

4. Key Rotation and Route Changes

When a route is rotated or an anomaly is detected:

Session keys for affected route segments are immediately destroyed.

New ephemeral keys are negotiated with new relay nodes.

Forward secrecy ensures past traffic remains secure.


Clients may precompute future keys for upcoming routes to minimize downtime.



---

5. Packet Format

Each packet contains the following fields:

Field	Description	Encryption/Signature

Header	Routing information (previous/next hop)	Partially encrypted
Nonce	Unique per packet	Used in encryption
Payload	Encrypted user data	Fully encrypted
Auth Tag	Integrity and authentication	Authenticated


Only the header relevant to a node is decrypted at that node.

Payload remains encrypted until the exit node.



---

6. Optional Advanced Features

Post-Quantum Key Exchange: for future-proofing, VRP may optionally support Kyber or other lattice-based algorithms.

Traffic Padding and Randomization: packets may be padded to reduce size-based correlation.

Adaptive Encryption Parameters: clients may vary nonce and packet sizes based on route heuristics to further reduce observability.



---

7. Security Posture Summary

VRP cryptography ensures:

Confidentiality: only intended recipient can read the payload.

Integrity: any modification is detected.

Forward Secrecy: compromise of current keys does not reveal past traffic.

Partial Node Knowledge: no single node can reconstruct the full path.

Adaptive Protection: keys rotate on movement or anomalies to reduce exposure.


All implementations must strictly follow these specifications for security compliance.
Protocol: Veil Routing Protocol (VRP)

1. Network Architecture

VRP consists of three types of nodes:

Client Node (C): initiates connections, constructs routes, and encrypts payloads.

Relay Node (R): forwards traffic between nodes without knowledge of the full path.

Exit Node (E): final node before traffic reaches its destination, decrypts outer layer if necessary.


Each node type has minimal privileges and knowledge to maintain route confidentiality.


---

2. Node Roles

2.1 Client Node

Builds initial route through available relays.

Encrypts payloads using layered encryption (like onion routing).

Monitors route health and triggers reroute on anomalies.


2.2 Relay Node

Forwards encrypted packets without knowing source or destination.

Validates packet integrity.

Logs minimal telemetry only for debugging (optional, non-identifying).


2.3 Exit Node

Decrypts outermost encryption layer.

Forwards payload to final destination.

Cannot reconstruct full client route.



---

3. Route Construction

1. Node Selection:

Client randomly selects a sequence of relay nodes from trusted pool.

Avoid recently used nodes to minimize correlation.



2. Route Length:

Configurable minimum and maximum length.

Longer routes increase anonymity; shorter routes reduce latency.



3. Route Setup:

Client negotiates ephemeral session keys with each node.

Keys are derived using X25519/Noise protocol framework.

Routes are committed once all handshakes succeed.



4. Packet Forwarding:

Multi-layer encryption ensures each node can only decrypt its own layer.

Packets carry sequence numbers and integrity tags.





---

4. Session Lifecycle

1. Session Initiation:

Client generates ephemeral key pair.

Negotiates keys with relays.

Establishes first route and begins sending traffic.



2. Monitoring & Heuristics:

Client measures latency, jitter, integrity failures, and anomalies.

Suspicious patterns trigger reroute.



3. Route Rotation:

Routes are rotated periodically even without anomalies.

On detection of high-confidence anomalies, client immediately tears down current route.



4. Session Termination:

Session keys are destroyed.

Relays forget ephemeral session information.

Any leftover buffered packets are discarded.





---

5. Node Knowledge

Client: knows full route and session keys.

Relay: knows only previous and next hop; sees encrypted payload.

Exit Node: sees payload if decrypted, knows immediate previous hop.


No node can reconstruct the entire client path.


---

6. Route Movement Policy

Periodic Movement: routes change at configured intervals to reduce long-term exposure.

Reactive Movement: routes change immediately upon detection of anomalies (latency spikes, MITM, DDoS signals, ransomware injection attempts).

Randomization: node selection is pseudo-random to prevent prediction.

Reuse Limitation: recently used nodes are avoided to prevent correlation attacks.



---

7. Residual Risks

Traffic correlation remains possible over short windows.

Frequent route changes may increase latency.

False positives in anomaly detection may cause unnecessary reroutes.


These trade-offs are intentional for increased security and survivability.


---

8. Summary

VRP prioritizes continuous movement, adaptive route selection, and partial knowledge per node.
The protocol ensures that no static route exists long enough for sustained traffic analysis and that clients can escape potentially compromised paths automatically.
# VRP Public Security Assurance

**Project:** Veil Routing Protocol (VRP)  
**Document Type:** Public Security Assurance  
**Status:** Public  
**Scope:** Architecture, Runtime Guarantees, Validation and Deployment Assurance  
**Revision:** October 2026

---

## Purpose

This document summarizes the public security properties of Veil Routing Protocol (VRP).

It focuses on properties that can be described, exercised, and evaluated externally.

The objective is not to present security as a marketing label.

The objective is to identify concrete runtime guarantees and the validation evidence associated with them.

VRP security is built around preserving:

- session identity;
- canonical state;
- lifecycle authority;
- message integrity;
- replay resistance;
- authority continuity;
- evidence integrity;
- fail-closed behavior.

---

# 1. Security Position

VRP does not assume that the underlying network is trustworthy or stable.

The runtime is designed for environments where it may encounter:

- transport failure;
- path changes;
- network rebinding;
- duplicated delivery;
- delayed delivery;
- reordered delivery;
- replay;
- stale lifecycle work;
- stale authority;
- invalid state transitions;
- restart races;
- contradictory runtime observations;
- evidence modification.

The security objective is to prevent those conditions from silently redefining canonical session state or current authority.

---

# 2. Core Security Invariant

The central VRP architectural property is:

> **Session ≠ Transport**

This is also a security boundary.

A transport becoming available does not automatically make it authoritative.

A transport disappearing does not automatically destroy logical session identity.

A new path does not automatically create a new canonical session.

A previously valid path does not automatically remain valid forever.

The session lifecycle remains the higher-level continuity boundary.

---

# 3. Security Properties

The public VRP security model includes the following properties:

    SESSION AUTHENTICITY

    SESSION CONTINUITY

    MESSAGE INTEGRITY

    REPLAY RESISTANCE

    LIFETIME ISOLATION

    AUTHORITY VALIDATION

    TRANSPORT INDEPENDENCE

    CANONICAL STATE VALIDATION

    EVIDENCE INTEGRITY

    FAIL-CLOSED ADMISSION

These properties are evaluated through engineering validation rather than being treated solely as design statements.

---

# 4. Session Identity

VRP treats session identity as distinct from transport identity.

This protects against a class of architectural errors where a network path, socket, or transport replacement is accidentally interpreted as a new logical session or as automatic authority over an existing session.

Validation has exercised session continuity across:

- transport interruption;
- transport replacement;
- path migration;
- relay migration;
- network rebinding;
- repeated migration;
- recovery.

---

# 5. Lifecycle Identity

A logical session identifier may exist across multiple lifetimes.

VRP therefore distinguishes current lifecycle identity from historical lifecycle identity.

The public security requirement is:

> Historical work must not become current merely because the logical session identifier matches.

This property has dedicated regression and stress coverage.

---

# 6. Multi-Lifetime Isolation

A dedicated validation exercised:

    TOTAL_INCARNATIONS=128

    HISTORICAL_OPERATIONS=127

    HISTORICAL_ACCEPTED=0

    HISTORICAL_REJECTED=127

The current lifetime remained authoritative.

This demonstrates the intended separation between logical naming and current lifecycle authority.

---

# 7. Stale Work Rejection

Delayed work is a normal distributed-systems condition.

A previously legitimate operation may become stale before it arrives.

VRP therefore validates work against the current lifecycle rather than assuming that historical validity implies current validity.

Expected result:

    STALE_WORK
        |
        v
    CURRENT LIFECYCLE VALIDATION
        |
        X
    REJECTED

---

# 8. Replay Resistance

Replay resistance is an explicit runtime security property.

Validation has exercised:

- duplicate delivery;
- stale sequence delivery;
- repeated replay;
- replay flooding;
- historical lifecycle replay;
- evidence replay scenarios.

A recorded validation exercised:

    REPLAY_ATTEMPTS=9999

    REPLAY_REJECTED=9999

    REPLAY_ACCEPTED=0

The current session identity remained preserved.

---

# 9. Duplicate Execution Resistance

Replay resistance is not limited to identifying identical network packets.

At the architectural level, the important property is that already accepted work must not silently become additional canonical work when presented again through a rejected duplicate path.

Conceptually:

    VALID OPERATION
          |
          v
    ACCEPTED ONCE

followed by:

    DUPLICATE / REPLAY
          |
          X
    NOT ACCEPTED AS NEW CANONICAL WORK

---

# 10. Canonical State Validation

VRP maintains an explicit canonical session lifecycle.

Events are evaluated against the current canonical state.

An invalid event is rejected.

The public invariant is:

    INVALID EVENT
         |
         X
    NO CANONICAL STATE CHANGE

Regression coverage additionally verifies:

    NO ACCEPTED HISTORY CHANGE

and:

    NO CANONICAL EVENTLOG CHANGE

for rejected state events.

---

# 11. Canonical Admission Atomicity

Accepted and rejected operations must not leave partially contradictory canonical records.

Dedicated regression coverage validates:

For accepted work:

    EXPECTED STATE CHANGE
    HISTORY DELTA = 1
    CANONICAL EVENT DELTA = 1

For rejected work:

    STATE DELTA = 0
    HISTORY DELTA = 0
    CANONICAL EVENT DELTA = 0

This protects consistency between canonical state and canonical evidence.

---

# 12. Duplicate Session Creation

Duplicate creation attempts have dedicated validation.

The security requirement is that an existing canonical session cannot silently acquire a competing lifecycle merely because creation is requested again.

Validation confirms preservation of:

- existing session state;
- lifecycle identity;
- canonical history.

---

# 13. Terminal State Protection

Terminal session states are treated as explicit lifecycle boundaries.

Validated terminal states include:

    SESSION_CLOSED

and:

    SESSION_FAILED

Unsupported attempts to resurrect these states through ordinary session events are rejected.

Validation confirms no mutation of:

- terminal canonical state;
- accepted transition history;
- canonical event history.

---

# 14. Fail-Closed Admission

VRP uses fail-closed canonical admission.

Representative behavior:

    missing lifecycle identity
        -> REJECTED

    stale lifetime
        -> REJECTED

    stale incarnation
        -> REJECTED

    invalid transition
        -> REJECTED

    replay
        -> REJECTED

    unauthorized authority
        -> REJECTED

Invalid or incomplete context is not interpreted as implicit permission.

---

# 15. Authority Separation

VRP distinguishes:

    CONNECTIVITY

from:

    AUTHORITY

A node or transport becoming reachable does not automatically become authorized to alter canonical session state.

Authority validation has exercised both accepted and rejected transitions.

---

# 16. Unauthorized Authority Rejection

A validated authority scenario included an operation from an unauthorized requester.

Observed result:

    REQUESTER=node-X

    RESULT=REJECTED

A valid authority path subsequently completed the accepted transition.

This demonstrates that network reachability alone is insufficient to establish canonical authority.

---

# 17. Authority Generation

Authority progression is represented as an ordered lineage.

A validated scenario observed:

    INITIAL_GENERATION=1

followed by:

    FINAL_GENERATION=2

after an accepted transition.

Historical generation state was not treated as equivalent to current authority.

---

# 18. Authority Rollback Resistance

VRP validation includes stale authority and rollback scenarios.

The public security property is:

> An older authority state must not silently replace newer accepted authority.

Validation has exercised:

- stale authority;
- rollback attempts;
- restart races;
- historical authority observations;
- contradictory authority information.

---

# 19. Authority Recovery

Authority recovery must preserve accepted lineage rather than merely restore connectivity.

Validation has exercised recovery scenarios involving:

- transport changes;
- authority progression;
- stale authority rejection;
- runtime restart conditions.

The objective is one coherent accepted authority history.

---

# 20. Transport Migration Security

Migration is security-sensitive because the active transport changes while session identity remains stable.

A migration must therefore preserve the distinction between:

    SESSION IDENTITY

and:

    TRANSPORT IDENTITY

A recorded validation completed:

    1,000 migrations

while preserving session identity.

Migration does not imply unrestricted authority transfer.

---

# 21. Physical Transport Failure

Security guarantees must remain meaningful when the network actually fails.

A recorded physical validation observed:

    TRANSPORT_REACHABILITY=LOST

followed by:

    TRANSPORT_REACHABILITY=RECOVERED

with an observed interruption of approximately:

    41.6 seconds

The continuity model remained logically distinct from the failed transport.

---

# 22. Recovery Security

Recovery is not permission to bypass canonical validation.

During recovery:

- stale work remains stale;
- replay remains replay;
- invalid transitions remain invalid;
- unauthorized authority remains unauthorized.

The recovery path remains subject to lifecycle and authority rules.

---

# 23. Mutable Boundary Security

Runtime APIs can create security problems if read-oriented interfaces accidentally expose mutable canonical state.

VRP engineering review has therefore examined mutable boundary surfaces.

Reviewed areas include:

- managed session representations;
- event logs;
- event metadata;
- authority representations;
- path-health data;
- relay collections;
- transport collections;
- runtime composition surfaces.

Where unintended mutable exposure was identified, it was corrected and covered by regression tests.

---

# 24. Session Read Isolation

Managed session read representations are isolated from canonical runtime state.

External mutation of a returned read representation does not directly mutate the canonical session.

This prevents an ordinary read path from becoming an unintended canonical state mutation path.

---

# 25. EventLog Isolation

Canonical event history has dedicated mutable-boundary protection.

A caller receiving an EventLog representation cannot directly erase or append canonical runtime history merely by mutating the returned representation.

Regression coverage verifies this property.

---

# 26. Event Metadata Isolation

Supported event metadata values associated with canonical history have dedicated isolation coverage.

Caller-side mutation of returned metadata does not directly rewrite canonical stored metadata.

This protects the evidentiary relationship between accepted events and later reads.

---

# 27. Path Health Isolation

Path-health read surfaces have been reviewed for mutation isolation.

Validated read surfaces include:

    Get
    All
    History

Returned read values do not provide an unintended direct mutation path into canonical path-health storage.

---

# 28. Relay Collection Isolation

Returned relay-list containers are isolated from the selector's internal collection.

Changing the returned collection does not directly rewrite the canonical selector list.

---

# 29. Transport Identity Semantics

Transport objects are active runtime entities and may intentionally retain runtime identity.

VRP explicitly distinguishes:

    ISOLATED COLLECTION

from:

    CLONED TRANSPORT OBJECT

The transport collection boundary has been tested so that container mutation does not silently rewrite registry structure while intentional transport identity semantics remain preserved.

---

# 30. Runtime Composition Boundary

Some runtime components intentionally preserve shared identity because they are composition surfaces rather than detached read snapshots.

Regression validation confirms:

    MANAGER IDENTITY=INTENTIONALLY SHARED

    AUTHORITY IDENTITY=INTENTIONALLY SHARED

while:

    UNBOUND CANONICAL EVENT=REJECTED

This demonstrates that intentional component sharing does not automatically remove lifecycle admission checks.

---

# 31. Message Integrity

VRP's public security model requires accepted protected messages to preserve integrity.

The security objective is that modification of protected message content must not silently become valid accepted content.

Message integrity is part of the runtime security model alongside:

- session identity;
- lifecycle identity;
- authority;
- replay resistance.

Environment-specific cryptographic review should evaluate the concrete protocol profile used by the deployment.

---

# 32. Cryptographic Boundary

VRP security claims should distinguish architectural properties from cryptographic implementation claims.

The public architecture includes cryptographically protected communication and evidence-integrity mechanisms.

However:

    CRYPTOGRAPHIC HASH
        !=
    PRODUCER AUTHENTICATION

and:

    ENCRYPTION
        !=
    COMPLETE PROTOCOL SECURITY

and:

    VALID EVIDENCE HASH
        !=
    FORMAL PROOF OF RUNTIME CORRECTNESS

Each property must be evaluated according to its actual purpose.

---

# 33. Evidence Integrity

VRP validation includes integrity checking of generated evidence.

Evidence may include:

- event observations;
- summaries;
- manifests;
- hashes;
- verification results.

Validation has included deliberate modification of evidence inputs.

Inconsistent evidence was detected in the corresponding exercised scenarios.

---

# 34. Coordinated Evidence Modification

Evidence testing has included scenarios where multiple related evidence components were modified.

This is important because an integrity model should not rely only on checking one isolated value.

The tested verification workflow detected inconsistency across coordinated evidence modifications.

---

# 35. Historical Evidence Integrity

VRP has exercised historical rewrite scenarios.

The objective is to detect when established evidence history has been changed inconsistently.

Expected behavior:

    ESTABLISHED HISTORY
           |
           v
    MODIFICATION
           |
           v
    VERIFICATION
           |
           X
    INCONSISTENCY DETECTED

---

# 36. Independent Verification

Selected VRP security properties can be evaluated through public evidence surfaces.

An independent verifier may evaluate properties such as:

- evidence consistency;
- replay rejection counts;
- stale-work rejection;
- lifecycle progression;
- authority progression;
- migration outcomes;
- historical integrity;
- final public verdict.

This verification boundary does not require unrestricted access to protected runtime implementation details.

---

# 37. Independent Verification Scope

Independent verification has a defined scope.

It verifies properties represented by the exposed evidence.

It does not automatically establish:

- correctness of every runtime instruction;
- absence of every implementation defect;
- security of every deployment configuration;
- correctness under every possible network;
- complete formal verification of the runtime.

The scope of each verifier should therefore remain explicit.

---

# 38. Formal Verification Position

VRP uses engineering validation including:

- invariant-oriented tests;
- deterministic regression;
- concurrency tests;
- race detection;
- failure injection;
- long-duration testing;
- evidence verification.

Unless a specific artifact explicitly states otherwise, VRP does not claim complete formal proof of the protected runtime using TLA+, Coq, Lean, Isabelle, or an equivalent formal verification system.

This distinction is important.

Engineering evidence and mathematical proof are not interchangeable terms.

---

# 39. Zero Trust Interpretation

VRP supports Zero Trust-oriented operation in the sense that network presence alone is not treated as sufficient authority.

Examples include:

    reachable != authorized

    historical != current

    duplicated != new

    transport identity != session identity

    observed event != canonical event

The exact organizational Zero Trust architecture remains deployment-specific.

---

# 40. Network Trust

VRP does not require the network path itself to be treated as a source of canonical authority.

The runtime must tolerate the possibility that the network can:

- fail;
- reorder;
- delay;
- duplicate;
- change path;
- change transport.

Security decisions therefore depend on higher-level validation rather than transport continuity alone.

---

# 41. Concurrency Security

Concurrency can create state-integrity failures even without a network attacker.

VRP security qualification therefore includes concurrency testing.

Validation has included:

- concurrent session operations;
- packet identifier generation;
- historical delayed work;
- repeated reincarnation;
- path activity;
- deterministic concurrency tests.

Selected scenarios have also been repeatedly exercised under Go race detection.

---

# 42. Deterministic Concurrency

A deterministic concurrency scenario was repeated:

    1,000 executions

with consistent successful results.

This provides evidence for the tested concurrency behavior.

It is not represented as exhaustive proof of every possible scheduler interleaving.

---

# 43. Resource Pressure

Security behavior should not disappear when the runtime is under pressure.

VRP has been exercised under:

- jitter;
- latency variation;
- packet reordering;
- congestion;
- repeated migration;
- resource-bounded execution.

The expected result is preservation of defined invariants or controlled failure containment.

---

# 44. Long-Duration Security

Some state-integrity failures appear only after extended execution.

VRP validation therefore includes long-duration scenarios.

Engineering runs have included:

- prolonged network turbulence;
- repeated transport changes;
- extended disconnection;
- week-scale harness execution.

The recorded runs preserved the evaluated invariants under their tested conditions.

---

# 45. Security Failure Is Evidence

A security-oriented regression that fails is not automatically a failed engineering process.

If a boundary defect is discovered and reproducibly demonstrated, the correct process is:

    REPRODUCE
       |
       v
    ISOLATE
       |
       v
    CORRECT
       |
       v
    ADD REGRESSION
       |
       v
    REQUALIFY

Recent VRP engineering work has followed this process for canonical and mutable-boundary defects.

---

# 46. Regression Preservation

When a security-relevant defect is corrected, dedicated regression coverage should remain.

This prevents the engineering record from becoming:

    defect discovered
        ->
    patch applied
        ->
    knowledge lost

Instead:

    defect discovered
        ->
    reproducible regression
        ->
    correction
        ->
    regression retained

This approach turns discovered defects into permanent validation coverage.

---

# 47. Public Security Claims

The current public engineering record supports the following claims under tested conditions:

    SESSION / TRANSPORT SEPARATION          VALIDATED

    SESSION LIFETIME ISOLATION              VALIDATED

    STALE WORK REJECTION                    VALIDATED

    REPLAY REJECTION                        VALIDATED

    INVALID TRANSITION REJECTION            VALIDATED

    TERMINAL RESURRECTION REJECTION         VALIDATED

    FAIL-CLOSED CANONICAL ADMISSION         VALIDATED

    UNAUTHORIZED AUTHORITY REJECTION        VALIDATED

    AUTHORITY ROLLBACK REJECTION            VALIDATED

    AUTHORITY LINEAGE RECOVERY              VALIDATED

    CANONICAL EVENT CONSISTENCY             VALIDATED

    MANAGED SESSION READ ISOLATION          VALIDATED

    EVENTLOG READ ISOLATION                 VALIDATED

    EVENT METADATA ISOLATION                VALIDATED

    PATH-HEALTH READ ISOLATION              VALIDATED

    RELAY-LIST ISOLATION                    VALIDATED

    TRANSPORT REGISTRY SEMANTICS            VALIDATED

    RUNTIME COMPOSITION BOUNDARY            VALIDATED

    EVIDENCE MODIFICATION DETECTION         VALIDATED

    HISTORICAL REWRITE DETECTION            VALIDATED

    INDEPENDENT EVIDENCE VERIFICATION       VALIDATED

---

# 48. Security Claims That Remain Environment Specific

Some security properties cannot be meaningfully reduced to one universal project-level result.

A deployment must independently evaluate:

- operational key management;
- deployment credentials;
- infrastructure access;
- host hardening;
- transport configuration;
- monitoring;
- incident response;
- deployment topology;
- organizational trust boundaries.

These properties belong to the actual deployment environment.

---

# 49. Security Review Expectations

An external security reviewer should challenge concrete properties.

Useful questions include:

    Can stale lifecycle work become current?

    Can replay become new canonical work?

    Can unauthorized authority become current authority?

    Can rejected work enter canonical history?

    Can a read API rewrite canonical state?

    Can evidence be modified without detection?

    Can transport replacement silently replace session identity?

    Can historical authority overwrite newer authority?

These questions are directly testable at the appropriate public validation boundary.

---

# 50. Security Review Evidence

A useful review should identify:

- evaluated build;
- evaluated environment;
- scenario;
- expected invariant;
- actual observation;
- evidence;
- final result.

A security statement without identifiable scope should not be treated as equivalent to an executed validation.

---

# 51. Deployment Security Gate

Before controlled operational deployment, the organization should evaluate at minimum:

    SESSION IDENTITY                  PASS

    LIFETIME ISOLATION               PASS

    REPLAY REJECTION                 PASS

    STALE-WORK REJECTION             PASS

    AUTHORITY VALIDATION             PASS

    CANONICAL ADMISSION              PASS

    RECOVERY                         PASS

    EVIDENCE VERIFICATION            PASS

Environment-specific security requirements may add additional mandatory gates.

---

# 52. What VRP Does Not Claim

VRP does not claim:

- that networks cannot fail;
- that software can contain no defects;
- that every possible security condition has been exhausted;
- that engineering testing equals complete formal proof;
- that evidence hashes alone establish producer authenticity;
- that a successful pilot certifies every other deployment;
- that protected implementation secrecy replaces cryptographic security;
- that independent verification removes all trust assumptions.

These limitations are part of an accurate public security model.

---

# 53. Security Assurance Position

The current VRP security position is based on a combination of:

    ARCHITECTURAL INVARIANTS
             +
    IMPLEMENTED RUNTIME
             +
    ADVERSARIAL REGRESSION
             +
    FAILURE VALIDATION
             +
    CONCURRENCY QUALIFICATION
             +
    EVIDENCE VERIFICATION
             =
    ENGINEERING SECURITY ASSURANCE

This is not a substitute for environment-specific security review.

It is the engineering foundation on which that review can operate.

---

# 54. Overall Assessment

VRP has an implemented security model centered on preserving canonical session and authority state while the underlying transport environment changes.

The runtime has been exercised against conditions including:

- replay;
- stale work;
- invalid lifecycle operations;
- duplicate creation;
- terminal resurrection attempts;
- unauthorized authority transitions;
- authority rollback;
- transport failure;
- repeated migration;
- evidence modification;
- historical evidence rewrite;
- concurrency;
- runtime pressure.

The public engineering evidence supports the conclusion that these boundaries are actively validated rather than existing only as architectural statements.

The appropriate public position is:

> **VRP provides an implemented, fail-closed continuity security model with validated lifecycle isolation, replay resistance, authority control, canonical-state protection, recovery validation, and independently checkable evidence boundaries under the tested conditions.**

Environment-specific security qualification remains required before production acceptance.

---

## Public Scope

This document describes externally relevant VRP security properties and validation outcomes.

Protected runtime implementation details are intentionally outside its scope.

---

**Veil Routing Protocol (VRP)**  
**Public Security Documentation — October 2026**
# VRP Public Security Assurance

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering security assurance  
**Updated:** October 2026

---

## 1. Purpose

This document describes the public security properties currently supported by VRP engineering validation.

It does not attempt to prove that VRP is immune to every possible attack or implementation defect.

The purpose is narrower:

> **Define the security-relevant invariants exposed by the architecture, identify the engineering evidence supporting them, and state the limits of those claims.**

The public assurance model focuses on externally relevant properties rather than protected implementation mechanisms.

---

## 2. Security Model

VRP security is organized around several independent but related boundaries:

    SESSION IDENTITY

    SESSION LIFETIME

    TRANSPORT

    CANONICAL STATE

    AUTHORITY

    REPLAY / DUPLICATE DELIVERY

    RECOVERY

    EVIDENCE

These boundaries should not be collapsed into a single concept such as "secure connection."

A secure transport does not by itself establish correct lifecycle state.

A reachable endpoint does not automatically possess authority.

A valid historical operation does not automatically remain valid in a newer lifetime.

A successful recovery does not permit replay or stale work.

---

## 3. Core Security Invariant

The architectural invariant:

    SESSION != TRANSPORT

has security consequences.

If logical session identity were defined entirely by current transport state, transport replacement could unnecessarily redefine higher-level identity.

VRP instead treats transport as replaceable runtime state.

Conceptually:

    SESSION S
       |
    TRANSPORT A
       |
       X
    TRANSPORT CHANGE
       |
    TRANSPORT B
       |
    SESSION S

where continuity is valid.

This does not mean every transport change must preserve a session.

Canonical lifecycle rules still determine whether continuity is permitted.

---

## 4. Transport Independence Is Not an Authentication Claim

Transport independence should not be misinterpreted.

The following implication is invalid:

    SAME SESSION ID
         =>
    AUTOMATICALLY TRUSTED

Session identity, lifecycle validity, state validity, and authority remain separately evaluated.

The ability to survive transport replacement must not become a bypass around admission rules.

---

## 5. Session Lifetime Security

A logical session identifier may exist across more than one lifecycle.

Therefore:

    SESSION ID

alone is insufficient to determine whether work is current.

VRP validation distinguishes lifecycle identity using public lifecycle context including:

    LifetimeEpoch

and:

    Incarnation

The important security property is:

> Work belonging to a historical lifecycle must not automatically become valid against the current lifecycle.

---

## 6. Historical-Lifetime Rejection

Dedicated validation created:

    TOTAL_INCARNATIONS=128

with:

    HISTORICAL_OPERATIONS=127

delivered after the final incarnation became current.

Result:

    HISTORICAL_REJECTED=127

    HISTORICAL_ACCEPTED=0

The final state was intentionally chosen so that the operation type could otherwise be valid.

This distinguishes lifecycle rejection from simple state-machine rejection.

The bounded security result is:

> Historical lifecycle context did not become current authority merely because it referenced the same logical session.

---

## 7. Replay Resistance

Previously accepted or observed work must not automatically become valid new work when delivered again.

The required property is:

    PREVIOUSLY OBSERVED
          |
          v
       REPLAY
          |
          X
    NEW CANONICAL ACCEPTANCE

VRP validation has included replay-flood scenarios.

A recorded validation produced:

    REPLAY_ATTEMPTS=9999

    REPLAY_REJECTED=9999

    REPLAY_ACCEPTED=0

The security-relevant result is zero accepted replay attempts in that scenario.

The number 9,999 is not claimed as a security threshold.

---

## 8. Duplicate Delivery

Duplicate delivery and malicious replay are not necessarily identical causes, but they share an important canonical requirement:

> The same logical work must not become multiple canonical executions merely because it is delivered more than once.

Expected behavior:

    ORIGINAL -> ACCEPTED ONCE

    DUPLICATE -> NOT NEW CANONICAL WORK

This property is validated at the canonical admission boundary rather than inferred solely from transport behavior.

---

## 9. Canonical State Security

A security decision is only useful if rejected work remains rejected throughout the canonical record.

The required relationship is:

    REJECTED EVENT

    STATE_DELTA=0

    ACCEPTED_HISTORY_DELTA=0

    CANONICAL_EVENT_DELTA=0

For accepted work:

    ACCEPTED EVENT

    STATE_DELTA=EXPECTED

    ACCEPTED_HISTORY_DELTA=1

    CANONICAL_EVENT_DELTA=1

This prevents an invalid operation from being rejected by one component while still contaminating canonical history elsewhere.

---

## 10. Canonical Admission Defect and Correction

Engineering validation previously identified a real ordering defect at this boundary.

A current-lifetime but state-invalid event could be appended to canonical EventLog history before final state-machine rejection.

This created an inconsistency:

    EVENT REJECTED BY STATE MACHINE

but:

    EVENT PRESENT IN CANONICAL EVENT HISTORY

The admission order was corrected.

Regression testing now verifies atomicity between state acceptance and canonical event recording.

This history is part of the public assurance record because the correction is stronger evidence than pretending the defect was never found.

---

## 11. Invalid Transition Rejection

An operation that is invalid for the current canonical state must remain invalid even when:

- transport changes;
- recovery is occurring;
- delayed work arrives;
- a new path becomes available.

Expected result:

    INVALID STATE TRANSITION
             |
             X
          REJECTED

A recovery path does not suspend canonical state rules.

---

## 12. Terminal-State Protection

Terminal states must resist unsupported resurrection.

Relevant states include:

    SESSION_CLOSED

    SESSION_FAILED

Dedicated regression testing exercises the supported event surface against terminal sessions.

Unsupported resurrection attempts are rejected without changing:

- canonical state;
- accepted transition history;
- canonical EventLog.

Explicit supported recovery remains a separate operation.

---

## 13. Duplicate Session Creation

Repeated or concurrent creation attempts must not silently produce competing canonical session state.

Expected property:

    EXISTING CANONICAL SESSION
              +
    DUPLICATE CREATE
              |
              X
    NO SECOND CANONICAL SESSION

This is relevant to retries, restart races, and recovery.

---

## 14. Fail-Closed Admission

VRP uses fail-closed behavior for canonical admission.

Representative conditions include:

    MISSING LIFETIME CONTEXT -> REJECT

    STALE LIFETIME          -> REJECT

    REPLAY                   -> REJECT

    INVALID STATE           -> REJECT

    UNAUTHORIZED AUTHORITY   -> REJECT

Unknown or invalid context is not automatically promoted to current valid state.

---

## 15. Connectivity Is Not Authority

One of the important security separations is:

    REACHABLE
       !=
    AUTHORIZED

A transport or node may become reachable without receiving canonical authority.

This matters during:

- failover;
- migration;
- recovery;
- relay replacement;
- restart.

Topology change must not silently redefine authority.

---

## 16. Authority Validation

Authority-oriented validation includes both negative and positive cases.

Representative behavior:

    UNAUTHORIZED TRANSITION=REJECTED

followed by:

    AUTHORIZED TRANSITION=ACCEPTED

when valid authority is used.

This demonstrates the tested distinction between connectivity and canonical authority.

---

## 17. Authority Generation

Authority progression is ordered.

A representative transition is:

    GENERATION 1
        |
        v
    VALID AUTHORITY CHANGE
        |
        v
    GENERATION 2

Once generation 2 is current, generation 1 must not silently regain equivalent authority.

---

## 18. Authority Rollback Rejection

Authority recovery validation has exercised stale and rollback conditions.

Expected behavior:

    STALE AUTHORITY
         |
         X
      REJECTED

and:

    AUTHORITY ROLLBACK
         |
         X
      REJECTED

This protects authority lineage during failover and recovery.

---

## 19. Restart-Race Security

Restart can expose a system to conflicting observations:

    OLD AUTHORITY

    NEW AUTHORITY

    DELAYED WORK

    REPLACEMENT TRANSPORT

    RECOVERED RUNTIME

VRP has exercised restart-oriented authority races.

The validated objective is:

> A restart does not automatically make stale authority current.

The public claim is limited to the tested authority and restart scenarios.

---

## 20. Recovery Security

Recovery is security-sensitive because implementations are often tempted to relax checks to restore service quickly.

VRP's intended rule is the opposite:

    RECOVERY
       |
       v
    NORMAL CANONICAL ADMISSION STILL APPLIES

Therefore:

    replay remains replay

    stale work remains stale

    invalid state remains invalid

    unauthorized authority remains unauthorized

Recovery is not a security bypass.

---

## 21. Transport Failure

Transport failure itself is not treated as evidence that the logical session has been compromised or destroyed.

A real validation run observed:

    TRANSPORT_REACHABILITY=LOST

followed by:

    TRANSPORT_REACHABILITY=RECOVERED

with an observed outage of approximately:

    41633 ms

The duration is not a security metric.

The relevant property is that transport failure remained distinguishable from logical session state.

---

## 22. Migration Security

Migration changes the transport/path environment.

It must not silently change higher-level identity or authority.

Project-level validation has exercised:

    1000 migrations

with session identity preserved.

The security interpretation is not that migration itself is novel.

The relevant property is that repeated transport movement did not redefine the tested logical session identity.

---

## 23. NAT and Path Change

Endpoint changes associated with NAT, CGNAT, roaming, or path replacement should not automatically become proof of new session identity or authority.

Transport observations are inputs.

They are not, by themselves, canonical authorization decisions.

This distinction is important in environments where externally visible network identity changes frequently.

---

## 24. Mutable Read-Boundary Security

Read APIs can become security-relevant if they accidentally expose mutable aliases into canonical runtime state.

VRP engineering review has therefore examined exported read surfaces for mutation escape.

This process identified real defects.

---

## 25. Managed Session Isolation

A previous API boundary returned live mutable ManagedSession state.

An external caller could reach the underlying session engine and mutate canonical state/history without passing through normal SessionManager admission.

The defect was reproduced.

The read boundary was changed to return detached snapshots.

Regression coverage verifies that caller-side mutation of the returned object does not mutate canonical session state.

---

## 26. EventLog Isolation

A previous EventLog accessor exposed the live canonical EventLog.

An external caller could mutate or clear canonical event data through what appeared to be a read-oriented API.

The defect was reproduced and corrected.

The accessor now provides detached read state.

Regression testing verifies mutation isolation.

---

## 27. Event Metadata Isolation

Event structures may contain map-backed metadata.

A shallow copy can therefore appear detached while still sharing mutable nested storage.

Engineering validation identified this alias boundary.

Supported EventLog values are copied so caller-side mutation of returned metadata does not silently rewrite canonical stored event metadata.

---

## 28. Deep Arbitrary Object Limitation

The public event-data model should not be interpreted as a general-purpose recursive object graph with guaranteed deep-copy semantics for every possible arbitrary nested value.

Current engineering validation is scoped to the supported production value shapes.

This limitation is explicit.

It avoids claiming a recursive isolation guarantee that has not been established.

---

## 29. Path Health Read Isolation

Path-health read APIs have been tested for caller-side mutation isolation.

The evaluated read semantics use detached value data.

Mutation of returned read data does not rewrite the monitor's canonical stored path-health state under the tested surface.

---

## 30. Relay List Isolation

A previous relay-list API exposed mutable slice storage.

Caller-side mutation could affect internal list representation.

The defect was reproduced and corrected.

The returned list container is now isolated from the canonical relay-selector container.

---

## 31. Transport Registry Semantics

TransportRegistry read semantics are intentionally different.

The returned list container is detached.

However, the Transport objects themselves preserve their runtime object identity.

This is intentional because transports are active runtime objects rather than immutable historical records.

Therefore:

    LIST CONTAINER = DETACHED

while:

    TRANSPORT OBJECT IDENTITY = PRESERVED

This is a documented ownership boundary rather than an accidental alias.

---

## 32. Runtime Composition Boundary

SessionRuntime exposes access to composed runtime components.

The Manager and Authority accessors intentionally preserve stable shared component identity.

This is not treated as a snapshot API.

Security is instead enforced by canonical admission rules.

The relevant property is:

    SHARED COMPONENT IDENTITY
            !=
    ADMISSION BYPASS

Regression coverage verifies that an unbound event cannot bypass required lifetime admission merely because the caller has access to the composed manager.

---

## 33. Trace Identity

Historical traces may contain events from multiple lifetimes of the same logical SessionID.

A trace that prints only SessionID can therefore become ambiguous.

VRP trace output was hardened to include lifecycle identity information such as:

    lifetime_epoch

    incarnation

This improves the ability to distinguish current and historical events during review.

---

## 34. Concurrency Security

Many state-integrity failures appear only under concurrency.

VRP engineering validation therefore includes concurrent scenarios involving:

- multiple sessions;
- repeated reincarnation;
- delayed historical work;
- transport migration;
- packet identifiers;
- recovery.

Selected test families have also been executed under Go race detection.

---

## 35. Deterministic Concurrency

A deterministic concurrency scenario has completed:

    1000 repeated executions

with consistent successful results.

This provides evidence for the exercised concurrency path.

It does not establish exhaustive correctness across every possible scheduler interleaving.

---

## 36. Packet Identifier Concurrency

Packet identifier generation has been exercised under concurrent conditions.

A recorded validation generated:

    4096 packet identifiers

with:

    DUPLICATES=0

    GAPS=0

for the tested scenario.

This is a regression result, not a claim of universal identifier-space behavior.

---

## 37. Runtime Pressure

Security-relevant state should remain coherent under degraded network conditions.

VRP has been exercised under:

- jitter;
- latency variation;
- packet reordering;
- congestion;
- repeated migration;
- temporary transport loss.

The objective is to detect whether pressure causes canonical admission or lifecycle rules to fail.

---

## 38. Long-Duration Validation

Some defects require accumulated runtime state before they appear.

VRP engineering validation therefore includes:

- prolonged network turbulence;
- repeated runtime cycles;
- week-scale harness execution.

These runs improve confidence in the tested invariants.

They are not proof of unlimited-duration correctness.

---

## 39. Cryptographic Security Objectives

The public VRP architecture includes cryptographic objectives such as:

- message integrity;
- authenticated protected communication;
- replay resistance;
- session/context binding;
- authority validation;
- evidence integrity.

Public assurance describes the required properties.

It does not require publication of protected runtime implementation details.

---

## 40. Cryptography Is Not the Entire Security Model

Even correct cryptography would not automatically prevent:

    stale lifecycle acceptance

    invalid state transitions

    authority rollback

    canonical history contamination

    mutable API aliasing

    incorrect recovery semantics

For this reason, VRP security validation extends beyond cryptographic primitive selection.

The runtime state model itself is part of the security boundary.

---

## 41. Evidence Integrity

Security claims should not depend exclusively on the runtime printing its own PASS verdict.

VRP evidence validation includes integrity checking over structured validation artifacts.

Engineering testing has deliberately modified evidence and verified that inconsistencies are detected.

---

## 42. Evidence Modification

Adversarial evidence validation has included modification of:

- event evidence;
- summary information;
- manifest-bound values;
- historical evidence.

The tested verifier rejected inconsistent evidence rather than continuing to trust the original successful verdict.

---

## 43. Coordinated Evidence Modification

A stronger test modifies multiple related artifacts rather than one isolated file.

VRP validation has exercised coordinated modification scenarios involving evidence and associated summary/manifest relationships.

The purpose is to test whether verification relies only on one superficial consistency check.

The result supports the tested evidence-integrity boundary.

---

## 44. Historical Evidence Integrity

Previously established validation history should not be silently replaceable by contradictory history.

Historical rewrite scenarios have been exercised.

The independent historical verification path detected inconsistent reconstruction under the tested evidence model.

---

## 45. Independent Evidence Verification

Selected security-relevant evidence can be evaluated separately from the runtime process that originally generated it.

Conceptually:

    RUNTIME
       |
       v
    EVIDENCE
       |
       v
    SEPARATE VERIFIER
       |
       v
    VERDICT

This reduces dependence on self-reported runtime success.

---

## 46. Verification Boundary

Independent evidence verification means:

> The exposed evidence contract can be evaluated separately.

It does not mean:

    COMPLETE PRIVATE SOURCE AUDIT

    COMPLETE HOST ATTESTATION

    FORMAL PROOF OF ALL RUNTIME STATES

    EXTERNAL SECURITY CERTIFICATION

Those are different assurance activities.

---

## 47. Evidence Integrity vs Authenticity

Integrity and authenticity must remain distinct.

Integrity asks:

> Has the evidence changed relative to the values or history against which it is being checked?

Authenticity asks:

> Who produced the evidence, and why should that producer identity be trusted?

Hash consistency primarily contributes to integrity.

It does not automatically establish organizational, operator, or host authenticity.

---

## 48. Evidence Is Not a Trusted Host

A valid evidence bundle does not prove that:

- the host operating system was uncompromised;
- every observation source was trustworthy;
- the operator was honest;
- every relevant event was captured;
- the runtime contained no undiscovered defect.

Those concerns require additional trust mechanisms or review.

The public evidence claim remains bounded.

---

## 49. Defect Discovery Is Part of Assurance

Security assurance should not be evaluated by whether a project can claim that no defect has ever existed.

A stronger engineering process is:

    IDENTIFY
       |
       v
    REPRODUCE
       |
       v
    CORRECT
       |
       v
    REGRESSION TEST
       |
       v
    REQUALIFY

VRP validation has used this process for real state-integrity and API-boundary defects.

Retained regression coverage makes those corrected boundaries continuously testable.

---

## 50. Failure Must Be Reportable

A security validation system must be capable of producing failure.

Relevant failure outcomes include:

    REPLAY_ACCEPTED

    STALE_WORK_ACCEPTED

    INVALID_TRANSITION_ACCEPTED

    CANONICAL_HISTORY_CONTAMINATED

    UNAUTHORIZED_AUTHORITY_ACCEPTED

    AUTHORITY_ROLLBACK_ACCEPTED

    MUTABLE_ALIAS_ESCAPE

    EVIDENCE_VERIFICATION_FAILED

Such outcomes should block the corresponding assurance claim until investigated.

---

## 51. Security Assurance Matrix

| Security Property | Current Engineering Status |
|---|---|
| Session / transport separation | Validated |
| Historical-lifetime rejection | Validated |
| Replay rejection | Validated |
| Duplicate canonical execution containment | Validated |
| Invalid-transition rejection | Validated |
| Canonical admission atomicity | Validated |
| Terminal-state protection | Validated |
| Duplicate-create isolation | Validated |
| Fail-closed admission | Validated |
| Connectivity / authority separation | Validated |
| Unauthorized authority rejection | Validated |
| Stale authority rejection | Validated |
| Authority rollback rejection | Validated |
| Restart-oriented authority recovery | Validated under tested scenarios |
| Managed-session read isolation | Validated |
| EventLog read isolation | Validated |
| Supported event metadata isolation | Validated |
| Path-health read isolation | Validated |
| Relay-list container isolation | Validated |
| Transport-list container isolation | Validated |
| Concurrency regression | Validated under tested scenarios |
| Replay-flood containment | Validated |
| Evidence modification detection | Validated |
| Historical rewrite detection | Validated |
| Independent evidence verification | Validated |

---

## 52. Threat Classes Exercised

The engineering record includes scenarios intended to challenge properties through conditions such as:

    REPLAY

    DUPLICATE DELIVERY

    STALE LIFETIME

    INVALID STATE

    TERMINAL RESURRECTION

    DUPLICATE CREATION

    AUTHORITY ROLLBACK

    STALE AUTHORITY

    RESTART RACE

    TRANSPORT FAILURE

    TRANSPORT MIGRATION

    MUTABLE API ALIASING

    EVIDENCE MODIFICATION

    HISTORICAL REWRITE

These are engineering test classes.

They should not be interpreted as an exhaustive threat taxonomy.

---

## 53. Threats Requiring Deployment-Specific Review

A target deployment should separately evaluate threats associated with its actual environment.

Examples may include:

- host compromise;
- credential theft;
- key management;
- endpoint compromise;
- denial of service;
- traffic analysis;
- infrastructure compromise;
- dependency compromise;
- application-layer vulnerabilities;
- operational misconfiguration.

Project-level invariant testing cannot automatically resolve these deployment-specific risks.

---

## 54. Denial of Service

Fail-closed admission can prevent invalid work from becoming canonical state.

It does not imply immunity to denial of service.

An attacker may still attempt to consume:

- CPU;
- memory;
- bandwidth;
- connection capacity;
- verifier resources.

Resource controls and deployment architecture require separate qualification.

---

## 55. Traffic Analysis

Transport continuity does not automatically provide traffic-analysis resistance.

An external observer may potentially learn information from:

- timing;
- traffic volume;
- response size;
- connection patterns;
- error behavior.

These concerns belong to a separate observability/leakage analysis.

They should not be inferred as solved merely because session continuity is preserved.

---

## 56. Host Compromise

VRP runtime invariants do not make a fully compromised host trustworthy.

If an attacker controls the execution environment, assumptions about:

- keys;
- runtime memory;
- evidence generation;
- local observations

may no longer hold.

Host security therefore remains part of deployment security.

---

## 57. Application Security

VRP does not replace application security.

Applications remain responsible for their own requirements such as:

- authorization;
- input validation;
- business logic;
- application secrets;
- user identity;
- data handling.

VRP continuity properties operate at a different architectural boundary.

---

## 58. Relationship to QUIC

QUIC includes mature mechanisms for connection identity, encrypted transport, replay-related protections in appropriate contexts, and connection migration.

VRP does not claim that transport-independent connection/session concepts were invented by this project.

The useful comparison is architectural:

    What identity survives?

    Which layer owns lifecycle?

    Which layer owns canonical state?

    How is stale work rejected?

    How is authority changed?

    What recovery evidence is available?

VRP should be evaluated on those properties rather than on a claim that migration itself is novel.

---

## 59. Relationship to Multipath TCP

Multipath TCP allows a connection to use multiple network paths.

That solves important transport-level continuity problems.

VRP's validation boundary additionally evaluates application-independent runtime concepts such as:

- lifecycle identity;
- canonical admission;
- authority lineage;
- stale-work rejection;
- evidence verification.

This does not make the technologies mutually exclusive.

They operate at different or potentially overlapping layers.

---

## 60. Relationship to VPN Roaming

VPN and tunnel implementations may support endpoint roaming, reconnection, or path replacement.

VRP does not claim that roaming itself is unique.

The relevant question is whether the system also defines and validates:

    logical session lifecycle

    canonical state

    authority lineage

    stale historical work

    recovery admission

at the required architectural layer.

---

## 61. Relationship to Zero Trust

VRP uses principles compatible with Zero Trust thinking, particularly:

    connectivity != authority

and:

    current validity must be established rather than assumed from location or reachability

However, VRP itself should not be represented as a complete Zero Trust architecture for an organization.

Identity systems, policy engines, endpoint posture, organizational controls, and application authorization remain separate concerns.

---

## 62. Formal Verification

Current VRP assurance is based primarily on engineering validation:

- unit and regression testing;
- adversarial scenarios;
- concurrency testing;
- race detection;
- failure injection;
- long-duration execution;
- real network interruption;
- evidence integrity testing;
- independent evidence verification.

Unless a specific artifact states otherwise, the project does not claim complete formal verification through systems such as:

    TLA+

    Coq

    Lean

    Isabelle

Engineering validation and mathematical proof are different assurance levels.

---

## 63. Security Certification

The public engineering record should not be interpreted as external certification unless an actual independent certification process has occurred.

Terms such as:

    validated

    independently verified evidence

refer to the defined engineering validation scope.

They do not automatically mean:

    certified by a standards body

    audited by an independent security laboratory

    formally verified

Those claims require their own evidence.

---

## 64. Review Questions

A technical security reviewer should ask:

    What is the protected invariant?

    What input attempts to violate it?

    Where is admission decided?

    What happens to rejected work?

    Can historical work become current?

    Can transport change authority?

    Can recovery weaken validation?

    Can read APIs mutate canonical state?

    Can evidence be modified without detection?

    Can the verifier independently fail?

    What remains outside the public verification boundary?

These questions directly test the assurance model.

---

## 65. Target Deployment Security Qualification

Before production use, the target environment should identify which VRP properties are security-critical for that deployment.

Qualification should then reproduce relevant scenarios using the actual:

    build

    operating environment

    network topology

    transports

    applications

    scale

    operational controls

Project-level evidence provides a starting point.

Deployment-specific evidence is still required.

---

## 66. Current Security Position

The current public engineering position is:

    SESSION / TRANSPORT SEPARATION      VALIDATED

    LIFETIME ISOLATION                  VALIDATED

    REPLAY REJECTION                    VALIDATED

    CANONICAL ADMISSION                 VALIDATED

    FAIL-CLOSED BEHAVIOR                VALIDATED

    AUTHORITY LINEAGE                   VALIDATED

    AUTHORITY ROLLBACK REJECTION        VALIDATED

    RECOVERY ADMISSION                  VALIDATED

    READ-BOUNDARY ISOLATION             VALIDATED ON REVIEWED SURFACES

    CONCURRENCY                         VALIDATED UNDER TESTED SCENARIOS

    EVIDENCE INTEGRITY                  VALIDATED

    HISTORICAL INTEGRITY                VALIDATED

    INDEPENDENT EVIDENCE VERIFICATION   VALIDATED

    UNIVERSAL SECURITY CERTIFICATION    NOT CLAIMED

    COMPLETE FORMAL VERIFICATION        NOT CLAIMED

    TARGET DEPLOYMENT QUALIFICATION     REQUIRED

---

## 67. What the Current Evidence Supports

The current engineering record supports bounded statements that:

- logical session identity can remain distinct from replaceable transport;
- historical session lifetimes can be rejected after reincarnation;
- replay can be rejected as new canonical work;
- invalid transitions can remain outside canonical history;
- terminal sessions can resist unsupported resurrection;
- connectivity can remain distinct from authority;
- stale and rollback authority can be rejected;
- recovery can remain subject to canonical admission;
- reviewed read APIs can preserve intended mutation boundaries;
- evidence inconsistency can be detected by the tested verification model;
- selected evidence can be independently evaluated.

These statements are limited to the implemented and tested boundaries.

---

## 68. What the Current Evidence Does Not Support

The current engineering record does not establish:

    PERFECT SECURITY

    ZERO VULNERABILITIES

    IMMUNITY TO HOST COMPROMISE

    IMMUNITY TO DENIAL OF SERVICE

    COMPLETE TRAFFIC-ANALYSIS RESISTANCE

    CORRECTNESS ON EVERY PLATFORM

    CORRECTNESS UNDER EVERY FAILURE

    EXHAUSTIVE CONCURRENCY CORRECTNESS

    UNIVERSAL PRODUCTION CERTIFICATION

    COMPLETE FORMAL PROOF

Those require separate evidence.

---

## Conclusion

VRP public security assurance is based on explicit state and authority invariants rather than on the claim that a secure transport alone solves continuity security.

The current engineering record has deliberately exercised:

    replay

    duplicate delivery

    stale lifetimes

    invalid transitions

    terminal resurrection

    duplicate creation

    transport failure

    migration

    authority rollback

    restart races

    mutable API boundaries

    evidence modification

    historical rewrite

and retained regression coverage for corrected defects discovered during that process.

The strongest appropriate public statement is:

> **VRP has an implemented security-relevant continuity model whose session lifecycle, canonical admission, authority, replay, recovery, and evidence boundaries have been deliberately challenged and validated under documented engineering scenarios.**

This is not a claim of perfect security.

It is a claim that specific security properties are defined precisely enough to be attacked in tests, fail when violated, and produce evidence that can be reviewed.
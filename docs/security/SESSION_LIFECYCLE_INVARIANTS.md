# VRP Session Lifecycle Invariants

**Document Type:** Architecture / Security Specification  
**Status:** Public Specification  
**Protocol:** Veil Routing Protocol (VRP)  
**Scope:** Session lifecycle, event ordering, concurrency, removal, recreation, and authority boundaries

---

## 1. Purpose

This document defines the security invariants governing the lifecycle of a VRP session.

VRP treats a session as a logical protocol identity whose lifetime is independent from any individual transport path.

The foundational rule remains:

> **Session ≠ Transport**

Transport replacement, path migration, relay changes, temporary network loss, and recovery must not implicitly create a new logical session.

At the same time, explicit destruction and later recreation of a logical session introduces a separate security boundary.

A newly created session must never inherit authority, state, or delayed actions from a previously destroyed session merely because the same external Session ID is reused.

This document defines the required behavior at that boundary.

---

## 2. Core Lifecycle Principle

A Session ID identifies a logical session namespace.

It must not, by itself, be treated as sufficient proof that an event belongs to the currently authoritative lifetime of that session.

Conceptually:

    Session Identity
          |
          +---- Session Lifetime / Incarnation
          |
          +---- Authority Generation
          |
          +---- State Machine
          |
          +---- Replaceable Transport(s)

These dimensions must not be accidentally collapsed into one another.

---

## 3. Session Creation Invariant

For a currently active Session ID:

    Create(S)
    Create(S)
    Create(S)
    ...

must result in exactly one successful creation.

Concurrent attempts to create the same active Session ID must not produce multiple canonical session objects.

The session registry must preserve:

    ACTIVE_CANONICAL_SESSION_COUNT(SessionID) <= 1

at all times.

---

## 4. Session Removal Invariant

Removing one session must not mutate the state of another session.

For distinct sessions:

    Remove(S1)

must not modify:

    State(S2)
    History(S2)
    Authority(S2)
    TransportBinding(S2)

where:

    S1 != S2

Session removal must therefore be isolated at the registry and state-machine boundaries.

---

## 5. Concurrent Removal Invariant

When multiple workers concurrently attempt to remove the same session, only one operation may perform the canonical removal.

Conceptually:

    N concurrent Remove(S)
            |
            v
    exactly one canonical success
            |
            +---- all others reject / observe absence

The operation must not cause:

- duplicate destruction,
- map corruption,
- inconsistent registry state,
- panic,
- cross-session mutation,
- resurrection of the removed session.

---

## 6. Event Ordering Invariant

VRP session state transitions are ordered.

An event that is not valid for the current session state must not advance canonical state.

Examples include:

    MigrationCompleted

without an accepted migration request, or:

    RecoveryConfirmed

before migration completion.

Canonical state transitions must therefore satisfy the session state machine rather than accepting events solely because they reference an existing Session ID.

---

## 7. Duplicate Transition Invariant

A transition that has already been consumed must not be capable of advancing the state machine a second time.

Examples include duplicate:

    MigrationCompleted
    RecoveryConfirmed

events.

For a canonical transition:

    State A
       |
       | Event X
       v
    State B

a later replay of `Event X` must not create another canonical transition merely because the Session ID remains valid.

Duplicate delivery must therefore be rejected or otherwise contained without advancing canonical session state.

---

## 8. Authority Generation

VRP runtime events may carry authority-generation information.

Conceptually:

    Session
       |
       +---- Owner
       |
       +---- Authority Generation

Authority generations provide a mechanism for distinguishing current authority from stale authority.

A lower or obsolete authority generation must never regain control over a newer canonical authority lineage.

This is distinct from session-lifetime fencing.

Authority generation protects authority lineage.

Session-lifetime fencing protects the boundary between destroyed and recreated session instances.

The two mechanisms must not be assumed to be interchangeable without explicit validation.

---

## 9. Session Reincarnation / ABA Boundary

Consider the following lifecycle:

    Create S1
       |
       v
    Operate S1
       |
       +---- delayed event E1 remains in flight
       |
    Remove S1
       |
       v
    Create S2

where:

    ExternalSessionID(S1) == ExternalSessionID(S2)

The delayed event then arrives:

    E1 -> S2 ?

This is an ABA-style lifecycle boundary.

The required invariant is:

> An event originating from a destroyed session lifetime must never mutate a later session lifetime solely because both use the same external Session ID.

Conceptually:

    OLD SESSION
    S1 / generation or incarnation A
            |
            | delayed event
            v
            X
            |
            | MUST NOT CROSS
            v
    NEW SESSION
    S2 / generation or incarnation B

---

## 10. Reincarnation Fencing Requirement

If Session ID reuse is permitted, an implementation must provide sufficient fencing to distinguish different session lifetimes.

The exact protected implementation mechanism is not specified by this public document.

Possible architectural mechanisms include:

- session incarnation identifiers,
- lifecycle generations,
- cryptographically bound session epochs,
- authority/lifecycle composite identity,
- equivalent monotonic fencing mechanisms.

The implementation mechanism remains an implementation concern.

The required externally observable property is:

    OLD_LIFETIME_EVENT
            +
    NEW_SESSION_WITH_SAME_EXTERNAL_ID
            =
    REJECT / CONTAIN

and never:

    OLD_LIFETIME_EVENT
            +
    NEW_SESSION_WITH_SAME_EXTERNAL_ID
            =
    CANONICAL_STATE_MUTATION

---

## 11. Stale Authority Invariant

After authority moves from an old owner or generation to a newer canonical authority:

    Authority(epoch=N)
            |
            v
    Takeover
            |
            v
    Authority(epoch=N+1)

the previous authority must not:

- renew canonical ownership,
- perform canonical migration,
- regain primary status,
- overwrite the newer authority,
- resurrect through concurrent stale operations.

This invariant applies even when stale operations arrive concurrently.

---

## 12. Session and Authority Separation

Session lifecycle and authority lifecycle are related but distinct.

The following must not be assumed:

    same SessionID
    =
    same session lifetime

and:

    same SessionID
    =
    current authority

and:

    valid state-machine event
    =
    authoritative event

An event may be syntactically valid for a state-machine transition while still being stale from the perspective of authority or session lifetime.

VRP implementations must preserve these distinctions.

---

## 13. Concurrency Requirement

All lifecycle invariants must continue to hold under concurrent execution.

Validation should include pressure from:

    duplicate creation
    duplicate removal
    stale authority operations
    parallel migration
    concurrent failover
    delayed events
    session destruction
    session recreation
    transport replacement

A successful sequential execution is not sufficient evidence of concurrency safety.

Race-detection and repeated adversarial execution should be used where implementation environments support them.

---

## 14. Event History Semantics

Runtime observation and canonical acceptance are separate concepts.

A system may retain rejected or attempted events for audit purposes.

Therefore:

    EVENT OBSERVED

does not necessarily mean:

    EVENT ACCEPTED AS CANONICAL

Implementations and evidence systems should preserve this distinction.

Where rejected events are retained, evidence should make their disposition unambiguous.

---

## 15. Fail-Closed Requirement

Ambiguity at a session-lifecycle or authority boundary must not silently grant canonical authority.

When the implementation cannot establish that an event belongs to the current valid session lifetime and authority lineage, the secure behavior is:

    REJECT

or:

    CONTAIN

rather than canonical mutation.

This follows the broader VRP principle:

> **Fail closed when authority cannot be established.**

---

## 16. Required Security Properties

A conforming VRP runtime should preserve the following properties:

    SESSION_IDENTITY_CONTINUITY
    TRANSPORT_INDEPENDENCE
    SINGLE_CANONICAL_SESSION
    REMOVAL_ISOLATION
    DUPLICATE_CREATION_CONTAINMENT
    DUPLICATE_REMOVAL_CONTAINMENT
    TRANSITION_ORDER_ENFORCEMENT
    DUPLICATE_TRANSITION_REJECTION
    STALE_AUTHORITY_REJECTION
    MONOTONIC_AUTHORITY_LINEAGE
    SESSION_REINCARNATION_FENCING
    CROSS_SESSION_ISOLATION
    FAIL_CLOSED_LIFECYCLE_BOUNDARIES

---

## 17. Validation Principle

VRP does not treat an architectural intention as a verified property.

A property becomes supported by engineering evidence only when an adversarial test exercises the relevant boundary and produces a reproducible result.

Therefore:

    ARCHITECTURAL REQUIREMENT
            !=
    VALIDATED IMPLEMENTATION PROPERTY

until evidence exists.

This distinction is intentional.

---

## 18. Public / Protected Boundary

This specification describes required security properties.

It does not disclose:

- protected runtime algorithms,
- internal fencing implementation,
- private authority mechanisms,
- cryptographic material,
- anti-clone mechanisms,
- proprietary recovery logic.

The public specification defines:

    WHAT MUST HOLD

while the protected VRP Core determines:

    HOW IT IS ENFORCED

---

## 19. Summary

VRP session lifecycle security is based on more than thread safety.

The runtime must preserve identity, authority, ordering, isolation, and lifecycle boundaries even when operations are duplicated, delayed, reordered, concurrent, stale, or adversarial.

The critical distinction is:

    SESSION != TRANSPORT

    SESSION ID != SESSION LIFETIME

    EVENT VALIDITY != EVENT AUTHORITY

    OLD AUTHORITY != CURRENT AUTHORITY

A transport may change while the session survives.

An authority may change while the session survives.

But once a session lifetime is destroyed, stale actions from that lifetime must never silently become authoritative over a later incarnation.

That boundary is a first-class VRP security invariant.
EOF

git status --short
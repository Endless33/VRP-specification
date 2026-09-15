# VRP Session Security Validation Index

**Document Type:** Security / Validation Index  
**Status:** Public  
**Protocol:** Veil Routing Protocol (VRP)  
**Scope:** Session Lifecycle, Authority, Concurrency, Ordering, Isolation, and Reincarnation

---

## 1. Purpose

This document provides a single navigation and interpretation point for the public VRP session-security specification and validation records.

The session-security model is intentionally separated into:

    REQUIREMENTS
        |
        v
    ADVERSARIAL VALIDATION
        |
        v
    EVIDENCE
        |
        v
    VERDICT

This separation prevents architectural requirements from being presented as validated implementation properties before corresponding evidence exists.

---

## 2. Core Principle

The foundational VRP principle is:

    SESSION != TRANSPORT

A logical session is not equivalent to the transport currently carrying its traffic.

Transport paths may:

    appear
    disappear
    fail
    recover
    migrate
    rebind
    be replaced

without automatically destroying the logical session.

Session continuity therefore belongs to the session layer rather than to one transport instance.

---

## 3. Extended Identity Model

Session lifecycle validation introduces additional distinctions:

    SESSION != TRANSPORT

    SESSION ID != SESSION LIFETIME

    EVENT VALIDITY != EVENT AUTHORITY

    OLD AUTHORITY != CURRENT AUTHORITY

These distinctions define separate security boundaries.

A matching Session ID alone must not automatically prove that an event belongs to the currently authoritative session lifetime.

---

## 4. Public Documents

### 4.1 Session Lifecycle Invariants

Document:

    docs/security/SESSION_LIFECYCLE_INVARIANTS.md

Purpose:

Defines the normative security requirements for:

- session creation,
- session removal,
- concurrent lifecycle operations,
- transition ordering,
- duplicate transition rejection,
- stale authority rejection,
- session reincarnation,
- fail-closed lifecycle behavior.

This document describes:

    WHAT MUST HOLD

It does not claim that every listed invariant has already been validated.

---

### 4.2 Session Manager Adversarial Validation

Document:

    docs/validation/SESSION_MANAGER_ADVERSARIAL_VALIDATION.md

Purpose:

Records adversarial validation already performed against the Session Manager and related state-machine boundaries.

The recorded surfaces include:

    duplicate creation
    duplicate removal
    removal isolation
    event ordering
    duplicate transition rejection
    invalid transition ordering
    repeated shuffled execution
    race-detector execution
    whole-Core regression

This document describes:

    WHAT WAS TESTED

and:

    WHAT RESULT WAS OBSERVED

---

### 4.3 Session Reincarnation / ABA Test Model

Document:

    docs/validation/SESSION_REINCARNATION_ABA_TEST_MODEL.md

Purpose:

Defines the dedicated adversarial model for:

    REMOVE OLD SESSION
            |
            v
    RECREATE SAME SESSION ID
            |
            v
    DELIVER OLD LIFETIME EVENT
            |
            v
    VERIFY NEW SESSION IS NOT MUTATED

This document describes an explicit lifecycle boundary that must be independently validated.

---

## 5. Validation Identity

The session-manager lockdown recorded on 2026-09-15 was associated with:

    HEAD=
    c39d3dc06e5f3a561a0fb3fd1551cba112a2e383

    TREE=
    04bdefd9af43a77bc0ed892fc87f7ab5a0510f6c

A dedicated session duplicate/order validation produced:

    FINAL_VERDICT=
    VRP_SESSION_ORDER_DUPLICATE_SURFACE_PRESERVED

with:

    PASS_COUNT=15
    FAIL_COUNT=0

The validation also preserved an unchanged source working tree during the evidence run.

---

## 6. Validated Session-Manager Surfaces

The currently recorded evidence supports the following tested properties:

    DUPLICATE_CREATE_CONTAINED

    DUPLICATE_REMOVE_CONTAINED

    CROSS_SESSION_REMOVAL_ISOLATED

    EVENT_ORDER_PRESERVED

    DUPLICATE_MIGRATION_COMPLETION_REJECTED

    DUPLICATE_RECOVERY_CONFIRMATION_REJECTED

    INVALID_RECOVERY_ORDER_REJECTED

    INVALID_MIGRATION_ORDER_REJECTED

    SESSION_ORDER_DUPLICATE_SURFACE_PRESERVED

    WHOLE_CORE_RACE_REGRESSION_PASSED

These verdicts apply to the recorded validation conditions.

They are not unrestricted claims about every possible execution schedule or future implementation revision.

---

## 7. Concurrent Creation Boundary

The Session Manager was exercised with:

    512 concurrent workers

attempting to create the same Session ID.

Required result:

    exactly 1 successful creation

    exactly 511 rejected duplicate creations

Observed result:

    PRESERVED

Security property:

    SINGLE_CANONICAL_SESSION

---

## 8. Concurrent Removal Boundary

The Session Manager was exercised with:

    512 concurrent workers

attempting to remove the same Session ID.

Required result:

    exactly 1 successful removal

    exactly 511 rejected duplicate removals

Observed result:

    PRESERVED

Security property:

    SINGLE_CANONICAL_REMOVAL

---

## 9. Cross-Session Removal Isolation

The removal-isolation validation exercised:

    1000 sessions

with:

    500 sessions removed

and required the remaining:

    500 sessions

to remain active.

Observed result:

    PRESERVED

Security properties:

    REMOVAL_ISOLATION

    CROSS_SESSION_STATE_ISOLATION

    SESSION_REGISTRY_INTEGRITY

---

## 10. Transition Ordering Boundary

The tested lifecycle sequence included:

    CreateSession
          |
          v
    InitComplete
          |
          v
    TransportAttached
          |
          v
    PathLost
          |
          v
    MigrationRequested
          |
          v
    MigrationCompleted
          |
          v
    RecoveryConfirmed

The tested event history preserved the submitted lifecycle ordering.

Additional adversarial tests rejected invalid ordering such as:

    RecoveryConfirmed
    before
    MigrationCompleted

and:

    MigrationCompleted
    without
    MigrationRequested

Security property:

    TRANSITION_ORDER_ENFORCEMENT

---

## 11. Duplicate Transition Boundary

Adversarial validation included duplicate:

    MigrationCompleted

and:

    RecoveryConfirmed

events.

The state machine rejected the tested duplicate transitions.

Security properties:

    DUPLICATE_TRANSITION_REJECTION

    STATE_MACHINE_REPLAY_CONTAINMENT

A duplicate transition rejection at this layer should not be interpreted as a complete protocol-wide replay-security proof.

It demonstrates the tested state-machine property.

---

## 12. Authority Boundary

VRP separately validates authority lineage.

Relevant adversarial surfaces include:

    stale owner

    expired authority

    concurrent stale-owner flood

    ownership transfer

    migration after ownership transfer

    lease epoch takeover

    stale epoch resurrection

The required authority invariant is:

    OLD AUTHORITY
          |
          X
          |
    NEW CANONICAL AUTHORITY

Once authority advances, obsolete authority must not regain canonical control.

---

## 13. Authority Generation and Session Lifetime

Authority generation and session lifetime are related but distinct concepts.

Authority generation answers:

    WHICH AUTHORITY IS CURRENT?

Session lifetime answers:

    WHICH INCARNATION OF THIS SESSION ID IS CURRENT?

Therefore:

    AUTHORITY_GENERATION
            !=
    SESSION_LIFETIME

unless the protected implementation explicitly binds them with equivalent security semantics.

Public validation must not infer one property solely from the existence of the other.

---

## 14. Session Reincarnation Boundary

The remaining explicitly tracked lifecycle boundary is:

    SESSION REINCARNATION / ABA

Attack model:

    Create S1
       |
       v
    Operate S1
       |
       +---- capture delayed event E1
       |
       v
    Remove S1
       |
       v
    Create S2
    using same Session ID
       |
       v
    Advance S2
       |
       v
    Inject E1
       |
       v
       ?

Required result:

    E1 MUST NOT MUTATE S2

This property is intentionally tracked separately from ordinary cross-session isolation.

---

## 15. Why Reincarnation Is Different

Traditional cross-session isolation considers:

    Session A
        !=
    Session B

with different identities.

ABA/reincarnation considers:

    OLD Session A
        |
        | destroyed
        v
    NEW Session A

where the external identifier may be identical.

Therefore:

    SAME SESSION ID

does not prove:

    SAME SESSION LIFETIME

This is the security distinction the dedicated ABA validation is designed to exercise.

---

## 16. Strong ABA Validation Requirement

The recreated session must be advanced into a state where the delayed old event would otherwise represent a valid transition.

Otherwise the state machine could reject the event simply because it arrived in the wrong state.

That would validate:

    EVENT ORDERING

but not necessarily:

    SESSION REINCARNATION FENCING

The stronger test requires:

    OLD EVENT
        +
    NEW COMPATIBLE STATE
        |
        v
        X
        |
    MUST STILL REJECT

---

## 17. Canonical Mutation Rule

For any stale-lifetime injection test:

    stateBefore = State(CurrentSession)

The stale event is then delivered.

Required result:

    stateAfter == stateBefore

The important security property is not merely that an error was returned.

The property is:

    NO UNAUTHORIZED CANONICAL MUTATION

A system that returns an error after already mutating canonical state has not preserved this invariant.

---

## 18. Evidence Semantics

VRP distinguishes:

    EVENT OBSERVED

from:

    EVENT ACCEPTED

and:

    EVENT RECORDED

from:

    CANONICAL TRANSITION

A rejected adversarial event may still be retained as evidence.

Its presence in an audit log does not automatically indicate that it modified canonical state.

Evidence consumers should be able to distinguish event observation from canonical acceptance.

---

## 19. Validation Escalation Model

A security boundary should progress through increasingly hostile validation stages:

    DETERMINISTIC SINGLE TEST
            |
            v
    REPEATED EXECUTION
            |
            v
    SHUFFLED EXECUTION
            |
            v
    RACE DETECTOR
            |
            v
    CONCURRENT PRESSURE
            |
            v
    WHOLE-CORE REGRESSION
            |
            v
    EVIDENCE CAPTURE

For session reincarnation, later stages may include:

    256 stale workers

or:

    512 stale workers

attempting to cross the old/new lifetime boundary concurrently.

---

## 20. Failure Policy

An adversarial failure must not be hidden by weakening the test.

Required process:

    FAILURE
       |
       v
    PRESERVE EVIDENCE
       |
       v
    REPRODUCE
       |
       v
    IDENTIFY INVARIANT VIOLATION
       |
       v
    MINIMAL PRODUCTION CORRECTION
       |
       v
    ORIGINAL TEST
       |
       v
    STRESS
       |
       v
    RACE
       |
       v
    FULL REGRESSION

A red adversarial test is useful evidence.

It identifies an assumption that did not survive hostile validation.

---

## 21. Evidence Preservation

Strong validation evidence should preserve:

    UTC timestamp

    Git HEAD

    Git Tree

    Go version

    exact test names

    test parameters

    PASS / FAIL results

    race-detector result

    source working-tree state

    final verdict

    evidence manifest hash

This allows a validation result to be associated with a specific implementation state rather than with a general project claim.

---

## 22. Claim Discipline

VRP uses four distinct validation states:

    DESIGNED

    TESTED

    PRESERVED

    INCOMPLETE / PENDING

These terms must not be collapsed.

A property may be architecturally required without yet having a completed adversarial validation.

Likewise, one passing test does not prove every related security property.

The intended rule is:

    CLAIM STRENGTH
        <=
    EVIDENCE STRENGTH

---

## 23. Current Session Security Position

Current evidence supports multiple adversarial session-management properties, including:

    concurrent creation containment

    concurrent removal containment

    cross-session removal isolation

    transition-order enforcement

    duplicate-transition rejection

    repeated shuffled execution

    race-detector regression

The dedicated session-reincarnation boundary remains separately tracked until its adversarial test is completed.

Current status:

    SESSION_MANAGER_DUPLICATE_ORDER_SURFACE =
    PRESERVED

    SESSION_REINCARNATION_FENCING =
    VALIDATION PENDING

---

## 24. Public / Protected Boundary

The public VRP specification documents:

    security invariants

    threat models

    validation methodology

    externally meaningful verdicts

    reproducible evidence boundaries

It does not disclose:

    protected Core algorithms

    cryptographic secrets

    proprietary recovery logic

    private fencing implementation

    anti-clone mechanisms

    internal authority machinery

The intended separation is:

    PUBLIC:
    WHAT MUST HOLD
    +
    WHAT WAS VALIDATED

    PRIVATE CORE:
    HOW THE PROPERTY IS ENFORCED

---

## 25. Validation Map

The current public document relationship is:

    SESSION_LIFECYCLE_INVARIANTS.md
                |
                | defines
                v
        SECURITY REQUIREMENTS
                |
                v
    SESSION_MANAGER_ADVERSARIAL_VALIDATION.md
                |
                | records
                v
         EXECUTED EVIDENCE
                |
                v
    SESSION_REINCARNATION_ABA_TEST_MODEL.md
                |
                | defines next boundary
                v
        ABA / REINCARNATION TEST

Together these documents separate:

    ARCHITECTURE

    VALIDATION

    EVIDENCE

    UNRESOLVED BOUNDARIES

---

## 26. Final Security Model

VRP session security can be summarized as:

    SESSION != TRANSPORT

    SESSION ID != SESSION LIFETIME

    EVENT VALIDITY != EVENT AUTHORITY

    OBSERVED EVENT != CANONICAL TRANSITION

    OLD AUTHORITY != CURRENT AUTHORITY

    OLD LIFETIME != NEW LIFETIME

The resulting security requirement is:

    TRANSPORT MAY CHANGE
    SESSION MAY CONTINUE

    AUTHORITY MAY CHANGE
    SESSION MAY CONTINUE

    OLD SESSION MAY DIE
    SAME EXTERNAL ID MAY RETURN

    BUT

    OLD AUTHORITY
    OLD EVENTS
    OLD WORKERS
    OLD RUNTIME STATE

    MUST NEVER SILENTLY BECOME
    AUTHORITY OVER THE NEW
    CANONICAL SESSION LIFETIME

That boundary is treated as a first-class VRP security invariant.

---

## 27. Current Verdict

For the recorded validation surface:

    VRP_SESSION_ORDER_DUPLICATE_SURFACE_PRESERVED

For session reincarnation:

    SESSION_REINCARNATION_VALIDATION_PENDING

The latter verdict must not be upgraded until the dedicated adversarial validation has been executed and the evidence preserved.
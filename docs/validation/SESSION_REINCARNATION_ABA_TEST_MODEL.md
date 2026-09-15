# VRP Session Reincarnation / ABA Test Model

**Document Status:** Public Validation Model  
**Scope:** Session lifecycle isolation  
**Implementation:** Protected / Not Disclosed

---

## Purpose

This document describes the public validation model used to test session reincarnation safety in VRP.

The objective is to verify a fundamental lifecycle invariant:

> An event belonging to an old session lifetime must never mutate a newer canonical lifetime that reuses the same logical Session ID.

This class of failure is commonly described as an ABA-style lifecycle problem.

This document describes the tested security property and observable validation results.

It does not disclose protected runtime implementation details.

---

## Core Invariant

VRP treats session identity and transport identity as separate concepts.

A transport may disappear, migrate, reconnect, or be replaced without redefining the canonical session.

The stronger lifecycle requirement is:

> OLD SESSION LIFETIME MUST NEVER MUTATE OR RESURRECT A NEW CANONICAL SESSION LIFETIME.

This remains true even when the logical Session ID is reused.

---

## Threat Model

Consider two different lifetimes of the same logical Session ID.

### Lifetime A

Session ID:

    S

Lifetime:

    A

The session progresses through normal state transitions.

An event belonging to Lifetime A is delayed somewhere in the system.

The original session is then removed.

### Lifetime B

Later, a new canonical session is created using the same logical Session ID:

    Session ID: S
    Lifetime: B

The delayed event from Lifetime A now arrives.

Without lifecycle fencing, the event may appear structurally valid because:

- the Session ID exists again;
- the event type is valid;
- the new state machine may currently allow the requested transition.

This is the ABA attack surface.

---

## Why State Validation Alone Is Insufficient

A stale event is not necessarily an invalid state-machine event.

The adversarial validation intentionally places the new session in a state where the delayed event would otherwise represent a legal transition.

Example:

    New lifetime state:
    DETACH_TRANSPORT

    Delayed old-lifetime event:
    MIGRATION_REQUESTED

A migration request can be meaningful from that state.

Therefore a rejection caused only by the state machine would not demonstrate lifecycle isolation.

The test requires the event to be rejected specifically because it belongs to an obsolete session lifetime.

---

## Historical Defect Reproduction

The adversarial test first reproduced the unsafe condition before lifecycle hardening was applied.

The sequence was conceptually:

    CREATE lifetime A
            |
            v
    ESTABLISH
            |
            v
    ATTACH TRANSPORT
            |
            v
    PATH LOST
            |
            v
    capture delayed MIGRATION_REQUESTED
            |
            v
    REMOVE lifetime A

    CREATE lifetime B
    with the same logical Session ID
            |
            v
    ESTABLISH
            |
            v
    ATTACH TRANSPORT
            |
            v
    PATH LOST
            |
            v
    inject delayed event from lifetime A

The historical RED result demonstrated that logical Session ID equality alone was insufficient to distinguish the two lifetimes.

This result was preserved as engineering evidence rather than hidden by weakening the test.

---

## Required Lifecycle Property

The hardened architecture requires a canonical event to be associated with the specific session lifetime for which it was created.

Conceptually:

    Logical Session ID
            +
    Session Lifetime Identity
            =
    Canonical Event Ownership

An event belonging to an obsolete lifetime must fail lifecycle validation before it can affect the new canonical lifetime.

---

## Adversarial Validation

The corrected adversarial test performs the following sequence:

    1. Create session lifetime A.

    2. Advance lifetime A through valid state transitions.

    3. Capture an event associated with lifetime A.

    4. Remove lifetime A.

    5. Create lifetime B using the same logical Session ID.

    6. Advance lifetime B into a state where the captured event
       would otherwise represent a valid state transition.

    7. Record:
       - canonical state;
       - transition-history length;
       - accepted event-log length.

    8. Inject the stale event from lifetime A.

    9. Require lifecycle rejection.

    10. Verify that the new lifetime remains unchanged.

---

## Isolation Requirements

A successful rejection must preserve all of the following.

### Canonical State Isolation

    state_before == state_after

A rejected old-lifetime event must not move the new canonical state machine.

### Transition History Isolation

    history_before == history_after

The rejected event must not create a canonical transition.

### Accepted Event Log Isolation

    event_log_before == event_log_after

An event rejected by lifecycle fencing must not become an accepted canonical event.

---

## Validation Result

The tested stale event was rejected before it could mutate the newer session lifetime.

Observed properties:

    Stale lifetime rejection        : PASS
    Canonical state isolation       : PASS
    Transition history isolation    : PASS
    Rejected-event log isolation    : PASS
    Same-ID reincarnation isolation : PASS

The validation was repeated under stress and race-sensitive execution.

Representative validation included:

    Targeted adversarial execution : PASS
    Repeated stress execution      : PASS
    Race-sensitive execution       : PASS

The corresponding engineering verdict was:

    VRP_SESSION_REINCARNATION_FENCING_PRESERVED

---

## Restart Boundary

Same-process reincarnation is only one lifecycle boundary.

A stronger test asks what happens when the Session Manager itself is destroyed and recreated.

A purely manager-local incarnation sequence may restart from its initial value.

Conceptually:

    Manager A:
        Session S
        local incarnation = 1

    restart

    Manager B:
        Session S
        local incarnation = 1

Therefore local incarnation alone must not be interpreted as globally restart-safe lifecycle identity.

VRP validates the restart boundary separately.

The public restart model is documented independently so that same-manager ABA isolation and cross-restart lifetime isolation are not incorrectly treated as the same property.

---

## Resource-Boundary Validation

Lifecycle protection must not introduce an unlimited historical-state requirement.

An earlier design retained lifecycle metadata for previously removed Session IDs.

Adversarial unique-ID churn demonstrated that this approach could cause retained lifecycle metadata to grow with the number of historical Session IDs even when no sessions remained active.

The RED boundary was preserved.

The design objective therefore became:

    ABA isolation
            +
    restart separation
            +
    bounded lifecycle metadata

rather than solving one property by sacrificing another.

---

## Fail-Closed Principle

Lifecycle ambiguity must not silently become authority.

If the runtime cannot establish the identity required to distinguish canonical lifetimes, it must not manufacture a weaker fallback identity and continue as though the lifecycle were authenticated.

The corresponding failure behavior is tested separately.

---

## Security Ordering

The required conceptual processing order is:

    incoming event
          |
          v
    session lookup
          |
          v
    lifecycle validation
          |
          +---- invalid / stale ----> REJECT
          |
          v
    accepted canonical event
          |
          v
    state-machine validation
          |
          v
    canonical transition

The critical property is:

> Lifecycle-invalid events are rejected before canonical mutation.

---

## Session Is Not Transport

This validation also reinforces the primary VRP architectural distinction:

    SESSION ≠ TRANSPORT

Transport replacement is expected.

Session reincarnation is a different security boundary.

A transport may change while a session continues.

A new session lifetime, however, must never inherit the authority of stale events merely because it reuses the same logical Session ID.

---

## What This Validation Demonstrates

Under the tested conditions, the validation provides evidence for:

- isolation between sequential lifetimes of the same Session ID;
- rejection of stale events from a previous lifetime;
- preservation of canonical state after rejection;
- preservation of transition history after rejection;
- preservation of accepted event-log state after rejection;
- compatibility with repeated adversarial execution;
- compatibility with race-sensitive execution.

---

## What This Validation Does Not Claim

This document does not claim that:

- every possible lifecycle attack has been exhausted;
- every distributed deployment topology has been validated;
- session lifecycle identity replaces authority epochs;
- session lifecycle identity replaces lease epochs;
- transport identity and session identity are equivalent;
- the complete VRP implementation is publicly disclosed;
- the complete VRP security model is proven solely by this test.

The result applies to the explicitly tested lifecycle invariants.

Additional restart, producer-binding, resource-boundary, malformed-input, concurrency, and recovery surfaces are validated independently.

---

## Protected Implementation Boundary

The public model intentionally describes:

    WHAT must remain invariant
    WHAT attack is performed
    WHAT observable result is required

It does not describe:

    HOW protected runtime internals implement the invariant
    HOW private state is represented
    HOW protected lifecycle material is generated internally
    HOW private runtime coordination operates

Those mechanisms remain part of the protected VRP Core.

---

## Engineering Principle

VRP validation follows a simple rule:

> A security property is stronger when an adversarial test can attempt to violate it and the resulting state can be independently inspected.

For session reincarnation, the required observable outcome is:

    OLD LIFETIME EVENT
            |
            v
         REJECT
            |
            +--> NEW STATE UNCHANGED
            |
            +--> HISTORY UNCHANGED
            |
            +--> ACCEPTED EVENT LOG UNCHANGED

That is the public validation contract for the VRP Session Reincarnation / ABA surface.
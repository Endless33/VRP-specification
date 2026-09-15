# VRP Session Lifetime / Restart Isolation Model

**Document Status:** Public Validation Model  
**Scope:** Session lifetime separation across runtime restart  
**Implementation:** Protected / Not Disclosed

---

## Purpose

This document describes the public VRP validation model for separating session lifetimes across runtime or Session Manager restart boundaries.

The central security requirement is:

> A session created after a runtime restart must not inherit the lifecycle authority of a stale event created before that restart.

This property is stronger than same-manager session reincarnation protection.

A local incarnation sequence may correctly distinguish repeated lifetimes while one manager remains alive and still become insufficient after that manager is destroyed and recreated.

VRP therefore treats same-manager reincarnation and cross-restart lifetime isolation as separate validation surfaces.

---

## Core Lifecycle Invariant

The lifecycle invariant is:

> OLD SESSION / OLD RUNTIME MUST NEVER MUTATE OR RESURRECT A NEW CANONICAL SESSION.

This requirement applies even when:

- the logical Session ID is identical;
- a local incarnation value is reused after restart;
- the stale event is structurally valid;
- the stale event represents a valid state-machine transition;
- the new session has already reached an active recovery or migration state.

---

## The Restart Problem

Consider a manager-local incarnation sequence.

Before restart:

    Manager A

    Session ID  = S
    Incarnation = 1

The runtime terminates.

A new manager is then created.

After restart:

    Manager B

    Session ID  = S
    Incarnation = 1

The local incarnation value has been reused.

This is not necessarily an error in the local counter itself.

It demonstrates that:

    local incarnation
        !=
    complete cross-restart lifetime identity

If an old event is identified only by:

    Session ID + local Incarnation

then an event from Manager A may appear to belong to the new session in Manager B.

That is the restart ABA boundary.

---

## Historical RED Validation

VRP explicitly tested this condition.

The historical test demonstrated:

    PRE-RESTART:
        Session ID  = S
        Incarnation = 1

    POST-RESTART:
        Session ID  = S
        Incarnation = 1

Result:

    LOCAL INCARNATION REUSE ACROSS RESTART: CONFIRMED

The RED result was preserved rather than hidden by changing the test until it became green.

The result established an architectural requirement:

> Restart-safe lifecycle identity cannot rely exclusively on a manager-local incarnation sequence.

---

## Lifecycle Namespace Separation

The hardened public model separates two concepts:

    Lifetime Namespace
            +
    Local Incarnation

Conceptually, canonical lifecycle ownership becomes:

    (Lifetime Namespace, Incarnation)

The local incarnation distinguishes sequential lifetimes inside one manager namespace.

The lifetime namespace separates one manager/runtime lifetime from another.

Example:

    Manager A:
        Lifetime Namespace = A
        Incarnation        = 1

    Manager B:
        Lifetime Namespace = B
        Incarnation        = 1

Although:

    Incarnation A == Incarnation B

the complete lifecycle identities differ:

    (A, 1) != (B, 1)

This is the property required for cross-restart stale-event fencing.

---

## Separation From Authority

Session lifecycle identity is not the same concept as authority generation.

It is also not the same concept as a lease epoch.

The public model deliberately separates:

    session lifecycle identity

from:

    authority generation
    lease epoch
    transport identity
    path identity

These mechanisms protect different invariants.

A lifecycle namespace answers:

> Which canonical lifetime does this event belong to?

An authority epoch answers a different question:

> Which authority lineage is currently permitted to act?

The two must not be silently treated as interchangeable.

---

## Restart Adversarial Test

The restart adversarial validation constructs two independent manager lifetimes.

Conceptually:

    MANAGER A
        |
        +--> create Session S
        |
        +--> lifetime identity = (A, 1)
        |
        +--> advance state machine
        |
        +--> capture stale event
        |
        v
      RESTART

    MANAGER B
        |
        +--> create Session S
        |
        +--> lifetime identity = (B, 1)
        |
        +--> advance state machine
        |
        +--> inject event from (A, 1)

The test intentionally allows the local incarnation value to repeat.

This prevents the test from obtaining a false PASS merely because the numerical incarnation happened to be different.

---

## State-Valid Attack Requirement

The stale event is injected only after the new lifetime reaches a state where the event would otherwise be meaningful.

Representative attack state:

    DETACH_TRANSPORT

Representative stale event:

    MIGRATION_REQUESTED

Therefore the expected rejection must come from lifecycle fencing rather than from an unrelated state-machine error.

This distinction is critical.

A test that rejects a stale event only because the transition is invalid does not prove restart lifetime isolation.

---

## Observed Restart Validation

The tested identities were intentionally constructed with equal local incarnation values and different lifetime namespaces.

Representative model:

    Lifetime A:
        Namespace   = A
        Incarnation = 1

    Lifetime B:
        Namespace   = B
        Incarnation = 1

    Stale event:
        Namespace   = A
        Incarnation = 1

The stale event was delivered to Lifetime B.

Observed result:

    Local incarnation reuse          : CONFIRMED
    Restart namespace separation     : PASS
    Stale lifetime rejection         : PASS
    Canonical state isolation        : PASS
    Transition history isolation     : PASS
    Rejected-event log isolation     : PASS

Engineering verdict:

    VRP_SESSION_RESTART_LIFETIME_FENCING_PRESERVED

---

## Canonical State Isolation

Before stale-event injection, the new lifetime state was recorded.

After rejection, the state was recorded again.

Required property:

    state_before == state_after

Observed:

    PASS

The stale event from the previous runtime lifetime did not move the new canonical state machine.

---

## Transition History Isolation

The canonical transition-history length was recorded before the attack.

Required property:

    history_before == history_after

Observed:

    PASS

The stale event did not become a canonical transition.

---

## Accepted Event Log Isolation

The accepted event-log length was also recorded before the attack.

Required property:

    accepted_events_before
        ==
    accepted_events_after

Observed:

    PASS

The lifecycle-invalid stale event did not enter the accepted canonical event history.

---

## Repeated Validation

The restart attack was not evaluated only once.

The relevant validation surface was repeated under normal and race-sensitive execution.

Representative results:

    Targeted restart adversarial test : PASS
    Repeated restart validation       : PASS
    Race-sensitive validation         : PASS
    Same-manager ABA regression       : PASS

The purpose of repetition is not to claim mathematical proof.

It provides evidence that the tested lifecycle invariant remains stable under repeated execution and concurrency instrumentation.

---

## Entropy Failure Boundary

A lifetime namespace must not silently degrade to a weaker fallback identity when the namespace-generation source fails.

VRP therefore separately validates failure behavior.

The tested failure classes include:

    nil entropy source
    entropy read failure
    incomplete entropy input
    invalid zero namespace

Required behavior:

    REJECT MANAGER CREATION

and:

    NO FALLBACK LIFETIME IDENTITY

Observed:

    Nil entropy source        : REJECT
    Entropy read failure      : REJECT
    Incomplete entropy input  : REJECT
    Zero namespace            : REJECT
    Fallback identity         : NONE

Engineering verdict:

    VRP_SESSION_LIFETIME_ENTROPY_FAILURE_FAIL_CLOSED

---

## Entropy Failure Stress Validation

The entropy failure surface was also repeatedly executed.

Representative validation:

    Targeted entropy validation : PASS
    Stress execution            : PASS
    Race-sensitive execution    : PASS

The tested property is specifically fail-closed behavior.

It does not claim that random namespace generation constitutes a globally monotonic counter.

It does not claim mathematical impossibility of namespace collision.

Those are different properties.

---

## Resource-Bounded Lifecycle Design

Restart isolation must not be achieved by retaining unlimited historical metadata for every Session ID ever observed.

An earlier lifecycle model was adversarially tested with large unique-Session-ID churn.

Conceptually:

    create S1
    remove S1

    create S2
    remove S2

    ...

    create SN
    remove SN

while:

    active sessions -> 0

The test exposed historical lifecycle metadata growth proportional to unique Session IDs.

That result was classified as a resource-boundary failure.

The design requirement therefore became:

    stale lifetime isolation
            +
    restart separation
            +
    bounded historical lifecycle metadata

A security mechanism should not solve ABA by creating an independent unbounded-memory attack surface.

---

## Why Tombstone Deletion Alone Is Not Sufficient

Simply deleting all historical lifecycle information after session removal can reopen ABA reuse.

Conversely, retaining historical state forever can create unbounded resource growth.

The public design problem is therefore not:

    keep history forever

or:

    forget everything immediately

The requirement is:

> Distinguish canonical lifetimes without requiring unlimited per-Session-ID historical retention.

The protected runtime implementation of that requirement is outside the scope of this document.

---

## Production Event Binding

Restart-safe lifecycle identity is useful only if canonical event producers actually bind events to the intended current lifetime.

VRP therefore validates producer boundaries separately.

For trusted post-bootstrap runtime events, the public model is:

    construct event
          |
          v
    bind to current canonical lifetime
          |
          v
    lifecycle validation
          |
          v
    canonical processing

The tested Session Runtime producer follows this model for post-bootstrap events.

---

## Bootstrap Boundary

Session creation is a special lifecycle boundary.

Before a new session is created, no current session lifetime exists to which the creation event can already be bound.

Conceptually:

    CREATE_SESSION
          |
          v
    trusted bootstrap boundary
          |
          v
    allocate canonical lifetime
          |
          v
    bind subsequent events to that lifetime

Therefore bootstrap creation and post-bootstrap event processing must not be confused.

---

## Runtime Producer Validation

During producer migration, an initial implementation attempted to bind the bootstrap creation event before the session existed.

The runtime correctly failed with a missing-session binding error.

This was classified as a producer-boundary setup defect, not as a reason to weaken lifecycle validation.

The production boundary was corrected so that:

    CREATE_SESSION
        -> explicit bootstrap path

while:

    all tested post-bootstrap runtime events
        -> current-lifetime binding
        -> canonical handling

After correction, validation included:

    Runtime targeted tests       : PASS
    Runtime repeated stress      : PASS
    Runtime race validation      : PASS
    Restart fencing regression   : PASS
    Same-manager ABA regression  : PASS
    Entropy regression           : PASS

Engineering verdict:

    SESSION_RUNTIME_LIFETIME_BINDING=PRESERVED

---

## Processing Order

The security-sensitive processing order is conceptually:

    EVENT
      |
      v
    SESSION LOOKUP
      |
      v
    LIFETIME VALIDATION
      |
      +------ stale / invalid ------> REJECT
      |
      v
    ACCEPTED EVENT
      |
      v
    STATE-MACHINE VALIDATION
      |
      v
    CANONICAL MUTATION

The essential requirement is:

> A stale lifetime event must fail before it can mutate the new canonical lifetime.

---

## Same-Manager ABA vs Restart ABA

These are related but distinct validation surfaces.

### Same-Manager Reincarnation

    Manager A
        Session S / incarnation 1
        remove
        Session S / incarnation 2

The manager remains alive.

### Restart Reincarnation

    Manager A
        Session S / incarnation 1

    restart

    Manager B
        Session S / incarnation 1

The local sequence may restart.

Therefore:

    SAME-MANAGER ABA FENCING
        !=
    CROSS-RESTART LIFETIME FENCING

VRP validates both.

---

## Current Publicly Supported Claims

Under the tested conditions, the evidence supports the following statements:

- sequential same-ID session lifetimes can be isolated inside one manager lifetime;
- stale events from an older tested lifetime can be rejected before canonical mutation;
- equal local incarnation values can be separated across tested manager lifetime namespaces;
- rejected stale events preserve the tested new-session canonical state;
- rejected stale events preserve the tested transition history;
- rejected stale events preserve the tested accepted event log;
- tested entropy-source failures fail closed;
- tested entropy failures do not create a fallback lifecycle identity;
- the tested Session Runtime post-bootstrap producer binds events to the current lifecycle identity;
- the previous per-ID retention problem was explicitly reproduced and preserved as RED engineering evidence.

---

## Current Validation Boundary

This document intentionally does not declare the entire session lifecycle surface complete.

Additional hardening remains relevant for:

    remaining production event producers
    legacy unbound event compatibility
    bootstrap creation authority
    delayed unbound creation attempts
    complete Core regression
    extended resource-boundary validation

These surfaces must be evaluated independently before a broader final lifecycle verdict is issued.

---

## What This Model Does Not Claim

This validation does not claim:

- mathematical proof of global lifetime uniqueness;
- globally monotonic lifetime namespaces;
- exhaustion of every restart race;
- validation of every possible distributed deployment;
- that lifecycle identity replaces authority epochs;
- that lifecycle identity replaces lease epochs;
- that all event producers have already completed migration;
- that legacy compatibility has already been removed;
- that bootstrap creation has no remaining attack surface;
- that the complete private implementation is publicly disclosed;
- that the entire VRP Core is proven by this validation alone.

Claims remain limited to explicitly exercised invariants.

---

## Protected Core Boundary

The public model exposes the security contract:

    stale old-runtime event
            |
            v
    lifecycle mismatch
            |
            v
          REJECT
            |
            +--> canonical state unchanged
            |
            +--> history unchanged
            |
            +--> accepted event log unchanged

The private implementation details used to enforce that contract remain protected.

---

## Engineering Principle

VRP lifecycle validation follows three requirements simultaneously:

    SECURITY
        stale lifetime cannot mutate new canonical lifetime

    RECOVERY
        legitimate new lifetime can continue operating

    RESOURCE BOUNDARY
        protection must not require unlimited historical metadata

A lifecycle mechanism that satisfies only one or two of these properties is incomplete.

The target is their intersection.

---

## Final Public Validation Statement

The tested restart model demonstrates that local incarnation reuse does not by itself have to imply lifecycle reuse.

Under the tested conditions, distinct manager lifetime namespaces successfully separated an old session lifetime from a new canonical session using the same logical Session ID and the same local incarnation value.

The stale event was rejected before canonical mutation.

The tested canonical state, transition history, and accepted event log remained unchanged.

The corresponding public engineering verdict is:

    VRP_SESSION_RESTART_LIFETIME_FENCING_PRESERVED

This verdict applies to the tested restart-lifetime fencing surface.

It is not a claim that all remaining session lifecycle boundaries have completed validation.
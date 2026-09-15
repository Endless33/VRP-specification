# VRP Session Reincarnation / ABA Adversarial Test Model

**Document Type:** Adversarial Validation Model  
**Status:** Public Validation Specification  
**Protocol:** Veil Routing Protocol (VRP)  
**Security Surface:** Session Lifecycle / Reincarnation / Delayed Events  
**Validation Class:** ABA / Stale-Lifetime Injection

---

## 1. Purpose

This document defines the adversarial validation model for session reincarnation in VRP.

The test addresses a specific lifecycle boundary:

A session is destroyed.

A new session is later created using the same external Session ID.

An event originating from the previous session lifetime arrives after the new session has become canonical.

The required security property is:

    OLD SESSION EVENT
            MUST NOT
    MUTATE NEW SESSION

This property is independent from ordinary duplicate-event rejection, duplicate creation protection, and cross-session isolation.

---

## 2. Security Invariant

The primary invariant is:

    OLD SESSION
    OLD AUTHORITY
    OLD RUNTIME
            |
            X
            |
    MUST NEVER MUTATE
    OR RESURRECT
            |
            v
    NEW CANONICAL SESSION

More formally:

    SessionID(S1) == SessionID(S2)

does not imply:

    Lifetime(S1) == Lifetime(S2)

Therefore:

    Event(S1) + CurrentSession(S2)

must not automatically imply:

    Apply(Event(S1), S2)

---

## 3. ABA Problem

The ABA pattern occurs when an externally visible identity appears unchanged even though the underlying object or lifecycle has changed.

For VRP:

    Session S1
    SessionID = X
          |
          | Remove
          v
    no active session X
          |
          | Create
          v
    Session S2
    SessionID = X

The external Session ID is identical.

The session lifetime is not.

This distinction is security-critical.

---

## 4. Threat Model

The adversarial event may originate from:

- delayed network delivery,
- transport buffering,
- asynchronous runtime processing,
- a stale relay,
- a stale worker,
- a delayed goroutine,
- retry logic,
- queued control-plane work,
- reordered event delivery,
- stale authority execution.

The delayed event does not need to be maliciously forged.

A legitimate event from an obsolete session lifetime is sufficient to exercise the boundary.

---

## 5. Attack Sequence

The canonical test sequence is:

    1. Create session S1.

    2. Advance S1 through a valid lifecycle.

    3. Construct or capture event E1 belonging to S1.

    4. Delay E1.

    5. Remove S1.

    6. Verify S1 is no longer registered.

    7. Create session S2 using the same external Session ID.

    8. Advance S2 independently.

    9. Place S2 into a state where E1 would otherwise represent
       a syntactically valid state-machine transition.

    10. Deliver E1.

    11. Verify E1 is rejected or contained.

    12. Verify canonical state of S2 remains unchanged.

---

## 6. Compatible-State Requirement

A weak ABA test could produce a false security result.

For example:

    S1 produces MigrationRequested

then:

    S1 removed
    S2 created

If the stale `MigrationRequested` event is delivered while S2 remains in its initial state, the state machine may reject it purely because the transition is out of order.

That would prove transition-order enforcement.

It would not prove session-reincarnation fencing.

The stronger test deliberately advances S2 into a state where the stale event would otherwise be valid:

    S1:
    ACTIVE
      |
      v
    PATH LOST
      |
      v
    E1 = MigrationRequested
         [DELAYED]

    S1 REMOVED

    S2:
    ACTIVE
      |
      v
    PATH LOST
      |
      v
    compatible state

    E1 FROM S1
         |
         v
         X
      MUST REJECT

A successful rejection at this boundary provides substantially stronger evidence.

---

## 7. Required Result

Before stale-event injection:

    State(S2) = X

After stale-event injection:

    State(S2) = X

The stale event must not cause:

    X -> Y

for any unauthorized canonical state Y.

Required successful verdict:

    SESSION_REINCARNATION_FENCING_PRESERVED

If the stale event is accepted and advances S2:

    SESSION_REINCARNATION_FENCING_VIOLATED

Such a result must be treated as a security-relevant lifecycle defect until investigated.

---

## 8. Session ID Is Not Sufficient Authority

The test explicitly validates:

    SESSION ID != SESSION LIFETIME

A Session ID may be necessary for locating a logical session namespace.

It must not automatically constitute proof that an event belongs to the current lifetime occupying that namespace.

The following relationship is forbidden:

    stale lifetime
          +
    matching Session ID
          =
    canonical authority

---

## 9. Authority Generation Is a Separate Dimension

VRP runtime events may carry authority-generation information.

Authority generation and session incarnation answer different questions.

Authority generation asks:

    Which authority lineage is current?

Session incarnation asks:

    Which lifetime of this Session ID is current?

Therefore:

    AUTHORITY GENERATION
            !=
    SESSION INCARNATION

unless the protected implementation explicitly and correctly binds those properties together.

The public specification does not require disclosure of the internal mechanism.

It requires the externally observable property:

    STALE LIFETIME
          |
          X
          |
    CURRENT LIFETIME

---

## 10. Canonical State Requirement

The primary measurement is canonical state.

The test must capture:

    stateBefore

before stale-event delivery.

It must capture:

    stateAfter

after stale-event delivery.

For a correctly contained stale event:

    stateAfter == stateBefore

A returned error alone is insufficient if canonical state was mutated before the error was produced.

Validation must therefore establish both:

    STALE EVENT REJECTED

and:

    CANONICAL STATE UNCHANGED

---

## 11. Event Observation vs Acceptance

A runtime may retain attempted or rejected events as security evidence.

Therefore:

    EVENT OBSERVED
            !=
    EVENT ACCEPTED

and:

    EVENT LOGGED
            !=
    CANONICAL TRANSITION

The security-critical failure condition is unauthorized canonical mutation.

Evidence systems should make the disposition of rejected and accepted events distinguishable wherever external verification depends on that distinction.

---

## 12. Deterministic Single-Event Validation

The first validation phase should use one deterministic stale event.

Example attack class:

    OLD MigrationRequested
            |
            v
    NEW compatible session lifetime

Required result:

    REJECT / CONTAIN

This establishes the fundamental lifecycle boundary before concurrency pressure is introduced.

---

## 13. Repeated Validation

After the deterministic test passes, it should be executed repeatedly.

Target:

    100 consecutive executions

Expected result:

    100 / 100 PASS

The objective is to detect unstable lifecycle behavior that may not appear during one execution.

---

## 14. Shuffle Validation

The test should also be executed under shuffled test ordering where supported.

Target:

    repeated execution
    shuffle enabled

Required result:

    PRESERVED

This helps expose hidden dependence on surrounding test order or shared state.

---

## 15. Race-Detector Validation

The reincarnation boundary should be exercised under the Go race detector.

The race detector does not prove semantic lifecycle correctness.

It provides an additional signal that the tested security property is not accidentally dependent on an unsafe memory race.

Required semantic invariant remains:

    OLD LIFETIME
    MUST NOT MUTATE
    NEW LIFETIME

---

## 16. Concurrent Stale-Event Flood

After deterministic validation succeeds, a stronger validation stage may inject multiple stale operations concurrently.

Conceptually:

    S1 destroyed
         |
         +---- stale E1
         +---- stale E2
         +---- stale E3
         +---- ...
         +---- stale EN
                  |
                  v
            CONCURRENT FLOOD
                  |
                  X
                  |
                 S2

Candidate pressure:

    workers = 256

or:

    workers = 512

Required property:

    ACCEPTED_STALE_CANONICAL_MUTATIONS = 0

and:

    State(S2) remains canonical

The concurrent flood is a second-stage validation.

It must not replace the deterministic proof.

---

## 17. Remove / Recreate Race

A later validation stage should exercise concurrency between:

    Remove(S1)

and:

    Create(S2)

using the same external Session ID.

The objective is to determine whether an ambiguous lifecycle window exists in which:

- old operations remain active,
- new registration becomes visible,
- stale events cross the lifecycle boundary.

Required invariants:

    AT MOST ONE CURRENT CANONICAL LIFETIME

and:

    OLD LIFETIME CANNOT CONTROL NEW LIFETIME

---

## 18. Stale Runtime Boundary

Session-reincarnation validation should eventually include stale runtime references.

Conceptually:

    Runtime R1
       |
       +---- references S1
       |
    S1 destroyed
       |
    S2 created
       |
    delayed R1 operation
       |
       X
       |
    S2

Required property:

    STALE_RUNTIME_MUTATION =
    REJECTED / CONTAINED

This extends ABA validation beyond raw event delivery.

---

## 19. Stale Authority Boundary

A further test should combine session reincarnation with stale authority:

    S1
    Authority A1
         |
         | destroy
         v
    S2
    Authority A2
         ^
         |
    stale A1 operation

Required property:

    A1 MUST NOT MUTATE S2

This validates the composition of:

    SESSION LIFECYCLE FENCING

and:

    AUTHORITY LINEAGE FENCING

rather than assuming either property automatically proves the other.

---

## 20. Cross-Session Isolation

ABA protection must not weaken existing cross-session isolation.

For:

    S1
    S2
    S3

operations against one session must not modify unrelated sessions.

A reincarnation defense must therefore preserve:

    CROSS_SESSION_ISOLATION

while adding:

    CROSS_LIFETIME_ISOLATION

These are complementary security properties.

---

## 21. Failure Handling

If the adversarial test fails:

    STOP

The test must not be weakened merely to obtain a passing result.

Preserve:

    failing test name
    exact failure output
    Git HEAD
    Git Tree
    UTC timestamp
    Go version
    state before stale injection
    state after stale injection
    stale event type
    race result if available

A reproducible failure is engineering evidence.

The next step is diagnosis of the violated invariant.

---

## 22. Production-Fix Rule

No production correction should be introduced before the failing behavior is reproduced and understood.

Required workflow:

    REPRODUCE
        |
        v
    CAPTURE EVIDENCE
        |
        v
    IDENTIFY VIOLATED INVARIANT
        |
        v
    INSPECT LIFECYCLE BOUNDARY
        |
        v
    MINIMAL PRODUCTION CORRECTION
        |
        v
    ORIGINAL ADVERSARIAL TEST
        |
        v
    STRESS
        |
        v
    RACE
        |
        v
    WHOLE-CORE REGRESSION

The original adversarial test must remain intact.

---

## 23. Evidence Requirements

A successful validation record should preserve:

    UTC timestamp

    Git HEAD

    Git Tree

    Go version

    exact adversarial test name

    deterministic result

    repeated result

    shuffle result

    race result

    whole-Core regression result

    source working-tree status

    final verdict

Where an evidence bundle is exported, its contents should be cryptographically bound by a manifest or equivalent integrity mechanism.

---

## 24. Verdict Vocabulary

Use:

    SESSION_REINCARNATION_FENCING_PRESERVED

only when the dedicated reincarnation attack has been successfully executed.

Use:

    SESSION_REINCARNATION_FENCING_VIOLATED

when a stale event from a destroyed lifetime causes unauthorized canonical mutation of a recreated session.

Use:

    SESSION_REINCARNATION_VALIDATION_INCOMPLETE

when the required validation surface was not fully exercised.

Do not convert:

    INCOMPLETE

into:

    PRESERVED

without additional evidence.

---

## 25. Public Claim Boundary

Before successful validation, the correct statement is:

    Session reincarnation fencing is an explicit
    VRP security requirement under adversarial validation.

After successful reproducible validation, the statement may become:

    Session reincarnation fencing was preserved
    under the recorded adversarial validation conditions.

This distinction is intentional.

---

## 26. Security Properties Exercised

The completed deterministic validation is intended to exercise:

    SESSION_REINCARNATION_FENCING

    CROSS_LIFETIME_ISOLATION

    STALE_EVENT_REJECTION

    CANONICAL_STATE_IMMUTABILITY

    SESSION_ID_REUSE_SAFETY

    FAIL_CLOSED_LIFECYCLE_BOUNDARY

Later concurrent extensions additionally exercise:

    STALE_EVENT_FLOOD_CONTAINMENT

    REMOVE_RECREATE_RACE_CONTAINMENT

    STALE_RUNTIME_ISOLATION

    STALE_AUTHORITY_ISOLATION

---

## 27. Final Invariant

The entire adversarial model reduces to one rule:

    OLD SESSION
          |
          | destroyed
          v
          X
          |
          | NO AUTHORITY ACROSS
          | THIS LIFECYCLE BOUNDARY
          v
    NEW SESSION

Even when:

    OLD.SessionID == NEW.SessionID

VRP must preserve:

    OLD LIFETIME != NEW LIFETIME

and therefore:

    OLD EVENT != CURRENT AUTHORITY

The Session ID may be reused.

Authority belonging to the destroyed session lifetime may not.
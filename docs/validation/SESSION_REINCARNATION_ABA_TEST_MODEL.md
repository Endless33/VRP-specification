# VRP Session Reincarnation / ABA Adversarial Test Model

**Status:** Validation Model / Execution Pending  
**Scope:** Public architectural validation  
**Component:** Session lifecycle boundary  
**Protocol:** Veil Routing Protocol (VRP)

---

## 1. Purpose

This document defines the adversarial validation model for session
reincarnation and ABA-style stale-event attacks.

The security invariant under test is:

> OLD SESSION / OLD AUTHORITY / OLD RUNTIME MUST NEVER MUTATE OR
> RESURRECT NEW CANONICAL SESSION.

A SessionID is an external session identifier.

Reuse of the same SessionID does not imply reuse of the same canonical
session lifetime.

Two different lifetimes using the same SessionID must remain
cryptographically and logically isolated at the canonical lifecycle
boundary.

---

## 2. Threat Model

Assume two distinct canonical session lifetimes:

    S1 = old session lifetime
    S2 = new session lifetime

Both may use the same externally visible SessionID.

An event created while S1 was canonical may be delayed because of:

- asynchronous runtime work;
- transport delay;
- scheduler delay;
- queueing;
- retry processing;
- concurrent teardown;
- failover;
- delayed control-plane delivery.

After S1 has been removed, S2 may be created using the same SessionID.

The attacker or delayed runtime then delivers an event originating from S1.

The security question is:

    Can an event belonging to S1 be interpreted as an event belonging
    to S2 merely because both lifetimes use the same SessionID?

The required answer is:

    NO.

---

## 3. Why This Is an ABA Boundary

The external identifier may appear to move through:

    SessionID -> S1 -> removed -> S2

From the perspective of code that observes only SessionID, the identifier
appears unchanged.

Canonical lifetime identity, however, has changed.

Therefore:

    SessionID equality != session lifetime equality

This is distinct from ordinary duplicate creation or duplicate removal.

It is also distinct from authority generation.

Authority generation describes authority evolution within an authority
model.

Session reincarnation fencing describes separation between different
canonical lifetimes.

Passing one invariant does not prove the other.

---

## 4. Adversarial Preconditions

The validation MUST satisfy all of the following conditions.

1. S1 and S2 use the same SessionID.

2. S1 reaches a lifecycle position where a migration request would be
   syntactically valid.

3. A migration event belonging to S1 is created but deliberately delayed.

4. S1 is removed.

5. S2 is created using the same SessionID.

6. S2 is independently advanced to the same lifecycle position where a
   migration request would normally be valid.

7. Only then is the delayed S1 event injected.

Condition 6 is essential.

Without it, the stale event might be rejected merely because the current
state machine does not permit that event.

Such a rejection would be an event-ordering PASS, not proof of
reincarnation fencing.

---

## 5. Abstract Attack Sequence

The required sequence is:

    Create S1
        |
        v
    Init S1
        |
        v
    Attach Transport S1
        |
        v
    Path Lost S1
        |
        v
    Construct E_old = MigrationRequested(S1)
        |
        |   E_old is intentionally delayed
        v
    Remove S1
        |
        v
    Create S2 using SAME SessionID
        |
        v
    Init S2
        |
        v
    Attach Transport S2
        |
        v
    Path Lost S2
        |
        v
    Record canonical S2 state/history
        |
        v
    Inject E_old
        |
        v
    Verify containment

The stale event must not acquire authority over S2.

---

## 6. Required PASS Conditions

The attack is considered contained only when all required canonical
properties remain preserved.

### 6.1 Stale Event Rejection

The delayed S1 event MUST NOT be accepted as an S2 transition.

### 6.2 Canonical State Preservation

The canonical state of S2 before and after stale-event injection MUST be
identical.

Formally:

    State(S2, after E_old) == State(S2, before E_old)

### 6.3 Canonical Transition History Preservation

The accepted canonical transition history of S2 MUST NOT gain a
transition caused by E_old.

### 6.4 Authority Preservation

Old authority or old runtime work MUST NOT acquire authority over S2.

### 6.5 No Resurrection

No component belonging exclusively to S1 may recreate, resume, advance,
or otherwise mutate the canonical lifetime represented by S2.

---

## 7. Event Observation vs Canonical Acceptance

An implementation may retain raw observations for diagnostics or
evidence.

Therefore:

    event observed != event accepted

and:

    event logged != canonical state transition

A rejected stale event may legitimately appear in an observational audit
surface if that surface clearly distinguishes attempted/rejected events
from accepted canonical transitions.

Validation MUST therefore inspect canonical state and accepted transition
history separately from raw event observation.

A raw event-log append alone is not sufficient evidence of canonical
mutation.

However, evidence surfaces SHOULD make rejection disposition explicit so
external verification cannot confuse observation with canonical
acceptance.

---

## 8. FAIL Conditions

The invariant is violated if any of the following occurs.

### 8.1 Stale Event Accepted

The S1 event is accepted after S2 has become canonical.

### 8.2 State Mutation

The stale event changes S2 canonical state.

Example:

    before = DETACH_TRANSPORT
    stale event = MIGRATION_REQUESTED
    after = MIGRATION

when the event belongs to S1 rather than S2.

### 8.3 History Mutation

The stale event adds an accepted transition to S2 canonical transition
history.

### 8.4 Authority Effect

Old authority, old ownership, old runtime work, or an old lifecycle
reference gains canonical effect over S2.

### 8.5 Resurrection

Removed S1 state becomes effective again after S2 exists.

---

## 9. Validation Ladder

A single successful execution is not sufficient for a final preservation
claim.

Required progression:

    deterministic single run
        ->
    repeated shuffled stress
        ->
    race-detector execution
        ->
    whole-Core regression
        ->
    static verification
        ->
    evidence capture

Recommended validation pressure after the first PASS:

    targeted test:
        -count=100
        -shuffle=on

    targeted race:
        -race
        -count=20
        -shuffle=on

Then:

    go test -race -count=1 ./core/...
    go vet ./core/...

The exact pressure values are validation parameters, not protocol
constants.

---

## 10. Evidence Requirements

Execution evidence SHOULD capture:

- UTC timestamp;
- repository HEAD;
- repository TREE;
- Go version;
- operating environment;
- exact test name;
- exact test command;
- S2 state before stale injection;
- S2 state after stale injection;
- S2 history length before injection;
- S2 history length after injection;
- stale event type;
- stale-event rejection result;
- targeted deterministic output;
- shuffled stress output;
- race-detector output;
- whole-Core regression output;
- go vet result;
- git diff/status;
- evidence manifest;
- manifest SHA-256.

Evidence should bind the result to the exact source revision under test.

---

## 11. Failure Handling Rule

If the adversarial test fails:

    DO NOT weaken the test.
    DO NOT delete the failing test.
    DO NOT reinterpret mutation as success.
    DO NOT claim the invariant is preserved.

Instead:

1. preserve the exact failing output;
2. preserve HEAD and TREE;
3. preserve the exact test source;
4. record before/after canonical state;
5. record before/after canonical history;
6. reproduce deterministically;
7. identify the violated lifecycle invariant;
8. design the smallest production correction;
9. rerun the original adversarial test unchanged;
10. run stress, race, and full regression validation.

The failing adversarial case becomes part of the regression surface.

---

## 12. Verdict Vocabulary

Before execution:

    SESSION_REINCARNATION_FENCING=VALIDATION_PENDING

A reproduced violation may be reported as:

    VRP_SESSION_REINCARNATION_FENCING_VIOLATION_OBSERVED

Only after diagnosis and deterministic reproduction should stronger defect
language be used.

A successful single run alone does not justify a final preservation
verdict.

After the required stress, race, regression, and evidence stages pass, the
candidate preservation verdict is:

    VRP_SESSION_REINCARNATION_FENCING_PRESERVED

The verdict applies only to the explicitly tested invariant surface and
validation conditions.

---

## 13. Non-Claims

This validation model does NOT claim that reincarnation fencing is already
implemented.

This document alone proves no implementation property.

The following are also insufficient by themselves:

- presence of an authority-generation field;
- successful stale-owner rejection;
- successful stale-epoch rejection;
- duplicate-create rejection;
- duplicate-remove rejection;
- cross-session removal isolation;
- correct transition ordering;
- race-detector cleanliness.

Those properties are valuable but test different invariant surfaces.

Session reincarnation fencing requires its own adversarial validation.

---

## 14. Relationship to Authority Generation

VRP distinguishes:

    session lifetime
    authority generation
    lease epoch
    transport lifetime

These concepts MUST NOT be treated as interchangeable.

Authority generation may advance while the same canonical session lifetime
continues.

A session lifetime may terminate entirely.

If the external SessionID is later reused, the new lifetime must not inherit
canonical effects from delayed work belonging to the previous lifetime.

Therefore:

    authority freshness != session reincarnation freshness

Both boundaries require independent validation.

---

## 15. Relationship to Session ≠ Transport

The VRP architectural rule:

    SESSION != TRANSPORT

requires session continuity across replaceable transports.

It does not mean that every object carrying the same SessionID belongs to
the same canonical session lifetime.

Transport replacement is continuity inside an authorized session lifetime.

Session reincarnation is creation of a different lifetime.

Those operations require different security semantics.

---

## 16. Public / Protected Boundary

This document specifies:

- the invariant;
- the threat model;
- the adversarial sequence;
- observable PASS/FAIL conditions;
- evidence requirements.

It intentionally does not specify protected Core implementation mechanisms
used to enforce lifecycle fencing.

The public contract is the externally testable security property:

> An event belonging to an obsolete session lifetime cannot mutate or
> resurrect a newer canonical lifetime, even when both use the same
> external SessionID.

---

## 17. Current Status

Current public status:

    TEST_MODEL_DEFINED=YES
    ADVERSARIAL_BOUNDARY_DEFINED=YES
    FALSE_PASS_ORDERING_GUARD=YES
    SESSION_REINCARNATION_FENCING=VALIDATION_PENDING

No preservation claim should be published until execution evidence satisfies
the validation ladder defined above.

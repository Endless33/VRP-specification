# VRP Session Security Validation Index

**Status:** Public Validation Index  
**Protocol:** Veil Routing Protocol (VRP)  
**Scope:** Session lifecycle, authority continuity, adversarial validation  
**Implementation boundary:** Protected Core remains private

---

## 1. Purpose

This document is the public index for VRP session-security requirements
and their corresponding adversarial validation surfaces.

The index separates three different concepts:

1. architectural security requirements;
2. experimentally validated properties;
3. properties that remain pending until their complete validation ladder
   has been executed.

A documented requirement is not automatically a validated implementation
property.

A passing unit test is not automatically evidence of a complete security
property.

VRP validation therefore binds claims to explicit invariants,
adversarial models, reproducible execution, and evidence.

---

## 2. Primary Session Invariant

The central architectural rule remains:

> SESSION != TRANSPORT

Transport state is replaceable.

Session identity and canonical session authority must survive legitimate
transport replacement without allowing stale transport state, stale
authority, replayed work, or an obsolete session lifetime to become
canonical.

---

## 3. Session Lifetime Invariant

A SessionID identifies a session namespace.

A SessionID alone MUST NOT be treated as sufficient proof that an event
belongs to the currently canonical lifetime of that session.

The lifecycle security invariant is:

> OLD SESSION / OLD AUTHORITY / OLD RUNTIME MUST NEVER MUTATE OR
> RESURRECT NEW CANONICAL SESSION.

This requirement applies when the same externally visible SessionID is
reused after an earlier session lifetime has been removed.

---

## 4. Public Document Map

### 4.1 Session lifecycle specification

Path:

`docs/security/SESSION_LIFECYCLE_INVARIANTS.md`

Purpose:

Defines the normative session-lifecycle security requirements.

Primary subjects include:

- Session != Transport;
- session creation and removal;
- duplicate transition rejection;
- event ordering;
- authority separation;
- stale authority containment;
- reincarnation fencing;
- fail-closed lifecycle behavior;
- canonical state protection.

---

### 4.2 Session-manager adversarial validation

Path:

`docs/validation/SESSION_MANAGER_ADVERSARIAL_VALIDATION.md`

Purpose:

Records the adversarial surfaces already exercised around session
creation, removal, isolation, duplicate transitions, ordering, and
concurrency.

The document distinguishes validated surfaces from properties that
remain pending.

---

### 4.3 Session reincarnation / ABA test model

Path:

`docs/validation/SESSION_REINCARNATION_ABA_TEST_MODEL.md`

Purpose:

Defines the adversarial model for delayed work originating from an old
session lifetime and arriving after a new lifetime has been created
under the same SessionID.

The model deliberately places the new lifetime in a state where the
stale event would otherwise be syntactically valid.

This prevents ordinary transition-order rejection from producing a
false security PASS.

---

## 5. Validation Matrix

| Security Surface | Required Property | Status |
|---|---|---|
| Concurrent session creation | One canonical session for one active SessionID | PRESERVED |
| Concurrent session removal | One canonical removal result | PRESERVED |
| Cross-session removal isolation | Removing one session cannot mutate unrelated sessions | PRESERVED |
| Session event ordering | Canonical transition ordering remains deterministic | PRESERVED |
| Duplicate migration completion | Duplicate transition rejected | PRESERVED |
| Duplicate recovery confirmation | Duplicate transition rejected | PRESERVED |
| Recovery before migration completion | Invalid ordering rejected | PRESERVED |
| Migration completion without migration | Invalid ordering rejected | PRESERVED |
| Concurrent failover | Canonical state remains contained under concurrency | PRESERVED |
| Stale authority | Obsolete authority cannot become canonical | PRESERVED on tested authority surfaces |
| Lease epoch takeover | Old epoch cannot resurrect after takeover | PRESERVED on tested lease surfaces |
| Session reincarnation / ABA | Old lifetime cannot mutate new lifetime sharing SessionID | VALIDATION IN PROGRESS |

---

## 6. Duplicate Operations Are Not ABA Fencing

Duplicate-create and duplicate-remove protection are important but
different from session reincarnation protection.

Duplicate creation asks:

> Can multiple concurrent actors create the same currently active
> SessionID?

ABA reincarnation asks:

> Can delayed work from an earlier lifetime affect a later lifetime
> after the old lifetime has already been removed?

A system can correctly reject duplicate creation while still being
vulnerable to stale work crossing a lifetime boundary.

Therefore duplicate-operation validation MUST NOT be cited as proof of
reincarnation fencing.

---

## 7. Authority Generation Is Not Session Incarnation

VRP authority generation and lease epoch provide authority-lineage
protection.

Session incarnation represents a different security dimension.

Authority generation answers questions such as:

- which owner currently has authority;
- whether authority advanced monotonically;
- whether an old owner or old epoch is stale.

Session incarnation answers:

- which lifetime of a reused SessionID produced this work.

These concepts may interact, but they MUST NOT be treated as
interchangeable.

The presence of authority-generation metadata alone is not sufficient
evidence that stale work from an earlier session lifetime is fenced.

---

## 8. ABA Adversarial Sequence

The public abstract attack sequence is:

1. create first canonical lifetime S1;
2. initialize S1;
3. attach transport to S1;
4. move S1 into a migration-eligible failure state;
5. create but delay an S1 migration event;
6. remove S1;
7. create S2 using the same SessionID;
8. independently initialize S2;
9. independently attach S2 transport;
10. move S2 into the equivalent migration-eligible state;
11. inject the delayed S1 event.

The important property is step 10.

S2 is intentionally positioned so that rejection cannot be explained
merely by an invalid state-machine transition.

The lifecycle boundary itself must provide containment.

---

## 9. ABA PASS Conditions

The reincarnation surface can be considered preserved only when stale
work from S1 cannot gain canonical effect over S2.

Required observations include:

- stale S1 work is rejected or otherwise contained;
- S2 canonical state does not advance because of S1;
- S2 canonical transition history does not gain an S1 transition;
- old authority does not gain canonical authority over S2;
- removed S1 cannot be resurrected;
- concurrent execution does not bypass the lifecycle boundary;
- race-detector execution remains clean;
- broader Core regression remains clean.

No single observation substitutes for the complete validation ladder.

---

## 10. ABA FAIL Conditions

A lifecycle violation exists if obsolete S1 work can:

- successfully transition S2;
- alter S2 canonical history;
- acquire canonical authority over S2;
- resurrect state belonging to S1;
- bypass lifecycle fencing because the external SessionID is identical.

A deterministic reproduction of such behavior must be preserved as
defect evidence rather than hidden by weakening the adversarial test.

---

## 11. Observed Events vs Canonical Acceptance

VRP distinguishes observation from authority.

An event may be observed for diagnostics, audit, or evidence purposes
without becoming an accepted canonical transition.

Therefore:

> OBSERVED != CANONICALLY ACCEPTED

A raw event log is not, by itself, proof that an event successfully
changed canonical session state.

Validation reports must distinguish:

- attempted event;
- rejected event;
- accepted transition;
- resulting canonical state.

This distinction is especially important for stale-event and ABA
validation.

---

## 12. Evidence Ladder

A security surface should progress through the following ladder before
a strong preservation verdict is published:

### Stage 1 — Deterministic adversarial reproduction

Execute the exact attack once with explicit before/after state.

### Stage 2 — Repeated stress

Repeat the targeted invariant under shuffled scheduling.

### Stage 3 — Race detection

Execute the targeted attack under the Go race detector.

### Stage 4 — Combined regression

Run adjacent session, authority, migration, and lifecycle surfaces.

### Stage 5 — Whole-Core regression

Execute the complete Core test surface under the required race and
static-analysis checks.

### Stage 6 — Evidence binding

Capture:

- UTC timestamp;
- repository HEAD;
- repository tree identity;
- Go version;
- exact test name;
- exact commands;
- before/after canonical state;
- PASS/FAIL result;
- stress output;
- race output;
- static-analysis output;
- working-tree state;
- evidence manifest;
- manifest hash.

Only after these stages should a strong final verdict be attached to
the implementation state.

---

## 13. Current ABA Verdict Vocabulary

Before complete validation:

`SESSION_REINCARNATION_FENCING=VALIDATION_IN_PROGRESS`

If an adversarial execution exposes the boundary:

`VRP_SESSION_REINCARNATION_FENCING_VIOLATION_OBSERVED`

After deterministic reproduction, diagnosis, minimal correction, and
complete regression:

`VRP_SESSION_REINCARNATION_FENCING_PRESERVED`

The final preservation verdict MUST NOT be emitted merely because the
architecture specifies the property.

---

## 14. Failure Handling Rule

If an adversarial lifecycle test fails:

1. preserve the exact failing test;
2. preserve the exact output;
3. preserve HEAD and tree identity;
4. do not weaken the test;
5. do not redefine the invariant to match existing behavior;
6. identify the violated lifecycle assumption;
7. apply the smallest production correction that restores the
   invariant;
8. rerun targeted stress;
9. rerun race detection;
10. rerun broader Core regression;
11. bind the result into evidence.

A failing adversarial test is engineering information.

It must remain reproducible until the violated invariant is understood.

---

## 15. Relationship to Authority Security

Session lifecycle fencing complements, rather than replaces, VRP
authority protections.

The relevant authority properties include:

- monotonic authority generation;
- stale-owner rejection;
- expired-authority rejection;
- lease epoch monotonicity;
- takeover containment;
- old-epoch resurrection prevention.

The combined requirement is:

> A valid session lifetime must not accept obsolete authority.

and:

> Valid authority from an obsolete session lifetime must not authorize
> mutation of a new canonical lifetime.

Both dimensions must hold.

---

## 16. Relationship to Transport Continuity

Reincarnation fencing does not change the primary VRP continuity model.

A legitimate transport replacement inside the same canonical session
lifetime must preserve the session.

A destroyed session followed by a new session using the same external
identifier is a different lifecycle case.

Therefore:

> TRANSPORT CHANGE != SESSION REINCARNATION

and:

> SESSION ID REUSE != PROOF OF SESSION CONTINUITY

---

## 17. Fail-Closed Requirement

When lifecycle provenance cannot be established with sufficient
confidence, canonical mutation must fail closed.

Ambiguity must not silently grant authority to an event merely because
its SessionID matches the currently active session.

---

## 18. Public / Protected Boundary

This documentation intentionally exposes:

- architectural invariants;
- threat models;
- adversarial sequences;
- validation methodology;
- verdict semantics;
- evidence requirements.

It does not disclose:

- protected runtime internals;
- private lifecycle-fencing implementation;
- private cryptographic material;
- proprietary state-management mechanisms;
- protected operational configuration.

The architecture can be public while the Core implementation remains
protected.

---

## 19. Interpretation Rules

Public VRP validation material follows these rules:

### PRESERVED

The stated invariant survived the documented validation conditions.

### REJECTED

The tested invalid or stale operation was rejected.

### CONTAINED

The adversarial condition occurred but did not gain prohibited
canonical effect.

### VALIDATION IN PROGRESS

The security requirement exists and its dedicated validation surface is
being executed or hardened.

### VIOLATION OBSERVED

An adversarial execution demonstrated prohibited canonical behavior.

These terms describe evidence under stated conditions.

They are not claims of mathematical perfection or universal immunity.

---

## 20. Current Session-Security Position

The validated session-manager surfaces currently provide evidence for:

- duplicate-create containment;
- duplicate-remove containment;
- cross-session removal isolation;
- deterministic event ordering;
- duplicate-transition rejection;
- invalid-order rejection;
- concurrency/race containment on tested surfaces;
- stale-authority containment on tested authority surfaces;
- stale-epoch containment on tested lease surfaces.

Session reincarnation / ABA fencing is tracked independently because it
tests a different invariant:

> identity reuse across time must not collapse two canonical lifetimes
> into one authority domain.

Its final status must be determined by dedicated adversarial execution
and the complete validation ladder.

---

## 21. Summary

VRP session security is evaluated across three independent axes:

**Identity**
- Which session namespace is referenced?

**Lifetime**
- Which incarnation of that session produced the work?

**Authority**
- Which owner / generation / epoch is permitted to mutate canonical
  state?

Secure canonical mutation requires these dimensions to remain
consistent.

The public validation position is therefore:

> SESSION != TRANSPORT

> SESSION ID != SESSION LIFETIME

> AUTHORITY GENERATION != SESSION INCARNATION

> OBSERVATION != CANONICAL ACCEPTANCE

> OLD SESSION / OLD AUTHORITY / OLD RUNTIME MUST NEVER MUTATE OR
> RESURRECT NEW CANONICAL SESSION.

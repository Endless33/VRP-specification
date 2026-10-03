# VRP Validated Guarantees

**Project:** Veil Routing Protocol (VRP)  
**Document Type:** Public Validated Guarantees  
**Status:** Public  
**Validation Scope:** Engineering evidence available as of October 2026

---

## Purpose

This document defines the externally relevant properties of Veil Routing Protocol (VRP) that are supported by engineering validation.

The purpose is to distinguish:

- architectural intent;
- implemented behavior;
- validated behavior;
- deployment-specific requirements.

Only public guarantees and observable validation results are described here.

Protected runtime implementation details are outside the scope of this document.

---

# 1. Validation Principle

VRP follows a simple engineering rule:

> **A guarantee should be supported by observable evidence.**

Architectural statements alone are not treated as sufficient validation.

Where practical, VRP properties are exercised through:

- deterministic regression tests;
- repeated execution;
- concurrency testing;
- race-detector testing;
- failure injection;
- transport interruption;
- migration stress;
- stale-work scenarios;
- replay scenarios;
- authority-transition scenarios;
- long-duration execution;
- evidence verification.

The objective is not to prove that failure is impossible.

The objective is to demonstrate that defined invariants remain preserved or fail in a controlled manner under the conditions exercised.

---

# 2. Guarantee: Session Is Not Transport

The primary VRP architectural guarantee is:

> **Session ≠ Transport**

Session identity is not defined by the currently active:

- socket;
- network interface;
- path;
- relay;
- transport instance.

Transport may change while the higher-level session remains the same continuity object.

This property has been exercised through:

- transport interruption;
- transport replacement;
- Wi-Fi/mobile transitions;
- path migration;
- relay migration;
- network rebinding;
- repeated migration;
- transport recovery.

**Validation status:**

    VALIDATED

---

# 3. Guarantee: Transport Replacement Does Not Require Session Replacement

A transport failure does not automatically require destruction of the logical session.

VRP has been exercised under physical and simulated transport changes.

Observed scenarios include:

- transport becoming unavailable;
- alternative transport becoming available;
- migration to replacement transport;
- restoration of transport reachability.

A physical validation observed:

    TRANSPORT_REACHABILITY=LOST

followed by:

    TRANSPORT_REACHABILITY=RECOVERED

with an observed interruption of approximately:

    41.6 seconds

The continuity model remained distinct from transport reachability.

**Validation status:**

    VALIDATED

---

# 4. Guarantee: Repeated Migration Can Preserve Session Identity

VRP migration behavior has been tested repeatedly rather than only through one successful transition.

A recorded validation completed:

    1,000 transport migrations

while preserving session identity.

Additional migration validation includes:

- path replacement;
- relay changes;
- transport collapse;
- transport recovery;
- degraded network conditions.

**Validation status:**

    VALIDATED

---

# 5. Guarantee: Session Lifetimes Are Distinguishable

Reusing a logical session identifier does not make historical work current again.

VRP maintains lifecycle identity capable of distinguishing different lifetimes of the same logical session.

Validation has exercised:

- session recreation;
- repeated reincarnation;
- delayed historical operations;
- historical migration requests;
- current-lifetime operations.

A dedicated validation produced:

    Sequential incarnations:           128
    Historical delayed operations:     127
    Historical operations accepted:    0
    Historical operations rejected:    127

The current lifetime remained unchanged.

**Validation status:**

    VALIDATED

---

# 6. Guarantee: Stale Historical Work Is Rejected

Historical work from an earlier lifecycle must not automatically receive authority over the current lifecycle.

Validation confirms that stale work can be rejected even when:

- the logical session identifier is the same;
- a new incarnation is active;
- the operation would otherwise be valid for the current session state.

This distinction is important.

The rejection is based on lifecycle validity, not merely whether an operation name is valid in the current state.

**Validation status:**

    VALIDATED

---

# 7. Guarantee: Replay Is Rejected

VRP includes replay resistance as a runtime property.

Validation has exercised:

- duplicate delivery;
- stale sequence delivery;
- repeated replay;
- replay flooding;
- historical lifecycle replay;
- evidence replay scenarios.

A recorded live validation rejected:

    9,999 replay attempts

while preserving session identity.

Separate validation has exercised thousands of replay attempts without allowing them to become accepted current work.

**Validation status:**

    VALIDATED

---

# 8. Guarantee: Invalid State Transitions Are Rejected

VRP uses an explicit canonical session lifecycle.

An event that is invalid for the current canonical state is rejected rather than silently accepted.

Validation verifies that invalid state events do not alter:

- canonical session state;
- accepted transition history;
- canonical event history.

**Validation status:**

    VALIDATED

---

# 9. Guarantee: Rejected Events Do Not Become Canonical History

A rejected event must not appear as though it were successfully accepted.

Recent regression validation specifically verifies:

    rejected state event
            |
            v
    canonical state unchanged
            |
            v
    transition history unchanged
            |
            v
    canonical EventLog unchanged

This maintains consistency between:

- accepted state;
- transition history;
- canonical event history.

**Validation status:**

    VALIDATED

---

# 10. Guarantee: Accepted Canonical Events Are Recorded Once

The inverse property is also validated.

When a valid event is accepted:

- the expected state transition occurs;
- accepted history advances;
- canonical event history advances exactly once.

This creates a consistent relationship between canonical admission and canonical evidence.

**Validation status:**

    VALIDATED

---

# 11. Guarantee: Duplicate Creation Does Not Create Additional Canonical Authority

Duplicate session creation has dedicated regression coverage.

A duplicate create operation is not allowed to silently create additional canonical lifecycle authority or contaminate accepted event history.

Validation confirms preservation of:

- existing canonical state;
- existing lifecycle identity;
- canonical history.

**Validation status:**

    VALIDATED

---

# 12. Guarantee: Terminal Session States Resist Resurrection

Terminal lifecycle behavior has been explicitly exercised.

Validation has covered attempts to apply the complete supported session-event surface against terminal states.

Tested terminal states include:

    SESSION_CLOSED

and:

    SESSION_FAILED

Invalid resurrection attempts were rejected without mutation of:

- canonical state;
- accepted transition history;
- canonical event history.

**Validation status:**

    VALIDATED

---

# 13. Guarantee: Canonical Admission Fails Closed

Canonical event admission requires valid lifecycle context.

Validation verifies:

    unbound event
        -> REJECTED

    stale lifetime
        -> REJECTED

    stale incarnation
        -> REJECTED

    correctly bound current-lifetime event
        -> ACCEPTED when otherwise valid

Missing lifecycle context is not interpreted as implicit current authority.

**Validation status:**

    VALIDATED

---

# 14. Guarantee: Unauthorized Authority Migration Is Rejected

VRP distinguishes connectivity from authority.

A node becoming reachable does not automatically give that node authority to perform canonical migration.

A validated authority scenario demonstrated:

    Current authority: node-A

    Migration requested by node-X
    Result: REJECTED

    Migration requested through valid authority
    Result: ACCEPTED

**Validation status:**

    VALIDATED

---

# 15. Guarantee: Authority Generation Advances Canonically

Authority migration validation has exercised generation changes.

One validated flow observed:

    Initial generation: 1

followed by a valid authority transition resulting in:

    Final generation: 2

The runtime preserved the expected authority lineage.

**Validation status:**

    VALIDATED

---

# 16. Guarantee: Stale Authority Is Rejected

Authority validation has included attempts to reuse authority information that is no longer current.

Validation scenarios include:

- stale authority;
- authority rollback;
- restart races;
- historical authority state.

Stale authority does not automatically regain canonical ownership.

**Validation status:**

    VALIDATED

---

# 17. Guarantee: Authority Recovery Preserves Lineage

VRP authority validation has included recovery and restart scenarios.

Engineering evidence has exercised:

- authority failover;
- authority recovery;
- cross-region restart races;
- contradictory authority observations;
- stale authority resurrection attempts.

The validated objective is preservation of one accepted canonical authority lineage.

**Validation status:**

    VALIDATED

---

# 18. Guarantee: Read Operations Do Not Implicitly Become Canonical Writes

Runtime boundary review has specifically examined whether values returned by read-oriented APIs can accidentally mutate canonical state.

Validated isolation includes:

- managed session representations;
- canonical event-log views;
- event metadata;
- authority representations;
- path-health collections;
- relay collections.

Where mutable exposure was identified, the boundary was corrected and regression coverage added.

**Validation status:**

    VALIDATED FOR REVIEWED SURFACES

---

# 19. Guarantee: Managed Session Reads Are Detached

Returned managed-session representations are detached from canonical session state.

External modification of a returned representation does not directly modify the canonical runtime session.

Validation includes isolation of:

- session lifecycle identity;
- session state representation;
- transition history representation.

**Validation status:**

    VALIDATED

---

# 20. Guarantee: Canonical EventLog Reads Are Isolated

A caller receiving an EventLog representation must not be able to erase or append canonical events merely by modifying the returned read representation.

Regression validation confirms that modifications to returned EventLog views do not directly modify canonical EventLog storage.

**Validation status:**

    VALIDATED

---

# 21. Guarantee: Supported Event Metadata Is Isolated

Event metadata associated with canonical history has dedicated isolation coverage.

Caller-side modification of supported returned metadata values does not directly rewrite canonical event metadata.

**Validation status:**

    VALIDATED

---

# 22. Guarantee: Path Health Reads Are Isolated

Public path-health read operations have been evaluated for mutation isolation.

Validated surfaces include:

    Get
    All
    History

Returned read representations do not provide an accidental direct mutation path into canonical path-health storage.

**Validation status:**

    VALIDATED

---

# 23. Guarantee: Relay List Containers Are Isolated

Returned relay collections use isolated container storage.

Modification of a returned relay list does not directly rewrite the selector's internal relay collection.

**Validation status:**

    VALIDATED

---

# 24. Guarantee: Runtime Transport Identity Is Explicit

Transport objects are active runtime entities.

VRP distinguishes:

    collection isolation

from:

    runtime object identity

Returned transport-list containers are isolated while transport objects may intentionally preserve runtime identity where required.

This behavior has been explicitly classified and regression-tested.

**Validation status:**

    VALIDATED

---

# 25. Guarantee: Runtime Composition Identity Is Explicit

VRP distinguishes read snapshots from intentional composition surfaces.

Runtime manager and authority-manager accessors intentionally expose stable runtime composition identities.

Regression validation confirms that this intentional sharing does not remove canonical lifecycle admission requirements.

Observed behavior:

    Manager identity:            intentionally shared
    Authority identity:          intentionally shared
    Unbound event:               rejected
    Valid bound event:           accepted
    Admission policy:            fail-closed

**Validation status:**

    VALIDATED

---

# 26. Guarantee: Canonical Trace Preserves Lifecycle Identity

Cross-lifetime event traces must allow different session lifetimes to be distinguished.

Publicly relevant trace output includes lifecycle identity sufficient to distinguish historical events belonging to different incarnations.

This prevents a same-session-ID history from becoming visually ambiguous across recreation.

**Validation status:**

    VALIDATED

---

# 27. Guarantee: Recovery Can Return a Session to Active State

Recovery scenarios have exercised movement through degraded or migration-related states back to active operation.

A validated authority/migration scenario concluded with:

    FINAL_SESSION_STATE=SESSION_ACTIVE

after accepted migration and recovery.

**Validation status:**

    VALIDATED

---

# 28. Guarantee: Runtime Pressure Does Not Automatically Break Canonical Continuity

VRP has been exercised under:

- jitter;
- latency variation;
- packet reordering;
- congestion;
- repeated migration;
- transport interruption;
- resource pressure.

The validation objective is preservation or controlled containment of canonical invariants.

Recorded pressure-validation scenarios have completed with continuity preserved or failure contained according to the tested scenario.

**Validation status:**

    VALIDATED UNDER TESTED CONDITIONS

---

# 29. Guarantee: Concurrent Execution Is Regression Tested

VRP concurrency validation includes:

- concurrent session activity;
- concurrent path operations;
- packet identifier generation;
- historical delayed work;
- repeated session reincarnation;
- deterministic concurrency scenarios.

Selected tests are repeatedly executed with Go race detection.

A deterministic concurrency scenario was repeated:

    1,000 times

with consistent successful results.

**Validation status:**

    VALIDATED UNDER TESTED CONDITIONS

---

# 30. Guarantee: Packet Identifier Generation Has Concurrency Coverage

Concurrent packet-identifier behavior has dedicated regression coverage.

A recorded validation generated:

    4,096 packet identifiers

with:

    duplicate identifiers: 0
    sequence gaps:         0

under the tested concurrency scenario.

**Validation status:**

    VALIDATED UNDER TESTED CONDITIONS

---

# 31. Guarantee: Runtime Behavior Is Tested Beyond Short Executions

VRP validation includes extended runtime execution.

Testing has included:

- repeated cycles;
- prolonged network turbulence;
- extended transport interruption;
- repeated transport transitions;
- week-scale validation harness execution.

The recorded long-duration harness repeatedly preserved the evaluated invariants.

**Validation status:**

    VALIDATED UNDER TESTED CONDITIONS

---

# 32. Guarantee: Evidence Integrity Is Checked

VRP validation artifacts are not treated as trustworthy merely because they exist.

Evidence validation has exercised modifications to:

- recorded events;
- summaries;
- manifests;
- hashes;
- historical evidence.

Inconsistent evidence was detected by the corresponding verification process in the tested scenarios.

**Validation status:**

    VALIDATED

---

# 33. Guarantee: Historical Evidence Rewrite Can Be Detected

Historical integrity validation has exercised attempts to modify previously established evidence history.

The validation system detected and rejected inconsistent historical reconstruction in the tested scenarios.

**Validation status:**

    VALIDATED

---

# 34. Guarantee: Evidence Can Be Independently Verified

VRP includes public verification surfaces intended to allow an evaluator to check exposed evidence without access to protected runtime implementation details.

Independent verification has been exercised against generated evidence bundles.

The design objective is:

> **The evaluator verifies observable guarantees rather than trusting an unverifiable claim from the implementation owner.**

**Validation status:**

    VALIDATED

---

# 35. Guarantee: Validation Is Bound to Engineering Context

Engineering evidence should identify enough context to determine what was actually evaluated.

Recorded validation commonly includes information such as:

- source revision;
- source tree identity;
- timestamp;
- runtime environment;
- test result;
- evidence hashes;
- final verdict.

This improves reproducibility and prevents unrelated builds from being silently represented as the evaluated build.

**Validation status:**

    IMPLEMENTED IN VALIDATION WORKFLOW

---

# 36. Guarantee: Failure Is Treated as an Engineering Signal

VRP validation is not designed to hide failed tests.

During engineering qualification, discovered boundary defects have been:

1. reproduced;
2. isolated;
3. corrected;
4. regression-tested;
5. repeatedly re-evaluated.

This process has been used for canonical admission and mutable read-boundary issues.

The presence of a discovered defect is therefore treated as input to hardening rather than as evidence to suppress.

---

# 37. Validation Summary

The current public guarantee matrix is:

    Session ≠ Transport                         VALIDATED

    Transport replacement continuity           VALIDATED

    Repeated migration                          VALIDATED

    Session lifetime isolation                  VALIDATED

    Historical stale-work rejection             VALIDATED

    Replay rejection                            VALIDATED

    Invalid transition rejection                VALIDATED

    Rejected-event history isolation            VALIDATED

    Accepted-event atomicity                     VALIDATED

    Duplicate-create isolation                  VALIDATED

    Terminal-state resurrection rejection       VALIDATED

    Fail-closed lifecycle admission              VALIDATED

    Unauthorized authority rejection            VALIDATED

    Authority generation continuity             VALIDATED

    Stale authority rejection                   VALIDATED

    Authority recovery lineage                  VALIDATED

    Managed-session read isolation              VALIDATED

    Canonical EventLog isolation                VALIDATED

    Event metadata isolation                    VALIDATED

    Path-health read isolation                  VALIDATED

    Relay-list isolation                        VALIDATED

    Transport identity semantics                VALIDATED

    Runtime composition semantics               VALIDATED

    Lifecycle-aware event trace                 VALIDATED

    Runtime recovery                            VALIDATED

    Runtime pressure behavior                   VALIDATED UNDER TESTED CONDITIONS

    Concurrency behavior                        VALIDATED UNDER TESTED CONDITIONS

    Long-duration behavior                      VALIDATED UNDER TESTED CONDITIONS

    Evidence integrity                          VALIDATED

    Historical evidence integrity               VALIDATED

    Independent evidence verification           VALIDATED

---

# 38. Scope of These Guarantees

A validation result applies to the conditions exercised by that validation.

It should not automatically be interpreted as proof of behavior under every possible:

- operating system;
- network;
- application;
- transport;
- hardware platform;
- deployment topology;
- traffic profile;
- failure combination;
- scale.

Environment-specific deployment qualification remains necessary.

---

# 39. What These Guarantees Do Not Mean

These guarantees do not mean:

- networks cannot fail;
- packets cannot be lost;
- physical disconnection disappears;
- applications cannot time out;
- unlimited scale has been demonstrated;
- every transport has identical characteristics;
- every possible implementation defect has been eliminated;
- every deployment has already been certified.

They describe properties supported by the current engineering evidence.

---

# 40. Deployment Interpretation

The validated guarantees provide the engineering basis for controlled deployment evaluation.

Current status:

    Architecture:                  IMPLEMENTED
    Core continuity properties:   VALIDATED
    Canonical admission:          VALIDATED
    Lifetime isolation:           VALIDATED
    Authority behavior:           VALIDATED
    Recovery behavior:            VALIDATED
    Evidence model:               VALIDATED
    Controlled deployment:        READY FOR EVALUATION
    Environment qualification:    REQUIRED
    Universal certification:      NOT CLAIMED

---

# 41. Overall Statement

The current VRP engineering record supports a concrete set of validated continuity properties.

The strongest public conclusion is not that VRP makes networks infallible.

It is that VRP has an implemented and repeatedly exercised model for preserving canonical session continuity while the transport underneath that session changes, fails, recovers, or is replaced.

That model is supported by engineering evidence covering:

- lifecycle isolation;
- replay rejection;
- migration;
- authority;
- canonical admission;
- runtime recovery;
- concurrency;
- evidence integrity;
- physical network interruption;
- long-duration execution.

These guarantees provide the technical basis for controlled pilot and deployment evaluation.

---

## Public Scope

This document describes externally relevant guarantees and their validation status.

Protected runtime implementation details are intentionally outside its scope.

---

**Veil Routing Protocol (VRP)**  
**Public Engineering Documentation — October 2026**
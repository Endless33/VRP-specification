# VRP Validated Guarantees

**Project:** Veil Routing Protocol (VRP)  
**Status:** Public engineering guarantees  
**Updated:** October 2026

---

## 1. Purpose

This document defines the VRP properties that currently have implementation-backed validation.

It is deliberately written as a list of bounded engineering guarantees rather than a broad security or reliability claim.

For each guarantee, the relevant questions are:

1. What property is expected?
2. What failure would violate it?
3. What has actually been exercised?
4. What limitation remains?

The word **validated** means:

> The current implementation has been exercised against one or more scenarios specifically intended to violate the stated property, and the expected invariant was preserved under those tested conditions.

It does not mean universal proof.

---

## 2. Guarantee Model

VRP validation separates several properties that are often incorrectly collapsed into one statement such as "the connection survived."

The main categories are:

```text
SESSION IDENTITY

LIFETIME IDENTITY

TRANSPORT IDENTITY

CANONICAL STATE

AUTHORITY

REPLAY / DUPLICATION

RECOVERY

EVIDENCE
```

Success in one category does not automatically establish success in another.

For example:

```text
successful transport migration
    !=
proof of replay resistance
```

and:

```text
valid evidence hash
    !=
proof of runtime correctness
```

This separation is intentional.

---

## 3. Guarantee: Session Is Not Transport

### Property

A VRP session is not identified solely by its current transport.

Changing the active transport does not, by itself, create a new logical session.

Conceptually:

```text
SESSION S
   |
TRANSPORT A
```

may become:

```text
SESSION S
   |
TRANSPORT B
```

without requiring:

```text
SESSION S -> SESSION S2
```

### Validation

This property has been exercised through:

- transport interruption;
- transport replacement;
- path migration;
- network rebinding;
- repeated migration;
- recovery.

A recorded stress scenario completed 1,000 migrations while preserving session identity.

### Failure condition

This guarantee would be violated if a supported transport change unexpectedly caused the runtime to replace or merge logical session identity.

### Status

```text
SESSION_TRANSPORT_SEPARATION=VALIDATED
```

---

## 4. What This Guarantee Does Not Mean

`Session ≠ Transport` does not mean that transport-independent identity is unique to VRP.

Existing technologies provide related mechanisms, including:

- QUIC connection migration;
- Multipath TCP;
- tunnel/VPN endpoint roaming;
- application-level reconnect and failover.

VRP's claim is narrower:

> The current VRP runtime preserves its own logical session identity across the supported transport-change scenarios that have been tested.

The broader VRP model additionally associates that session with explicit lifecycle, canonical-state and authority rules.

---

## 5. Guarantee: Transport Failure Does Not Automatically Destroy Session Identity

### Property

Loss of the current transport is not automatically equivalent to destruction of the logical session.

### Validation

A physical network interruption produced observable transport loss followed by recovery.

The recorded outage was approximately 41.6 seconds.

The duration itself is not the guarantee.

The relevant observation is:

```text
TRANSPORT_REACHABILITY=LOST
```

followed by:

```text
TRANSPORT_REACHABILITY=RECOVERED
```

while logical session continuity remained distinguishable from physical connectivity.

### Failure condition

The guarantee would fail if loss of a replaceable transport necessarily destroyed the session under a scenario where continuity was expected.

### Status

```text
TRANSPORT_FAILURE_SEPARATION=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 6. Guarantee: Session Lifetimes Are Distinguishable

### Property

The same logical session identifier may exist across multiple lifetimes.

Those lifetimes are not interchangeable.

VRP represents lifecycle identity using information that includes:

```text
LifetimeEpoch
Incarnation
```

The conceptual current-lifetime identity is therefore stronger than `SessionID` alone.

### Failure condition

A historical operation must not become current merely because:

```text
historical.SessionID == current.SessionID
```

### Status

```text
LIFETIME_IDENTITY=IMPLEMENTED_AND_VALIDATED
```

---

## 7. Guarantee: Historical Work Is Rejected

### Property

Work associated with an older session lifetime must not silently mutate the current lifetime.

### Validation

A dedicated scenario created:

```text
TOTAL_INCARNATIONS=128
```

for one logical session identity.

After incarnation 128 became current, operations associated with the 127 historical incarnations were presented concurrently.

Result:

```text
HISTORICAL_OPERATIONS=127
HISTORICAL_ACCEPTED=0
HISTORICAL_REJECTED=127
CURRENT_LIFETIME_PRESERVED=YES
```

The current state was intentionally selected so that the operation type could otherwise be acceptable.

This distinguishes lifecycle rejection from simple state-machine rejection.

### Failure condition

Any historical operation becoming accepted solely because it targets the same logical SessionID would violate this guarantee.

### Status

```text
STALE_LIFETIME_REJECTION=VALIDATED
```

---

## 8. Guarantee: Replay Does Not Become New Canonical Work

### Property

Previously observed work must not become a new canonical operation merely because it is delivered again.

### Validation

Replay validation includes:

- duplicate delivery;
- stale delivery;
- replay flooding;
- historical-lifetime replay.

One recorded run produced:

```text
REPLAY_ATTEMPTS=9999
REPLAY_REJECTED=9999
REPLAY_ACCEPTED=0
```

### Failure condition

A replay that creates a new canonical state transition or duplicate canonical execution would violate this guarantee.

### Status

```text
REPLAY_REJECTION=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 9. Guarantee: Invalid State Transitions Are Rejected

### Property

An event that is invalid for the current canonical state does not become an accepted state transition.

### Validation

State-machine regression coverage exercises valid and invalid events across session lifecycle states.

Terminal-state testing additionally exercises the supported event alphabet against terminal states.

### Failure condition

An invalid event changing canonical state would violate this guarantee.

### Status

```text
STATE_TRANSITION_VALIDATION=VALIDATED
```

---

## 10. Guarantee: Rejection Is Canonically Atomic

Rejecting an operation must mean more than "the final state happened to remain unchanged."

### Property

For a rejected state operation:

```text
STATE_DELTA=0
HISTORY_DELTA=0
CANONICAL_EVENT_DELTA=0
```

For an accepted operation:

```text
STATE_DELTA=EXPECTED
HISTORY_DELTA=1
CANONICAL_EVENT_DELTA=1
```

### Why this matters

A runtime could reject an operation at the state-machine level while accidentally recording it as accepted historical evidence.

That would create disagreement between state and history.

### Validation history

Engineering testing found an ordering defect in this boundary: a current-lifetime but state-invalid event could enter the canonical EventLog before final state-machine rejection.

The admission order was corrected.

Dedicated regression coverage now verifies both acceptance and rejection behavior.

### Status

```text
CANONICAL_ACCEPTANCE_ATOMICITY=VALIDATED
```

---

## 11. Guarantee: Duplicate Session Creation Does Not Create Competing Canonical State

### Property

Attempting to create an already-existing logical session must not silently create an additional competing canonical lifecycle.

### Validation

Dedicated duplicate-create regression coverage verifies preservation of the existing canonical session and its associated history.

### Failure condition

Duplicate creation producing additional current authority or contradictory canonical history would violate this guarantee.

### Status

```text
DUPLICATE_CREATE_ISOLATION=VALIDATED
```

---

## 12. Guarantee: Terminal States Resist Unsupported Resurrection

### Property

Terminal states are absorbing with respect to unsupported ordinary lifecycle events.

Validated terminal states include:

```text
SESSION_CLOSED
SESSION_FAILED
```

### Validation

The supported session-event surface was exercised against terminal sessions.

Invalid resurrection attempts produced no mutation of:

```text
canonical state
transition history
canonical event history
```

### Important distinction

This does not prohibit an explicitly supported recovery mechanism.

It separates:

```text
explicit recovery
```

from:

```text
accidental resurrection by stale or invalid work
```

### Status

```text
TERMINAL_RESURRECTION_REJECTION=VALIDATED
```

---

## 13. Guarantee: Canonical Admission Fails Closed

### Property

Incomplete, stale or invalid lifecycle context is not interpreted as implicit authorization.

Representative behavior:

```text
missing lifetime binding -> REJECTED

stale lifetime          -> REJECTED

stale incarnation       -> REJECTED

invalid transition      -> REJECTED

replay                  -> REJECTED
```

### Failure condition

Unknown or stale lifecycle context being silently treated as current would violate this guarantee.

### Status

```text
CANONICAL_ADMISSION=FAIL_CLOSED
```

---

## 14. Guarantee: Connectivity Is Not Authority

### Property

A reachable node, path or transport does not automatically gain authority to alter canonical session state.

This distinction is fundamental:

```text
REACHABLE != AUTHORIZED
```

### Validation

Authority scenarios have exercised both unauthorized and authorized transitions.

A representative result is:

```text
UNAUTHORIZED_TRANSITION=REJECTED
AUTHORIZED_TRANSITION=ACCEPTED
```

### Status

```text
CONNECTIVITY_AUTHORITY_SEPARATION=VALIDATED
```

---

## 15. Guarantee: Authority Progression Is Ordered

### Property

Accepted authority progression follows a canonical lineage.

Historical authority does not automatically regain current status.

A representative validated transition observed:

```text
GENERATION=1
    |
    v
GENERATION=2
```

### Validation

Engineering scenarios include:

- valid authority transition;
- stale authority;
- rollback attempts;
- restart races;
- authority recovery.

### Failure condition

An older authority generation silently replacing newer accepted authority would violate this guarantee.

### Status

```text
AUTHORITY_LINEAGE=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 16. Guarantee: Recovery Does Not Disable Admission Rules

### Property

Recovery remains subject to lifecycle, replay and authority validation.

The following do not become valid merely because the runtime is recovering:

```text
stale work
replayed work
invalid transition
unauthorized authority
```

### Validation

Recovery scenarios have been exercised together with:

- transport failure;
- migration;
- stale work;
- authority changes;
- restart conditions.

### Status

```text
RECOVERY_ADMISSION=VALIDATED_UNDER_TESTED_CONDITIONS
```

---

## 17. Guarantee: Current State and Historical Lineage Are Distinct Views

VRP exposes two different concepts that should not be confused.

### Current state

Answers:

> What is authoritative now?

### Historical lineage

Answers:

> What accepted lifecycle events occurred across the session's history?

Historical lineage may contain events associated with multiple session lifetimes.

Current-state access is scoped to the current lifetime.

This distinction is intentional.

It allows history to remain observable without treating historical events as current authority.

---

## 18. Guarantee: Lifecycle Identity Is Present in Canonical Trace Output

### Property

When a trace spans more than one lifetime, lifecycle identity must remain externally distinguishable.

### Validation

Runtime trace output was hardened to include lifecycle information equivalent to:

```text
lifetime_epoch
incarnation
```

Dedicated regression coverage verifies that cross-lifetime traces preserve this distinction.

### Failure condition

A trace that merges multiple lifetimes into an ambiguous sequence without lifecycle identity would weaken reviewability and violate the tested observability property.

### Status

```text
TRACE_LIFETIME_OBSERVABILITY=VALIDATED
```

---

## 19. Guarantee: Managed Session Reads Do Not Directly Mutate Canonical State

### Property

Obtaining a managed-session representation through a read-oriented API must not provide an unintended path to rewrite the canonical session.

### Validation history

Engineering testing demonstrated that returned managed-session objects could previously expose live mutable state.

The boundary was corrected by returning detached representations.

Regression coverage verifies that caller-side mutation does not directly alter canonical session state.

### Status

```text
MANAGED_SESSION_READ_ISOLATION=VALIDATED
```

---

## 20. Guarantee: Canonical EventLog Reads Are Isolated

### Property

Reading canonical event history must not provide a direct capability to erase or append canonical runtime history.

### Validation history

Testing reproduced a mutable EventLog exposure where caller-side mutation could affect canonical stored history.

The read boundary was corrected.

Regression coverage verifies isolation for the supported EventLog read surface.

### Status

```text
EVENTLOG_READ_ISOLATION=VALIDATED
```

---

## 21. Guarantee: Supported Event Metadata Reads Are Isolated

### Property

Returned event metadata must not provide an unintended direct mutation path into canonical stored metadata.

### Validation

A separate regression identified nested top-level metadata/data aliasing at the event-log boundary.

The supported boundary was corrected and tested.

### Limitation

This guarantee applies to the supported production value shapes covered by the runtime boundary.

It is not a claim that arbitrary recursively nested user-defined object graphs are automatically deep-cloned.

### Status

```text
EVENT_METADATA_ISOLATION=VALIDATED_FOR_SUPPORTED_VALUES
```

---

## 22. Guarantee: Path-Health Reads Are Isolated

### Property

Reading path-health state does not provide an unintended write path into the monitor's canonical stored values.

Reviewed surfaces include:

```text
Get
All
History
```

### Status

```text
PATH_HEALTH_READ_ISOLATION=VALIDATED
```

---

## 23. Guarantee: Relay List Container Reads Are Isolated

### Property

Mutating a returned relay-list container must not rewrite the selector's internal list.

### Validation history

Testing identified a live-slice exposure at this boundary.

The implementation was corrected and regression coverage added.

### Status

```text
RELAY_LIST_ISOLATION=VALIDATED
```

---

## 24. Guarantee: Transport Registry Container and Transport Identity Have Explicit Semantics

Not every returned runtime object should automatically be deep-cloned.

Transport objects represent active runtime entities.

The validated boundary is therefore:

```text
returned list container = detached

transport object identity = intentionally preserved
```

This distinction is explicit and regression-tested.

### Status

```text
TRANSPORT_REGISTRY_LIST_SEMANTICS=VALIDATED
```

---

## 25. Guarantee: Runtime Composition Uses Intentional Shared Identity

The main runtime exposes composition components whose identity is intentionally shared.

Validated behavior includes:

```text
runtime.Manager()   -> stable manager identity

runtime.Authority() -> stable authority-manager identity
```

This is not treated as a detached read API.

The security boundary instead remains canonical admission.

A direct unbound canonical event is rejected even though the manager identity is intentionally accessible through runtime composition.

### Status

```text
RUNTIME_COMPOSITION_IDENTITY=EXPLICIT_AND_VALIDATED
```

---

## 26. Guarantee: Concurrency Does Not Automatically Break the Tested Invariants

### Validation

Concurrency-oriented tests cover scenarios including:

- multiple sessions;
- delayed historical work;
- packet identifier generation;
- repeated reincarnation;
- path activity;
- recovery.

A deterministic concurrency scenario completed:

```text
1000 repeated executions
```

with consistent successful results.

Selected regression families have also been exercised under Go race detection.

### Limitation

This does not prove every possible scheduler interleaving.

### Status

```text
CONCURRENCY_INVARIANTS=VALIDATED_UNDER_TESTED_SCENARIOS
```

---

## 27. Guarantee: Packet Identifier Generation Has Been Exercised Concurrently

A dedicated concurrency validation generated:

```text
4096 packet identifiers
```

with:

```text
DUPLICATES=0
GAPS=0
```

under the tested scenario.

This result applies to that validation scope and is not presented as a universal throughput benchmark.

---

## 28. Guarantee: Defined Invariants Survive Tested Runtime Pressure

VRP has been exercised under:

- latency variation;
- jitter;
- packet reordering;
- congestion;
- repeated migration;
- temporary transport loss.

The validation objective is not to claim perfect packet delivery under arbitrary failure.

The objective is that the defined canonical invariants remain preserved or failure remains contained.

### Status

```text
PRESSURE_INVARIANTS=VALIDATED_UNDER_TESTED_SCENARIOS
```

---

## 29. Guarantee: Long-Duration Behavior Has Been Exercised

Recorded validation includes:

- prolonged network turbulence;
- repeated network transitions;
- extended link interruption;
- repeated runtime cycles;
- week-scale harness execution.

These runs increase confidence against failures that require accumulated state or extended execution to appear.

They do not establish an unlimited-duration guarantee.

### Status

```text
LONG_DURATION_EXECUTION=VALIDATED_UNDER_RECORDED_RUNS
```

---

## 30. Guarantee: Evidence Modification Can Be Detected

### Property

Modification of evidence inputs should not silently remain equivalent to the original validated evidence.

### Validation

Engineering scenarios have deliberately modified:

- event evidence;
- summary information;
- manifest-bound data;
- historical evidence.

The tested verification paths detected resulting inconsistencies.

### Status

```text
EVIDENCE_MODIFICATION_DETECTION=VALIDATED
```

---

## 31. Guarantee: Historical Evidence Rewrite Can Be Detected

### Property

Previously established evidence history must not be silently replaceable by an inconsistent reconstructed history.

### Validation

Historical rewrite scenarios were executed and detected by the corresponding verification workflow.

### Status

```text
HISTORICAL_EVIDENCE_INTEGRITY=VALIDATED
```

---

## 32. Guarantee: Selected Evidence Can Be Verified Independently of Runtime Execution

### Property

An evidence verifier can evaluate selected exposed validation properties separately from the runtime execution that originally generated the evidence.

### What this can include

Depending on the scenario:

- artifact consistency;
- manifest consistency;
- counters;
- lifecycle identity;
- replay/stale rejection results;
- migration result;
- authority progression;
- historical consistency;
- final verdict derivation.

### What it does not mean

```text
independent evidence verification
    !=
independent source audit
```

and:

```text
independent evidence verification
    !=
formal proof of runtime correctness
```

### Status

```text
INDEPENDENT_EVIDENCE_VERIFICATION=VALIDATED
```

---

## 33. Guarantee: Evidence Is Bound to Engineering Context

Important validation runs can include identifiers such as:

```text
UTC
source revision
source tree
runtime version
environment
scenario
```

This provides a relationship between the reported result and the engineering state that produced it.

A build identifier does not prove correctness.

It makes the result more precisely attributable and reproducible.

---

## 34. Defects Are Part of the Validation Record

Several useful guarantees in this document became stronger because testing first demonstrated that the previous boundary was insufficient.

Examples include:

- canonical EventLog admission ordering;
- mutable EventLog exposure;
- managed-session mutable aliasing;
- event metadata aliasing;
- relay-list container exposure;
- lifecycle ambiguity in trace output.

The engineering process was:

```text
identify
   |
reproduce
   |
correct
   |
regression-test
   |
requalify
```

The existence of a past defect is not itself evidence of the current guarantee.

The reproduced failure plus retained regression coverage is what makes the current boundary testable.

---

## 35. Guarantees Are Scope-Bound

Every guarantee in this document should be interpreted as:

> **validated under the scenarios and implementation boundaries that were actually exercised.**

It should not be expanded into:

> **impossible to violate under every conceivable environment or future implementation.**

This distinction applies especially to:

- concurrency;
- performance;
- cryptography;
- scale;
- deployment configuration;
- application behavior.

---

## 36. Relationship to Formal Verification

The current guarantees are based on engineering validation.

Methods include:

- unit and regression tests;
- adversarial scenarios;
- deterministic repetition;
- race detection;
- failure injection;
- real network interruption;
- long-duration execution;
- evidence verification.

Unless a specific artifact states otherwise, these guarantees are not claims of complete formal verification using systems such as TLA+, Coq, Lean or Isabelle.

---

## 37. Relationship to Existing Protocols

Some individual VRP capabilities overlap with existing technologies.

That is expected.

Examples:

```text
QUIC             -> connection migration / connection identity mechanisms

Multipath TCP    -> multiple path transport

WireGuard-style
roaming systems  -> endpoint changes

Applications     -> reconnect / retry / failover logic
```

VRP should not be evaluated by claiming that these mechanisms do not exist elsewhere.

The relevant VRP boundary is the combination of:

```text
logical session continuity
        +
explicit lifecycle identity
        +
canonical state admission
        +
authority lineage
        +
stale/replay rejection
        +
replaceable transport
        +
reviewable evidence
```

Whether that combination provides value for a particular system is an integration question, not something this document attempts to decide for the reader.

---

## 38. Compact Guarantee Matrix

| Property | Current public status |
|---|---|
| Session / transport separation | Validated |
| Physical transport-loss continuity | Validated under tested conditions |
| Repeated migration | Validated |
| Lifetime identity | Validated |
| Historical-work rejection | Validated |
| Replay rejection | Validated |
| Invalid-transition rejection | Validated |
| Canonical acceptance atomicity | Validated |
| Duplicate-create isolation | Validated |
| Terminal resurrection rejection | Validated |
| Fail-closed admission | Validated |
| Connectivity / authority separation | Validated |
| Authority lineage | Validated under tested conditions |
| Recovery admission | Validated under tested conditions |
| Cross-lifetime trace observability | Validated |
| Managed-session read isolation | Validated |
| EventLog read isolation | Validated |
| Supported event metadata isolation | Validated |
| Path-health read isolation | Validated |
| Relay-list isolation | Validated |
| Transport-registry list semantics | Validated |
| Runtime composition boundary | Validated |
| Concurrency scenarios | Validated under tested conditions |
| Evidence modification detection | Validated |
| Historical evidence integrity | Validated |
| Independent evidence verification | Validated |

---

## 39. What Is Not Guaranteed by This Document

This document does not guarantee:

```text
zero packet loss

zero application disruption

zero runtime defects

unlimited scalability

identical behavior on every operating system

correctness under every network topology

correctness under every possible concurrency schedule

automatic security of deployment credentials or hosts

formal correctness of the entire implementation

universal production readiness
```

Those require separate qualification.

---

## 40. Deployment Interpretation

The guarantees documented here are sufficient to justify controlled integration evaluation.

A target deployment should reproduce the guarantees relevant to its own environment rather than relying only on project-level results.

The appropriate progression is:

```text
project validation
       |
       v
target integration
       |
       v
environment-specific failure tests
       |
       v
evidence
       |
       v
deployment decision
```

---

## Conclusion

The current VRP implementation has validation evidence for a defined set of continuity, lifecycle, canonical-state, authority, recovery and evidence-integrity properties.

The strongest useful interpretation is not:

> "VRP cannot fail."

It is:

> **VRP defines specific invariants around session continuity and canonical authority, and the current implementation has been deliberately exercised against failure conditions intended to violate those invariants.**

Those guarantees remain bounded by the scenarios, builds and environments in which they are validated.

# Veil Routing Protocol (VRP)

**Continuity First. Session ≠ Transport. Evidence Before Claims.**

**Public Engineering Specification | Active Development | Protected Runtime**

---

## Engineering Overview

Veil Routing Protocol (VRP) is a continuity-first runtime architecture for distributed systems operating across changing, unreliable, or interrupted communication infrastructure.

VRP is built around a fundamental separation:

> **A Logical Session is not its transport connection.**

IP addresses change. Network interfaces disappear. NAT bindings expire. Routes fail. Relays become unavailable. Communication paths recover.

These events should not automatically invalidate the identity of an application-level session.

VRP explores a runtime model in which Logical Session identity remains stable while the underlying transport infrastructure evolves, subject to explicit correctness, security, and recovery constraints.

The objective is not uninterrupted connectivity under every failure condition.

The objective is **controlled continuity: preserve valid session state, reject invalid transitions, and make recovery decisions observable and reproducible.**

The public specification describes the architecture, invariants, validation methodology, security boundaries, and engineering evaluation process.

The proprietary runtime implementation remains private.

---

## Project Status

| Component | Status |
|---|---|
| Public architectural specification | Available |
| Public engineering documentation | Available |
| Public validation methodology | Available |
| Public verification framework | Available |
| Public security and threat-model documentation | Available |
| Protected runtime implementation | Private |
| Runtime hardening | Active development |
| Adversarial and concurrency validation | Ongoing |
| Independent technical review | Supported through public materials |
| Private engineering evaluation | Invitation only |
| General production certification | Not established |

**Important:** Published architecture, successful demonstrations, and scenario-bounded validation do not constitute unrestricted production-readiness certification.

Engineering claims must be evaluated against the exact implementation, version, environment, workload, and failure conditions under test.

---

# 1. The Problem: Transport Failure Is Not Necessarily Session Failure

Modern distributed systems operate across increasingly dynamic infrastructure.

Examples include:

- Mobile networks transitioning between Wi-Fi and cellular connectivity
- Telecommunications infrastructure experiencing route or gateway changes
- Cloud applications moving across network boundaries
- Edge nodes operating under intermittent connectivity
- Industrial systems experiencing temporary network isolation
- Multi-region platforms recovering from infrastructure failure
- Satellite links with changing availability
- Distributed applications experiencing NAT rebinding or relay failure

Traditional connection-oriented application designs may associate session lifetime with a particular connection, endpoint, or transport instance.

When that transport fails, the application may be forced to reconnect, reauthenticate, reconstruct state, retry operations, or resolve uncertain execution outcomes.

These operations introduce complexity.

In stateful systems, careless retries may also introduce duplicate execution, inconsistent authority, or replay exposure.

VRP addresses this problem at the runtime architecture level.

**A transport interruption is an event that the runtime must evaluate. It is not automatically proof that the Logical Session has ended.**

---

# 2. The Core Architectural Principle

## SESSION ≠ TRANSPORT

VRP separates three concerns:

**Logical Session Identity**

The stable identity and lifecycle of the application-level session.

**Execution and Authority State**

The canonical rules governing accepted state transitions, ownership, replay protection, and execution decisions.

**Transport Infrastructure**

The replaceable communication paths through which session traffic is delivered.

This separation allows the runtime to reason about transport changes without automatically destroying valid logical state.

### Architectural invariant

A change in network path must not, by itself, authorize a change in session identity or canonical execution state.

### Security invariant

An observed endpoint, packet, transport, or recovery signal must not acquire execution authority merely because it is reachable.

### Recovery invariant

Recovery must preserve canonical correctness or fail closed when correctness cannot be established.

---

# 3. What VRP Is

VRP is a continuity-oriented runtime architecture designed to coordinate:

- Logical Session lifecycle
- Transport-independent session identity
- Canonical authority progression
- Deterministic state transitions
- Replay containment
- Transport migration
- Recovery decisions
- Execution admission
- Evidence generation
- Adversarial validation

It is intended to operate alongside existing communication technologies rather than replace the Internet protocol stack.

## What VRP Is Not

VRP is not presented as:

- A replacement for TCP
- A replacement for UDP
- A replacement for QUIC
- A replacement for TLS
- An Internet routing protocol
- A conventional SD-WAN product
- A universal solution to network outages
- A guarantee of zero packet loss
- A guarantee of zero downtime
- A substitute for application-level correctness

Although VRP originated in networking and VPN-related research, its current architectural focus is broader: **continuity of logical execution across changing communication infrastructure.**

---

# 4. Reference Architecture

```text
┌─────────────────────────────────────┐
│            APPLICATION              │
│      Business / Service Logic       │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│          RUNTIME INTERFACE          │
│      Logical Session Operations     │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│         PROTECTED VRP RUNTIME       │
│                                     │
│   Logical Session Management        │
│   Canonical Authority               │
│   State Transition Control          │
│   Replay Protection                 │
│   Recovery Coordination             │
│   Execution Admission               │
│   Evidence Generation               │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│        TRANSPORT ABSTRACTION        │
│                                     │
│        UDP / QUIC / TCP             │
│        Relays / Network Paths       │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│      COMMUNICATION INFRASTRUCTURE   │
│                                     │
│    Wi-Fi / Cellular / Cloud         │
│    Edge / WAN / Other Networks      │
└─────────────────────────────────────┘
```

This diagram is a conceptual architectural view, not a disclosure of the protected implementation or a complete deployment topology.

Actual integration boundaries depend on the evaluated runtime version and deployment environment.

---

# 5. Canonical Engineering Invariants

VRP engineering is organized around explicit invariants rather than assumptions about network reliability.

## 5.1 Stable Logical Session Identity

A valid transport change must not automatically create a new Logical Session.

Session identity is governed by the session lifecycle, not by the current remote endpoint.

## 5.2 Transport Is Replaceable

Transport paths may fail, recover, or be replaced.

The runtime evaluates candidate paths independently from the continued validity of the Logical Session.

## 5.3 Canonical Authority

Execution authority must be resolved through explicit canonical rules.

Concurrent or conflicting authority claims must not silently produce multiple accepted owners.

## 5.4 Monotonic Authority Progression

Accepted authority epochs must not roll backward.

Stale authority claims must not override newer canonical state.

## 5.5 Replay Protection

Previously consumed, duplicated, or stale inputs must be evaluated against the applicable replay and admission rules.

Replay protection must remain meaningful across supported transport changes.

## 5.6 Fail-Closed Admission

Untrusted observations must not mutate protected canonical state before successful admission.

Rejected inputs must not gain authority through side effects.

## 5.7 Deterministic State Transitions

Given equivalent canonical state and equivalent ordered inputs, the runtime should produce equivalent decisions under its defined deterministic contract.

This does not imply that arbitrary network scheduling or distributed timing is deterministic.

## 5.8 Observable Recovery

Recovery decisions should produce sufficient evidence to explain:

- What happened
- What was accepted
- What was rejected
- Which canonical state changed
- Which invariants were checked
- Whether continuity was preserved
- Whether recovery failed safely

## 5.9 Correctness Before Availability

Availability must not be preserved by violating canonical state, authority, or replay invariants.

When safe continuity cannot be established, the runtime must reject, suspend, or terminate the affected operation according to its defined state machine.

---

# 6. Execution Authority vs. Transport Continuity

A critical VRP design distinction is the separation between:

**Execution Authority**

The right to authorize canonical state changes or protected operations.

**Transport Continuity**

The ability to observe, select, migrate, or recover communication paths.

These are not interchangeable capabilities.

A packet arriving from a new endpoint does not automatically authorize that endpoint to become the persistent canonical path.

A recovered connection does not automatically authorize an execution retry.

A transport migration does not automatically authorize an authority epoch change.

This distinction is especially important during:

- NAT rebinding
- Path migration
- Relay replacement
- Concurrent packet arrival
- Stale authority delivery
- Duplicate requests
- Adversarial endpoint substitution

The architecture requires explicit admission and state-transition rules at these boundaries.

---

# 7. Failure and Threat Model

VRP evaluation focuses on situations where infrastructure instability and adversarial inputs can interact.

Representative scenarios include:

| Scenario | Property to evaluate |
|---|---|
| Wi-Fi to cellular transition | Logical Session continuity |
| UDP endpoint migration | Path admission correctness |
| NAT rebinding | Session identity preservation |
| Relay failure | Controlled recovery |
| Temporary blackout | Recovery without invalid state mutation |
| Duplicate delivery | Duplicate execution containment |
| Packet replay | Replay rejection |
| Stale authority | Monotonic authority enforcement |
| Authority race | Canonical ownership resolution |
| Epoch rollback attempt | Rollback rejection |
| Concurrent mutation | State consistency |
| Packet reordering | Admission and replay-window correctness |
| Invalid packet identity | Fail-closed behavior |
| Evidence tampering | Verification failure |
| Recovery contradiction | Detection and explicit rejection |

The purpose of these tests is not to assume that every attack has been defeated.

The purpose is to make the relevant failure conditions reproducible and to define what constitutes acceptable and unacceptable behavior.

A passing test establishes evidence for the tested conditions only.

---

# 8. Security Model

VRP treats communication infrastructure as potentially unreliable and its inputs as potentially untrusted.

The public security model emphasizes:

- Explicit trust boundaries
- Canonical state ownership
- Authenticated packet admission where applicable
- Replay protection
- Stale-authority rejection
- Controlled migration
- Fail-closed state transitions
- Evidence integrity
- Adversarial testing
- Separation of transport observations from execution decisions

## Security Claims Require Scope

VRP does not claim that architectural separation alone provides complete security.

A security assessment must examine the actual evaluated implementation, including:

- Cryptographic construction and key lifecycle
- Authentication and authorization
- Input validation
- Resource exhaustion behavior
- Concurrency safety
- Error handling
- Deployment configuration
- Logging and sensitive-data exposure
- Dependency security
- Operational recovery procedures

Independent review remains necessary before high-consequence production deployment.

---

# 9. Public Specification vs. Protected Implementation

VRP follows a deliberate publication boundary.

## Public Engineering Layer

The public layer is intended to describe:

- Architectural concepts
- Logical Session semantics
- State and authority invariants
- Failure models
- Threat assumptions
- Verification methodology
- Reproducible evaluation procedures
- Engineering evidence formats
- Integration considerations
- Security review requirements
- Deployment and rollback planning

## Protected Runtime Layer

The public specification does not disclose:

- Proprietary runtime source code
- Private algorithms
- Internal synchronization mechanisms
- Protected execution logic
- Unpublished optimization strategies
- Private transport-selection implementations
- Confidential operational assets
- Secret cryptographic material
- Internal implementation details

**Public documentation is not a grant of protected runtime access.**

Access to any private implementation or evaluation environment requires separate authorization and applicable written terms.

---

# 10. Verification Is Not the Same as Disclosure

Independent evaluation does not necessarily require publication of proprietary source code.

However, independent verification must have a clearly defined scope.

VRP distinguishes three evidence categories.

### A. Public Model Verification

A standalone public model tests a specified architectural invariant.

This demonstrates properties of that model.

It does not automatically establish that a separate protected implementation satisfies the same property.

### B. Black-Box Runtime Verification

An external harness exercises observable behavior through a defined interface.

This may establish evidence about the tested runtime version and scenario.

It does not expose every internal implementation path.

### C. Implementation-Level Verification

Source review, instrumentation, formal analysis, or specialized security testing may provide deeper assurance.

Such verification requires appropriate access and authorization.

These categories must not be presented as interchangeable.

**A model PASS is not automatically a protected-runtime PASS.**

**A scenario PASS is not automatically a production certification.**

---

# 11. Public Engineering Validation

The public evaluation approach prioritizes reproducibility.

A credible validation record should identify:

1. The evaluated subject
2. The exact version or immutable reference
3. The environment
4. The test scenario
5. The injected failure or adversarial input
6. The expected result
7. The observed result
8. The evidence produced
9. The verification procedure
10. The limitations of the conclusion

## Positive Controls

Positive controls demonstrate that valid behavior remains possible.

Examples:

- Valid session traffic is admitted.
- An authorized migration is accepted.
- A newer valid authority epoch is accepted.
- A recoverable interruption produces an allowed recovery transition.

## Negative Controls

Negative controls demonstrate that invalid behavior is rejected.

Examples:

- A replayed packet is rejected.
- A stale authority claim is rejected.
- An epoch rollback is rejected.
- An unauthorized endpoint cannot rewrite persistent path state.
- Duplicate execution is prevented under the tested commit contract.
- Tampered evidence fails verification.

Both positive and negative controls are necessary.

A system that rejects every input is not a successful continuity runtime.

A system that accepts every recovery attempt is not a safe continuity runtime.

---

# 12. Evidence Before Claims

VRP engineering reports should favor measurable observations over promotional conclusions.

Useful evidence may include:

- Test commands
- Runtime version identifiers
- Git commit and tree references
- Scenario definitions
- Packet and event traces
- Canonical state snapshots
- Replay rejection counts
- Authority transition records
- Recovery verdicts
- Race detector output
- Fuzzing results
- Benchmark methodology
- Evidence manifests
- Integrity verification results

Where appropriate, results should use explicit verdicts:

```text
SCENARIO=transport_migration
EXPECTED=CONTINUITY_PRESERVED
OBSERVED=CONTINUITY_PRESERVED
VERDICT=PASS
SCOPE=TESTED_SCENARIO_ONLY
```

This is an illustrative evidence format, not a claim that a specific test was executed.

Failure evidence is equally important.

A reproducible FAIL is an engineering result that should be investigated, not hidden.

---

# 13. Performance and Benchmarking

Continuity correctness and runtime performance are separate evaluation dimensions.

A correct design may still require substantial optimization before deployment.

Performance assessment should consider:

- Session admission overhead
- Migration latency
- Recovery latency
- Throughput
- CPU utilization
- Memory consumption
- Allocation behavior
- Tail latency
- Concurrency scaling
- Replay-window overhead
- Authority-transition overhead
- Behavior under packet loss and reordering
- Long-duration stability

## Benchmark Requirements

Performance claims should specify:

- Hardware
- Operating system
- Runtime version
- Network topology
- Workload
- Transport configuration
- Concurrency level
- Measurement methodology
- Baseline
- Repetition count
- Statistical treatment
- Known limitations

VRP does not claim universal performance superiority over WireGuard, OpenVPN, QUIC, or other technologies.

Such comparisons require comparable workloads, clearly defined metrics, and reproducible measurements.

---

# 14. Engineering Maturity and Readiness

VRP separates architectural maturity from implementation maturity and deployment readiness.

## Architecture

The public architecture defines the intended session, authority, transport, and recovery boundaries.

## Implementation

The protected runtime is under active engineering development and hardening.

## Validation

Public and controlled validation activities evaluate specified properties under defined conditions.

## Deployment

Production suitability requires an environment-specific engineering decision.

These stages must not be collapsed into a single marketing status.

A stable architectural direction does not imply that every runtime subsystem is complete.

A successful demonstration does not eliminate the need for integration testing.

A protected implementation does not eliminate the need for independent review.

---

# 15. Enterprise Engineering Evaluation

VRP is intended for organizations facing genuine continuity and state-correctness challenges.

Relevant domains may include:

- Telecommunications
- Cloud infrastructure
- Edge computing
- Industrial networking
- Distributed platforms
- High-availability services
- Mobile infrastructure
- Robotics
- Autonomous systems
- Critical infrastructure
- Financial and transactional systems
- AI infrastructure

These are potential evaluation domains, not claims of existing production deployment or suitability for every application.

## Recommended Evaluation Process

```text
PUBLIC DOCUMENTATION REVIEW
             |
             v
ARCHITECTURAL FIT ASSESSMENT
             |
             v
THREAT MODEL AND TRUST BOUNDARIES
             |
             v
REPRODUCIBLE PUBLIC VALIDATION
             |
             v
WORKLOAD AND FAILURE-SCENARIO DEFINITION
             |
             v
SECURITY AND INTEGRATION REVIEW
             |
             v
CONTROLLED PILOT, IF AUTHORIZED
             |
             v
EVIDENCE AND ACCEPTANCE REVIEW
             |
             v
INDEPENDENT ENGINEERING DECISION
```

Each stage should produce explicit findings.

A pilot is not a substitute for a security review.

A security review is not a substitute for operational testing.

A successful pilot is not an automatic authorization for production deployment.

---

# 16. Controlled Pilot Principles

A VRP pilot should be designed around measurable acceptance criteria.

Recommended safeguards include:

- Defined evaluation scope
- Named technical owners
- Explicit integration boundaries
- Isolated or controlled test environment
- Documented failure scenarios
- Observable pass/fail criteria
- Existing-system fallback
- Rollback procedures
- Security review
- Evidence collection
- Incident handling
- Clear termination conditions

The goal is to reduce uncertainty before expanding operational exposure.

## Pilot Acceptance

A pilot should not be considered successful merely because a demonstration completed.

Acceptance should depend on the agreed engineering criteria, including the treatment of failures, unsupported scenarios, and unresolved risks.

---

# 17. Documentation Map

The repository organizes its public engineering material by review area.

| Area | Purpose |
|---|---|
| `docs/architecture/` | Architectural models and invariants |
| `docs/runtime/` | Runtime semantics and state behavior |
| `docs/security/` | Security boundaries and threat analysis |
| `docs/evaluation/` | Validation and verification methodology |
| `docs/integration/` | Integration and deployment considerations |
| `docs/review/` | Technical and enterprise review material |
| `docs/diagrams/` | Architectural diagrams |
| `docs/rfc/` | RFC-style engineering proposals and specifications |
| `docs/adr/` | Architecture decision records |
| `examples/` | Public engineering examples |
| `engineering/` | Engineering-oriented supporting material |
| `whitepaper/` | Technical whitepapers |
| `legal/` | Legal and licensing documentation |

The presence of a documentation category does not imply that every proposed feature or implementation milestone is complete.

## Suggested Reading Order

1. This README
2. Executive and engineering overviews
3. Core architecture
4. Logical Session semantics
5. Authority and runtime state models
6. Security and threat model
7. Recovery and replay protection
8. Public validation methodology
9. Engineering evidence
10. Integration and deployment guidance
11. RFC and ADR documentation
12. Enterprise review material

---

# 18. Related Public Repositories

## Public Engineering Specification

https://github.com/Endless33/VRP-specification

The specification and associated public documentation describe the engineering model and its intended invariants.

## Veil Routing Protocol Documentation

https://github.com/Endless33/Veil-Routing-Protocol/tree/main/docs

Additional public engineering documentation and architectural material.

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

A public evaluation environment for specified continuity and security properties.

Validation-kit results must be interpreted according to the exact model, adapter, runtime subject, and scenario tested.

## Public Enterprise Timeline

https://endless33.github.io/vrp-countdown/vrp-countdown/final-public-window.html#pricing

Public information about the proposed enterprise engagement framework and commercial timeline.

Published pricing information is indicative and non-binding unless incorporated into a separately executed agreement.

---

# 19. Private Engineering Evaluation

**Program status: INVITATION ONLY**

VRP public documentation and verification material are available for independent review.

Private engineering discussions are selective and based on technical relevance.

Organizations should review the public engineering material before requesting additional engagement.

## Current Program Boundary

```text
PUBLIC_SPECIFICATION=AVAILABLE
PUBLIC_DOCUMENTATION=AVAILABLE
PUBLIC_VERIFICATION_MATERIAL=AVAILABLE

PRIVATE_EVALUATION=INVITATION_ONLY
ENGINEERING_REVIEW=CASE_BY_CASE
ENGINEERING_CAPACITY=LIMITED

PROTECTED_RUNTIME=PRIVATE
PROTOCOL_DEVELOPMENT=ACTIVE

PRODUCTION_CERTIFICATION=NOT_ESTABLISHED
```

These labels describe the public program position, not machine-generated runtime test results.

## Evaluation Eligibility

Relevant inquiries should identify:

- Organization and website
- Authorized engineering representative
- Deployment environment
- Existing continuity problem
- Proposed evaluation objectives
- Relevant failure scenarios
- Expected operational value
- Technical reasons for considering VRP

Do not submit passwords, private keys, production credentials, access tokens, confidential customer information, or regulated data.

## Initial Engineering Contact

**jumpingvpn@proton.me**

An inquiry does not guarantee a response, acceptance, scheduling, evaluation access, reserved engineering capacity, licensing, or a future commercial relationship.

Participation requires separate written authorization.

---

# 20. Responsible Security Research

Constructive technical criticism is welcome.

The most useful security reports include:

- A clearly identified affected component or public model
- An exact version or commit reference
- Reproduction steps
- Expected behavior
- Actual behavior
- Evidence of the discrepancy
- A bounded explanation of impact

Security findings should be reported responsibly through the available project contact channel.

Do not test infrastructure or private runtime systems without authorization.

Public availability of a specification does not authorize intrusive testing of unrelated systems or protected infrastructure.

---

# 21. Known Evaluation Boundaries

The following limitations are fundamental to interpreting the public material.

**Public models are not automatically the protected runtime.**

A standalone model may demonstrate a design property without proving that the protected implementation has the same behavior.

**A successful migration does not imply zero packet loss.**

Logical continuity, packet delivery, application retry semantics, and transaction correctness are distinct properties.

**Replay rejection does not establish complete security.**

It addresses one class of threats within the tested assumptions.

**Deterministic decisions do not imply deterministic networks.**

Distributed timing, scheduling, packet delivery, and failure occurrence may remain nondeterministic.

**Transport independence does not eliminate transport limitations.**

Recovery remains subject to connectivity, available resources, authentication, state validity, and defined runtime behavior.

**Protected source code does not itself provide assurance.**

Independent evidence, security analysis, and operational testing remain necessary.

**No universal production-readiness claim is made.**

Suitability must be established for the intended deployment.

---

# 22. Licensing and Intellectual Property

This repository publishes architectural documentation, engineering methodology, and evaluation guidance.

Rights to use, copy, modify, or redistribute public repository content are governed by any applicable repository license and other valid terms.

Publication does not, by itself, grant access to or rights in:

- The Protected VRP Runtime
- Private source code
- Proprietary implementation algorithms
- Confidential engineering assets
- Unpublished implementation details
- Commercial runtime licensing
- Production deployment of protected components
- Intellectual property ownership
- Exclusivity or sublicensing rights

Any protected runtime access, commercial deployment rights, or additional licensing arrangements require separate written authorization and applicable agreements.

Where a repository license is present, consult its actual text rather than treating this README as a replacement for that license.

---

# 23. Engineering Principles

The project is guided by a small number of persistent engineering commitments.

### Session ≠ Transport

Logical identity must not be reduced to a network connection.

### Correctness Before Availability

An unsafe recovery is not a successful recovery.

### Authority Must Be Explicit

Transport reachability is not execution authorization.

### Replays Must Not Become New Operations

Duplicate delivery must not silently create duplicate execution.

### State Changes Must Be Explainable

Accepted and rejected transitions require an auditable rationale.

### Evidence Before Claims

Assertions are starting points for experiments, not substitutes for them.

### Failure Is Part of the Design

A serious runtime must define how it behaves when infrastructure becomes unreliable.

### Independent Review Matters

Engineering confidence should increase through reproducible scrutiny.

### Protected Implementation, Reviewable Behavior

Private implementation details and public technical accountability can coexist, provided the evaluation boundary is clearly stated.

---

# 24. Final Engineering Statement

VRP is not asking engineers to accept a promise of perfect connectivity.

It is proposing a different boundary between communication infrastructure and logical execution.

The central question is:

> **When the network changes, what must remain true for the application session to continue safely?**

That question leads to concrete engineering requirements:

Stable identity.

Canonical authority.

Monotonic state.

Replay protection.

Controlled admission.

Deterministic decisions.

Observable recovery.

Reproducible evidence.

These requirements must be tested, challenged, and refined.

The public specification exists to make that process possible.

The protected runtime exists to implement the architecture.

Neither documentation nor implementation should be considered beyond scrutiny.

---

## The Engineering Standard

```text
READ THE SPECIFICATION.

IDENTIFY THE INVARIANTS.

CHALLENGE THE ASSUMPTIONS.

REPRODUCE THE TESTS.

INJECT THE FAILURES.

INSPECT THE EVIDENCE.

VERIFY THE BOUNDARIES.

MEASURE THE PERFORMANCE.

DOCUMENT THE LIMITATIONS.

MAKE AN INDEPENDENT DECISION.
```

---

# VEIL ROUTING PROTOCOL

**SESSION ≠ TRANSPORT**

**CONTINUITY FIRST**

**CORRECTNESS BEFORE AVAILABILITY**

**EVIDENCE BEFORE CLAIMS**

Public architecture. Protected implementation. Independent engineering scrutiny.

**Build. Break. Measure. Verify. Repeat.**
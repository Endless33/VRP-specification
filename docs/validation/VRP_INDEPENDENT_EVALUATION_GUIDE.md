# VRP Independent Evaluation Guide

**Document class:** Public evaluation procedure
**Evaluation model:** Black-box
**Protected implementation access:** Not required

## 1. Objective

This guide defines how an independent engineering team can evaluate
externally observable VRP behavior without access to proprietary
implementation details.

The evaluation concerns declared properties, execution conditions,
observable outcomes and retained evidence.

## 2. Evaluation principles

An evaluation must distinguish:

- configured conditions from actually executed conditions;
- transport availability from application delivery;
- packet reception from logical session identity;
- recovery time from recovery correctness;
- test execution from successful qualification;
- integrity verification from semantic verification.

No single measurement substitutes for the others.

## 3. Evaluation dimensions

### 3.1 Transport migration

Record endpoint transitions, timestamps, transport availability
and observed application behavior.

### 3.2 Packet delivery

Record attempted transmissions, injected loss, successful sends,
received messages, application acknowledgements and unresolved events.

### 3.3 Logical session identity

Use an independent observation mechanism to compare the logical
session identifier before, during and after transport transitions.

Process identifiers, socket identifiers and successful handshakes
are not substitutes for logical session identity.

### 3.4 Recovery

Record the beginning and end of each interruption, recovery duration,
terminal application state and identity observations.

### 3.5 Security behavior

Record accepted and rejected test operations, including the
expected and observed outcome.

A rejection count alone does not establish correctness unless
the relevant postconditions are independently checked.

## 4. Evidence requirements

Each evaluation package should contain:

- execution metadata;
- scenario identifier;
- runtime build identity;
- environment description;
- measurement records;
- machine-readable verification results;
- integrity manifest;
- evaluation report.

Private implementation details and operational secrets must not
be included.

## 5. Result classification

| Status | Meaning |
|---|---|
| PASS | Required observation and postcondition verified |
| FAIL | Required property violated |
| NOT_PROVEN | Evidence insufficient to establish the property |
| NOT_RUN | Scenario was not executed |
| INVALID | Evidence integrity or execution attribution failed |

A missing result must never be interpreted as PASS.

## 6. Independent verification

The evaluator should:

1. Verify the published artifact identity.
2. Verify evidence integrity.
3. Confirm the execution conditions.
4. Recalculate derived measurements.
5. Compare observations with declared acceptance criteria.
6. Preserve failures and unresolved findings.
7. Publish a qualification summary with explicit limitations.

## 7. Intellectual property boundary

The evaluation does not require publication of:

- protected source code;
- internal algorithms;
- proprietary state-transition mechanisms;
- private interfaces;
- implementation-specific authority mechanisms;
- credentials or private operational configuration.

The evaluation target is observable behavior and its declared contract.

## 8. Reporting requirements

The final report should identify:

- tested properties;
- execution conditions;
- evidence references;
- verified outcomes;
- failures;
- unresolved claims;
- limitations;
- independent verifier results.

Claims must not exceed the scope of the retained evidence.

## 9. Conclusion

Independent evaluation of VRP should be based on reproducible
external observations and verifiable evidence.

The implementation may remain proprietary while its declared
behavior is subjected to meaningful technical scrutiny.

**No hype. Read the specifications. Examine the evidence.
Challenge the engineering.**

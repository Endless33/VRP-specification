# VRP Black-Box Runtime Observations

**Document class:** Public engineering evidence
**Assessment:** Limited observed results
**Implementation disclosure:** None

## 1. Purpose

This report records aggregate observations from a network
fault-suppression experiment.
It describes externally observable measurements only.

## 2. Recorded measurements

| Scenario | Send attempts | Injected | Successful sends | Suppression | Receive successes |
|---|---:|---:|---:|---:|---:|
| BASELINE | 8 | 0 | 8 | 0.0% | 16 |
| PARTIAL | 8 | 3 | 5 | 37.5% | 13 |
| FULL | 8 | 8 | 0 | 100.0% | 8 |

## 3. Interpretation

The recorded scenarios demonstrate different observed send
suppression levels under the tested conditions.
Receive operations were also observed in the full-suppression case.

Receive observations do not independently establish application
delivery or causal attribution to a particular logical session.

## 4. Evidence limitations

The original trace attribution was incomplete.
The available measurements do not establish:

- application-level message delivery;
- canonical logical session identity preservation;
- authority integrity;
- uninterrupted session continuity;
- recovery correctness under compound faults.

These properties are not claimed by this report.

## 5. Independent evaluation

Send suppression, receive success, application delivery and
logical session identity are separate evaluation dimensions.

Raw private traces and implementation details are not included.

## 6. Intellectual property boundary

This report excludes protected source code, internal algorithms,
private interfaces, credentials and proprietary implementation
mechanisms.

## 7. Conclusion

The experiment provides recorded observations of send suppression
and receive activity. It does not establish the broader continuity
properties listed above.

**No hype. Read the specifications. Examine the evidence.**

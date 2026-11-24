# VRP Session Continuity — Evidence Boundary

## Purpose

Veil Routing Protocol (VRP) treats logical session continuity and transport
continuity as different properties.

**SESSION != TRANSPORT**

A surviving socket, process, endpoint, connection, or recovered packet path is
not, by itself, proof that the same logical session survived.

This document defines the public evidence boundary used when evaluating that
claim.

## What black-box qualification has demonstrated

Controlled runtime observations have demonstrated transport transitions in
which:

- the network endpoint changed;
- application traffic was accepted after the transition;
- transport operation recovered or continued across the transition;
- replayed traffic was rejected in qualified scenarios;
- the runtime exposed a stable session label across observed transition events.

These observations are evidence of transport resilience and application
delivery across a transport transition.

They are not automatically proof of canonical logical-session continuity.

## What is deliberately not claimed

VRP does not treat any of the following as sufficient identity evidence:

- unchanged PID;
- unchanged process lifetime;
- socket survival;
- socket recreation;
- endpoint recovery;
- successful packet delivery;
- reconnect success;
- a session identifier carried only by the traffic being tested;
- a runtime-generated continuity label without independent observation.

Accordingly, transport recovery must not be presented as proof of logical
session identity.

## Independent identity criterion

A stronger continuity qualification requires an observation boundary that is
independent of the transport property under test.

The evaluator must be able to establish logical identity before, during, and
after transport disruption without deriving the conclusion merely from the
same endpoint, socket, packet stream, or transport recovery event.

The observer must also preserve the distinction between:

1. transport recovery;
2. application delivery;
3. logical session identity;
4. authority integrity.

A PASS in one category does not imply a PASS in another.

## Current qualification boundary

The current public evidence supports black-box claims concerning transport
transition behavior, post-transition delivery, and qualified replay rejection.

Independent canonical logical-session identity across the full adversarial
transport series remains a separate qualification requirement.

Until that requirement is independently satisfied, VRP documentation will
report logical-session continuity as **NOT PROVEN by that evidence set** rather
than infer it from transport behavior.

This is intentional.

A protocol claiming transport-independent sessions should be evaluated by a
standard capable of falsifying that claim.

## Release principle

VRP qualification follows a simple rule:

> Evidence may strengthen a claim. Labels may not.

Future public resilience bundles should therefore separate measurements and
verdicts for transport behavior, delivery, session identity, authority
integrity, replay containment, and recovery.

No Protected Core implementation detail is required to evaluate these
externally observable properties.

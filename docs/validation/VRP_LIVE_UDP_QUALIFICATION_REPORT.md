# VRP Live UDP Qualification Report

Date: 2026-11-24

**Principle: SESSION != TRANSPORT**

## Scope

Controlled local UDP experiments using an isolated network
environment and relay-induced source-port changes.

This report describes observed transport behavior.
It does not establish canonical logical session continuity.

## Measurements

| Metric | Observed result |
|---|---:|
| Fault phase checks | 9/9 |
| Post-fault exchange recoveries | 4/4 |
| Standalone endpoint rotations | 30 |
| Bidirectional post-rotation epochs | 30/30 |
| Standalone unique source ports | 31 |
| Target rotation interval | 2 seconds |
| Maximum standalone cadence deviation | 0.435 ms |
| Compound endpoint rotations | 59 |
| Compound unique source ports | 60 |
| Compound configured loss | 80% |
| Compound measured loss | 76.92% (10/13) |
| Complete relay blackout | 30 seconds |
| Compound recovery epochs | 14/15 |

## Qualification verdict

- Transport recovery: OBSERVED.
- Strict compound qualification: NOT QUALIFIED.
- Live canonical session identity: NOT PROVEN.
- Application delivery: NOT PROVEN.
- Authority integrity: NOT PROVEN.
- Compound replay validation: NOT TESTED.
- Full release qualification: OPEN.

The first recovery interval contained no observed exchange.
Bidirectional exchange resumed during the subsequent
2-4 second window. Exact recovery latency was not measured.

The original strict recovery criterion remains unchanged.

## Limitations

Source-port rotation is not physical IP migration.
Configured loss probability is not measured loss.
The compound loss sample contains only 13 datagrams.
Relay forwarding does not establish application delivery,
canonical session identity or authority integrity.

These are operator-generated laboratory measurements,
not independent external certification.

## Evidence provenance

Private source evidence manifest SHA-256:

`e4bc04b70bcb38a097eb8d06d19f6824a0868d2622e28a0aef8fb5302702afa6`

Public measurement summary SHA-256:

`39fd90c330862052a919e927cba554f16161f34d13454dd50428172d4ecdf7b1`

Public measurement summary:
[VRP_LIVE_UDP_QUALIFICATION_EVIDENCE.json](VRP_LIVE_UDP_QUALIFICATION_EVIDENCE.json)

The private manifest commitment identifies the recorded
evidence bundle but cannot independently establish its
contents or protocol behavior without access to that bundle.

The public JSON digest verifies the published summary only.

Protected implementation details are intentionally excluded.

**Evidence over assumptions.**

# VRP Design Principles

Status: Release Candidate (v1.0.0-rc1)

## Philosophy

VRP is designed around continuity rather than connection persistence.

The protocol assumes that networks are unreliable and that transports
may appear, disappear or change during the lifetime of a session.

The objective is not to preserve a transport.

The objective is to preserve a canonical session.

## Principles

### Session Identity

A session represents application identity.

It exists independently of the transport currently carrying traffic.

### Transport Independence

Any transport capable of carrying VRP messages may participate.

Changing transport does not imply creating a new session.

### Canonical State

Only one canonical session state exists.

Concurrent observations or delayed packets must never create competing
authoritative histories.

### Explicit Authority

Every state transition requires explicit authority.

Unauthorized transitions are rejected.

### Deterministic Behaviour

Given the same canonical inputs, every compliant implementation must
produce identical protocol decisions.

### Replay Resistance

Previously accepted messages must not become valid again.

Replay protection is part of canonical state management.

### Fail Closed

Whenever protocol state cannot be determined safely,
the implementation rejects the transition instead of guessing.

## Engineering Goals

- deterministic behaviour
- transport independence
- verifiable state transitions
- implementation portability
- protocol reproducibility


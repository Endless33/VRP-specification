# 08. Security Model

VRP security is based on ambiguity, motion, and non-persistence.

## Principles

1. **Zero-Knowledge Architecture**  
   No logs, no history, no reconstructable state.

2. **Blind-Node Routing**  
   No node sees the full path.

3. **Entropy-Based Mutation**  
   Routes shift before they can be analyzed.

4. **Non-Persistent Sessions**  
   Nothing survives beyond the session boundary.

## Threat Model

VRP protects against:

- traffic correlation  
- route reconstruction  
- behavioral fingerprinting  
- long-term metadata accumulation  

VRP does not rely on trust.  
It relies on motion.
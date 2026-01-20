# 28. Node Coordination (Abstract)

VRP nodes coordinate without sharing identity or state.

## Coordination Principles

- no persistent communication  
- no shared identifiers  
- no route reconstruction  

## Coordination Signals

Nodes exchange:

- entropy hints  
- phase compatibility markers  
- mutation readiness signals  

## Guarantees

Coordination is:

- ephemeral  
- partial  
- non-reconstructable  
- non-linkable
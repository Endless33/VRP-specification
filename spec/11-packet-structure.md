# 11. Packet Structure (Abstract)

VRP packets do not follow traditional VPN tunneling formats.  
The structure is intentionally abstracted to prevent reconstruction and correlation.

## Packet Layers

1. **Behavior Layer**  
   Encodes movement influence, entropy weight, and phase context.

2. **Mutation Layer**  
   Determines how the packet interacts with routing mutation windows.

3. **Blind Node Layer**  
   Contains partial path fragments, never the full route.

4. **Payload Layer**  
   Encrypted data segment (implementation-specific).

## Design Principles

- No persistent identifiers  
- No stable headers  
- No reconstructable metadata  
- No predictable routing markers  

VRP packets are designed to be unreadable outside their moment of transit.
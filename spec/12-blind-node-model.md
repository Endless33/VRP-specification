# 12. Blind Node Model

The Blind Node Model ensures that no single node can reconstruct the full routing path.

## Node Roles

- **Ingress Node**  
  Receives the packet but does not know the exit.

- **Transit Nodes**  
  Hold partial path fragments and entropy modifiers.

- **Veil Exit Node**  
  Sees only the final hop, not the origin or full route.

## Guarantees

- No node sees the entire path  
- No node can correlate packets  
- No node can reconstruct session behavior  

## Node Behavior

Nodes operate on:

- partial entropy  
- partial routing context  
- partial phase state  

The Blind Node Model is the backbone of VRP anonymity.
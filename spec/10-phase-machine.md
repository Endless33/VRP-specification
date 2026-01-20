# 10. Phase Machine

The VRP Phase Machine defines how the protocol transitions between behavioral states.

## Phase Types

- **Dormant** — minimal entropy, passive state  
- **Active** — movement-driven, entropy accumulation  
- **Mutation Window** — routing and state mutation  
- **Stabilization** — entropy decay, session closure  

## Transition Rules

Transitions depend on:

- entropy weight  
- movement intensity  
- event density  
- temporal variance  

## Phase Guarantees

- No phase is permanent  
- No transition is reversible  
- No state is fully observable  

The Phase Machine is the heartbeat of VRP.
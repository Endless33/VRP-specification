# 06. Sessions

A VRP session represents a temporary behavioral container that holds state, entropy, and routing context.

## Session Lifecycle

1. **Initialization**  
   A session is created with minimal state and zero accumulated entropy.

2. **Active Phase**  
   Movement, events, and entropy continuously modify the session state.

3. **Mutation Windows**  
   Specific intervals where routing and phase transitions may occur.

4. **Termination**  
   A session ends when its entropy weight stabilizes or the client closes it.

## Session Properties

- Session ID (ephemeral, non-persistent)
- Phase state
- Entropy weight
- Routing context
- Movement influence

Sessions are intentionally non-reconstructable.
# 19. Veil Exit Model

The Veil Exit is the final node in the VRP routing chain.  
It sees only the last hop, never the origin or full route.

## Exit Properties

- Stateless  
- Blind to session identity  
- Blind to routing history  
- Blind to entropy drift  

## Exit Responsibilities

- Deliver the final packet  
- Apply final mutation filters  
- Maintain non-correlation guarantees  

## Exit Limitations

The exit cannot:

- identify the client  
- reconstruct the route  
- correlate multiple sessions  

The Veil Exit is the final veil of the protocol.
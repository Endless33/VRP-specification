# 25. Veil Exit Security

The Veil Exit is the final boundary between VRP and the external network.

## Security Principles

- No origin visibility  
- No session visibility  
- No routing visibility  
- No drift visibility  

## Exit Behavior

- applies final mutation filters  
- strips residual entropy markers  
- prevents correlation with ingress nodes  

## Guarantees

The exit cannot:

- identify the client  
- reconstruct the route  
- correlate multiple packets  

The Veil Exit is the last veil of the protocol.
# 22. Mutation Windows Specification

Mutation Windows are temporal intervals where routing and phase changes may occur.

## Window Types

- **Soft Window** — low mutation probability  
- **Hard Window** — high mutation probability  
- **Entropy Window** — triggered by entropy thresholds  
- **Drift Window** — triggered by behavioral drift  

## Window Properties

- unpredictable  
- non-periodic  
- entropy-driven  
- movement-influenced  

## Window Guarantees

- cannot be predicted  
- cannot be replayed  
- cannot be reconstructed  

Mutation Windows ensure continuous motion.
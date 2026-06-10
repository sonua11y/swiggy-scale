# Swiggy Scale Simulation

World Cup Final traffic simulation for a Swiggy-like system.

## Documents

| File | Purpose |
|--------|--------|
| FAILURE-CASCADE.md | Failure analysis |
| ARCHITECTURE.md | Architecture redesign |
| COST-ESTIMATE.md | AWS costs |
| RUNBOOK.md | Incident guide |

## Key Findings

- 500K RPS demand
- 12K RPS capacity
- DB fails first
- Redis reduces DB load
- CDN prevents image overload
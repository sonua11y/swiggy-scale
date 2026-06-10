# Swiggy Scale Simulation

A scalability analysis of a Swiggy-like food delivery platform during a World Cup Final traffic spike.

## Scenario

* 180M users notified
* 10M active users
* 500K RPS demand
* Single Node.js server
* Single PostgreSQL database

## Documents

| File               | Purpose                 |
| ------------------ | ----------------------- |
| FAILURE-CASCADE.md | Failure analysis        |
| ARCHITECTURE.md    | Architecture redesign   |
| COST-ESTIMATE.md   | AWS cost calculations   |
| RUNBOOK.md         | Incident response guide |

## Key Findings

* Demand reaches 500K RPS
* PostgreSQL fails first
* Node.js crashes after DB exhaustion
* Redis reduces database load
* CDN prevents image overload

## Technologies

* Node.js
* PostgreSQL
* Redis
* AWS
* CloudFront
* SQS
* PgBouncer

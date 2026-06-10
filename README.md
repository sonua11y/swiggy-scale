# README.md

# Swiggy is Down - Scale Simulation

## Project Overview

This project analyzes how a Swiggy-like food delivery platform would behave during a massive traffic spike caused by a 50% discount campaign during the India vs Pakistan World Cup Final.

The current system consists of:

* Single Node.js server
* Single PostgreSQL database
* No cache
* No CDN
* No load balancer

The objective is to identify failure points, redesign the architecture, estimate AWS costs, and prepare an incident response runbook.

---

## Scenario

* Users notified: 180 million
* Expected CTR: 8%
* Users opening app: 14.4 million
* Active users considered: 10 million
* API calls per user: 3

Peak RPS:

(10,000,000 × 3) / 60

= 500,000 RPS

---

## Documents

| Document           | Purpose                                    |
| ------------------ | ------------------------------------------ |
| FAILURE-CASCADE.md | Failure analysis and capacity calculations |
| ARCHITECTURE.md    | Redesigned scalable architecture           |
| COST-ESTIMATE.md   | AWS infrastructure cost estimate           |
| RUNBOOK.md         | Incident response guide                    |

---

## Key Findings

* Demand reaches approximately 500,000 RPS.
* PostgreSQL connection pool fails first at ~394 RPS.
* Node.js saturates around 12,000 RPS.
* Redis eliminates most database reads.
* CloudFront prevents image traffic from overwhelming origin servers.

---

## Technologies

* Node.js
* PostgreSQL
* Redis
* AWS EC2
* AWS RDS
* CloudFront
* SQS
* PgBouncer

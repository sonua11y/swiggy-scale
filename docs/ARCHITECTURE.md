# ARCHITECTURE

## Current Architecture

Users
→ Node.js
→ PostgreSQL

Problems:

* Single point of failure
* No cache
* No load balancing
* No CDN

---

## Proposed Architecture

Users
→ CloudFront CDN
→ Application Load Balancer
→ Node.js Instances
→ Redis Cache
→ PgBouncer
→ PostgreSQL Primary

PostgreSQL Primary
→ Read Replica 1
→ Read Replica 2

SQS
→ Payment Workers

---

## Component Justification

| Component       | Failure It Prevents  | How                                       |
| --------------- | -------------------- | ----------------------------------------- |
| CloudFront CDN  | Failure 5            | Serves images from edge                   |
| ALB             | Single point failure | Distributes traffic                       |
| Redis Cache     | Failure 1            | Reduces DB reads                          |
| Redis SETNX     | Failure 4            | Atomic promo updates                      |
| PgBouncer       | Failure 1            | Reuses DB connections                     |
| Read Replicas   | Read bottleneck      | Separates reads and writes                |
| SQS Queue       | Failure 3            | Async payment processing                  |
| Payment Workers | Failure 3            | Removes payment latency from request path |

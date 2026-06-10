# ARCHITECTURE.md

# Architecture Redesign

## Current Architecture

Users
→ Node.js
→ PostgreSQL

Weaknesses:

* Single point of failure
* No cache
* No load balancing
* No CDN

---

## Proposed Architecture

Users
→ CloudFront CDN
→ Application Load Balancer
→ Node.js Auto Scaling Group
→ Redis Cache
→ PgBouncer
→ PostgreSQL Primary

PostgreSQL Primary
→ Read Replica 1
→ Read Replica 2

SQS Queue
→ Payment Workers

---

## Component Justification Table

| Component          | Failure Prevented                | How                                           |
| ------------------ | -------------------------------- | --------------------------------------------- |
| CloudFront CDN     | Failure 5: NIC Saturation        | Static assets served from edge locations      |
| ALB                | Single Point of Failure          | Traffic distributed across multiple instances |
| Auto Scaling Group | Failure 2: Node.js Saturation    | Adds instances automatically                  |
| Redis Cache        | Failure 1: PostgreSQL Exhaustion | Reduces DB reads by 80%                       |
| Redis SETNX        | Failure 4: Promo Race Condition  | Atomic promo updates                          |
| PgBouncer          | Failure 1: PostgreSQL Exhaustion | Reuses database connections                   |
| Read Replicas      | Read Bottleneck                  | Separates reads and writes                    |
| SQS Queue          | Failure 3: Payment Amplification | Makes payments asynchronous                   |
| Payment Workers    | Failure 3: Payment Amplification | Processes payments outside request path       |

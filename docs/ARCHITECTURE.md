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

## Component Justification Table

| Component                 | Failure Prevented                     | How It Prevents It                                       |
| ------------------------- | ------------------------------------- | -------------------------------------------------------- |
| CloudFront CDN            | Failure 5: NIC Saturation             | Images are served from edge locations instead of Node.js |
| Application Load Balancer | Single Point of Failure               | Routes traffic across multiple instances                 |
| Redis Cache               | Failure 1: PostgreSQL Pool Exhaustion | Reduces database reads by caching menus                  |
| Redis SETNX               | Failure 4: Promo Race Condition       | Makes promo updates atomic                               |
| PgBouncer                 | Failure 1: PostgreSQL Pool Exhaustion | Reuses database connections efficiently                  |
| Read Replicas             | Read Bottleneck                       | Separates read traffic from write traffic                |
| SQS Queue                 | Failure 3: Payment Call Amplification | Removes payment processing from request path             |
| Payment Workers           | Failure 3: Payment Call Amplification | Processes payments asynchronously                        |
| Auto Scaling Group        | Failure 2: Node.js Saturation         | Adds more Node.js instances when CPU increases           |

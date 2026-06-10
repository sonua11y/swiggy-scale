# Architecture

## Current

Users
→ Node.js
→ PostgreSQL

## Proposed

Users
→ CloudFront
→ ALB
→ Node.js Instances
→ Redis
→ PgBouncer
→ PostgreSQL Primary
→ Read Replicas

SQS
→ Payment Workers

## Justification

| Component | Prevents |
|------------|------------|
| CloudFront | NIC saturation |
| ALB | Single point failure |
| Redis | DB overload |
| PgBouncer | Connection exhaustion |
| Read Replicas | Read bottleneck |
| SQS | Payment delays |
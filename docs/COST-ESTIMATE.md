# COST-ESTIMATE.md

# AWS Cost Estimate

## Baseline Architecture

### EC2

t3.medium × 4

$0.0416 × 720 × 4

= $119.81/month

---

### PostgreSQL Primary

db.r6g.large

$0.182 × 720

= $131.04/month

---

### Read Replicas

db.r6g.large × 2

$0.182 × 720 × 2

= $262.08/month

---

### Redis Cluster

cache.r6g.large × 3

$0.166 × 720 × 3

= $358.56/month

---

### Application Load Balancer

≈ $56.20/month

---

### CloudFront

≈ $85/month

---

### SQS

≈ $12/month

---

## Baseline Total

≈ $1024/month

---

## Peak Event Cost

### Additional EC2 Capacity

t3.2xlarge × 20 for 4 hours

≈ $26.62

### Temporary RDS Upgrade

db.r6g.4xlarge for 4 hours

≈ $4.11

### CloudFront Surge Traffic

50TB transfer

≈ $425

---

## Peak Event Extra Cost

≈ $455

---

## Business Justification

Revenue loss rate:

₹4.2 crore/minute

45-minute outage:

₹4.2 × 45

= ₹189 crore

Infrastructure cost:

≈ $1024/month

Investing in scalable infrastructure is significantly cheaper than downtime.

# AWS Cost Estimate

## Baseline

EC2: $120
RDS: $131
Read Replicas: $262
Redis: $359
ALB: $56
CloudFront: $85
SQS: $12

Total ≈ $1024/month

## Peak Event

Extra EC2: $26.62
DB Upgrade: $4.11
CloudFront Surge: $425

Extra Cost ≈ $455

## Business Case

45 min outage

₹4.2 crore/min × 45

= ₹189 crore loss

Infrastructure cost is much cheaper than downtime.
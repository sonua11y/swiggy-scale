# AWS Cost Estimate

## Baseline

| Service           | Monthly Cost |
| ----------------- | ------------ |
| EC2 t3.medium × 4 | $119.81      |
| RDS db.r6g.large  | $131.04      |
| Read Replicas × 2 | $262.08      |
| Redis Cluster × 3 | $358.56      |
| ALB               | $56.20       |
| CloudFront        | $85          |
| SQS               | $12          |

Total Cost = $1024/month

---

## Peak Event Cost

| Service          | Additional Cost |
| ---------------- | --------------- |
| EC2 Auto Scaling | $26.62          |
| RDS Upgrade      | $4.11           |
| CloudFront Surge | $425            |

Peak Event Extra Cost = $455

---

## Business Justification

Revenue Loss:

₹4.2 crore/minute

45 minutes outage:

₹4.2 × 45

= ₹189 crore

Infrastructure Cost:

≈ $1024/month

Conclusion:

Paying $1024/month is significantly cheaper than losing ₹189 crore during an outage.

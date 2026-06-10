# RUNBOOK.md

# Incident Runbook

## STEP 1 - DETECT

Alert Thresholds

* ALB 5xx > 5% (Critical)
* PostgreSQL Connections > 80% (Warning)
* CPU > 80% (Warning)
* Redis Memory > 75% (Warning)
* SQS Queue Depth > 10,000 (Warning)
* P99 Latency > 2 seconds (Critical)

---

## STEP 2 - TRIAGE

1. Check PostgreSQL Connections.

If red:
Go to Step 3A.

2. Check CPU Utilization.

If red:
Go to Step 3B.

3. Check Redis Cache Miss Rate.

If red:
Go to Step 3C.

4. Check SQS Queue Depth.

If red:
Go to Step 3D.

---

## STEP 3A - DB Pool Exhaustion

Command:

aws rds describe-db-instances

Action:

* Scale RDS if required
* Restart PgBouncer

Success:

Connections below 80%.

---

## STEP 3B - Compute Saturation

Command:

aws autoscaling set-desired-capacity 
--auto-scaling-group-name swift-api 
--desired-capacity 10

Success:

CPU below 70%.

---

## STEP 3C - Redis Cache Issue

Action:

* Warm cache
* Verify Redis cluster health

Success:

Cache hit ratio improves.

---

## STEP 3D - Payment Queue Backup

Action:

* Restart payment workers
* Drain SQS backlog

Success:

Queue depth decreasing.

---

## STEP 4 - ROLLBACK

Rollback Command

aws ecs update-service 
--cluster swift-prod 
--service api 
--task-definition PREVIOUS_STABLE_VERSION

Warning:

Never rollback database schema.

Rollback application code only.

---

## STEP 5 - POSTMORTEM

Template

Timeline

Root Cause

Impact

What Worked

Action Items

Owner

Due Date

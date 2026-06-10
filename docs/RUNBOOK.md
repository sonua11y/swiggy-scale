# Incident Runbook

## Step 1 - Detect

- ALB 5xx > 5%
- DB Connections > 80%
- CPU > 80%
- Redis Memory > 75%
- SQS Depth > 10000
- P99 Latency > 2s

## Step 2 - Triage

Check DB
→ If red = DB issue

Check CPU
→ If red = Compute issue

Check Redis
→ If red = Cache issue

Check SQS
→ If red = Queue issue

## Step 3 - Respond

DB issue → Scale DB/PgBouncer

CPU issue → Add instances

Queue issue → Restart workers

Redis issue → Warm cache

## Step 4 - Rollback

aws ecs update-service \
--cluster swiggy-prod \
--service api \
--task-definition PREVIOUS_VERSION

## Step 5 - Postmortem

Timeline
Root Cause
Impact
What Worked
Action Items
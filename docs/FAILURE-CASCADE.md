# FAILURE CASCADE ANALYSIS

## Traffic Simulation

Total notification recipients: 180,000,000

CTR: 8%

Users opening app: 14,400,000

Assume 10,000,000 active users in first minute.

Each user makes 3 API calls:

* GET /restaurants
* GET /restaurant/:id
* POST /orders

Peak RPS:

(10,000,000 × 3) / 60

= 500,000 RPS

---

## Capacity Limits

| Component    | Capacity        |
| ------------ | --------------- |
| PostgreSQL   | 100 connections |
| Node.js      | ~12,000 RPS     |
| Payment Call | 200–2000 ms     |
| Memory       | 4 GB            |

---

## PostgreSQL Pool Exhaustion Formula

Connections Held =
(% Non-Payment RPS × Query Time) +
(% Payment RPS × Payment Hold Time)

Assumptions:

* Total RPS = 500
* 70% normal requests
* 30% payment requests
* Query time = 20ms = 0.02s
* Payment hold = 800ms = 0.8s

Connections Held =
(350 × 0.02) + (150 × 0.8)

= 7 + 120

= 127

Pool Limit = 100

Result:
Pool exhaustion occurs around 394 RPS.

---

## Failure 1: PostgreSQL Pool Exhaustion

Severity: CRITICAL

Trigger: ~394 RPS

Impact:
New DB connections rejected.

Next Failure:
Node.js backlog.

---

## Failure 2: Node.js Event Loop Saturation

Severity: CRITICAL

Trigger: ~12,000 RPS

Impact:
Response time increases dramatically.

Next Failure:
OOM crash.

---

## Failure 3: Synchronous Payment Amplification

Severity: HIGH

Trigger: ~300–500 RPS

Impact:
Connections held too long.

Next Failure:
DB pool exhaustion.

---

## Failure 4: Promo Code Race Condition

Severity: HIGH

Trigger:
Millions of concurrent promo requests.

Impact:
Promo oversold.

---

## Failure 5: NIC Saturation

Severity: CRITICAL

Trigger:
10M users loading images.

Impact:
Network completely saturated.

---

## Incident Timeline

T+0s Notification sent

T+3s PostgreSQL pool exhausted

T+5s Node.js queue buildup

T+8s DB rejects connections

T+10s Payment timeouts

T+12s Promo oversold

T+15s NIC saturated

T+18s Node.js OOM crash

T+45m Root cause identified

T+2h Recovery complete

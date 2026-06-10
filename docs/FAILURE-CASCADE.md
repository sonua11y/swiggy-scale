# FAILURE-CASCADE.md

# Failure Cascade Analysis

## Traffic Simulation

Total users notified = 180,000,000

CTR = 8%

Users opening app = 14,400,000

Assume 10,000,000 active users during the first minute.

Each user performs:

* GET /restaurants
* GET /restaurant/:id
* POST /orders

Peak RPS:

(10,000,000 × 3) / 60

= 500,000 RPS

---

## Component Capacity Limits

| Component         | Capacity        |
| ----------------- | --------------- |
| PostgreSQL        | 100 connections |
| Node.js           | ~12,000 RPS     |
| Payment Hold Time | 200–2000 ms     |
| Memory            | 4 GB            |

---

## PostgreSQL Pool Exhaustion Formula

Connections Held =
(% Non-Payment RPS × Query Time) +
(% Payment RPS × Payment Hold Time)

Assumptions:

* Total RPS = 500
* 70% normal requests
* 30% payment requests
* Query Time = 20ms = 0.02s
* Payment Hold Time = 800ms = 0.8s

Connections Held

= (350 × 0.02)

* (150 × 0.8)

= 7 + 120

= 127 Connections

Pool Limit = 100

Result:

PostgreSQL pool exhaustion occurs around 394 RPS.

---

## Failure 1 - PostgreSQL Connection Pool Exhaustion

Severity: CRITICAL

Trigger:
~394 RPS

User Impact:
500 errors and failed requests.

Next Failure:
Node.js request backlog.

---

## Failure 2 - Node.js Event Loop Saturation

Severity: CRITICAL

Trigger:
~12,000 RPS

User Impact:
Latency increases from milliseconds to seconds.

Next Failure:
Out-of-memory crash.

---

## Failure 3 - Synchronous Payment Call Amplification

Severity: HIGH

Trigger:
~300–500 RPS

User Impact:
Payment timeouts.

Reason:
Database connections remain occupied during payment processing.

Next Failure:
Connection pool exhaustion.

---

## Failure 4 - Promo Code Race Condition

Severity: HIGH

Trigger:
Millions of simultaneous promo requests.

User Impact:
Promo budget oversold.

Next Failure:
Financial loss and customer complaints.

---

## Failure 5 - NIC Saturation (No CDN)

Severity: CRITICAL

Trigger:

10M users × 20 images × 200KB

≈ 40TB transfer in first minute

User Impact:
Application becomes unreachable.

Next Failure:
Complete API outage.

---

## Incident Timeline

T+0s: Notification sent

T+3s: PostgreSQL pool exhausted

T+5s: Node.js queue buildup

T+8s: DB rejects new connections

T+10s: Payment timeouts

T+12s: Promo oversold

T+15s: NIC saturation

T+18s: Node.js OOM crash

T+45m: Root cause identified

T+2h: Service restored

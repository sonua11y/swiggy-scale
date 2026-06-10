# Failure Cascade Analysis

## Traffic Simulation
- Users notified: 180M
- CTR: 8%
- Active users: 14.4M
- API calls/user: 3

Peak RPS = (10,000,000 × 3) / 60
= 500,000 RPS

## Capacity Limits
- PostgreSQL: 100 connections
- Node.js: ~12,000 RPS
- Payment hold time: 200–2000ms
- RAM: 4GB

## Failures
1. PostgreSQL Pool Exhaustion
2. Node.js Event Loop Saturation
3. Payment Call Amplification
4. Promo Race Condition
5. NIC Saturation

## Timeline
T+0s Notification sent
T+3s DB exhausted
T+5s Node backlog
T+10s Payment timeout
T+15s NIC saturation
T+18s Node crash
T+45m Root cause found
T+2h Recovery
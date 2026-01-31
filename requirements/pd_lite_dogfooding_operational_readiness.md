# PD Lite – Dogfooding & Operational Readiness (v1)

## Purpose

This document defines how PD Lite is operated internally, validated under stress, and proven reliable before and after general availability. Operational readiness is a prerequisite for trust.

PD Lite must be good enough to run PD Lite.

---

## Design Principles

1. We run incidents on the same system customers use
2. Failures are expected, rehearsed, and learned from
3. No special internal bypasses in production
4. Operational data drives product decisions
5. Readiness is continuously validated, not declared once

---

## Internal Usage (Dogfooding)

### Mandatory Internal Adoption

- All PD Lite engineering and product teams use PD Lite for incidents
- On-call schedules and escalation policies are real
- No shadow systems allowed

### Scope

- Production incidents
- Staging and pre-production incidents
- Planned game days and chaos exercises

---

## Incident Process Alignment

- Incident roles mirror customer roles
- Same workflows, permissions, and limitations
- No elevated privileges during incidents except break-glass

---

## Game Days and Chaos Testing

### Game Day Cadence

- Monthly planned game days
- Quarterly large-scale simulations

### Scenarios

- Telephony provider outage
- Chat platform outage
- AI system failure
- Regional cloud outage
- Data store read-only mode

### Success Criteria

- Paging works
- State preserved
- Clear communication
- Learnings captured

---

## Operational Metrics

### Reliability Metrics

- Incident paging latency
- Notification delivery success rate
- System availability
- Failover detection time

### Product Metrics

- Incident resolution time
- AI suggestion acceptance rate
- Manual override frequency

---

## Instrumentation and Observability

- Distributed tracing across incident flows
- Structured logs for all incident actions
- Metrics surfaced to operators and leadership

---

## Launch Readiness Checklist

Before GA:

- All failure modes exercised
- Audit logging verified
- Migration tools validated
- SLOs consistently met

---

## Continuous Improvement Loop

- Post-incident reviews required for internal incidents
- Product gaps tracked as backlog items
- Reliability regressions block releases

---

## PagerDuty Alignment

- Mirrors PagerDuty’s operational rigor
- Reinforces trust during acquisition diligence

---

## Exit Criteria

This document is complete when:

- PD Lite successfully handles its own incidents
- Game day learnings feed product improvements
- Leadership trusts PD Lite for mission-critical response


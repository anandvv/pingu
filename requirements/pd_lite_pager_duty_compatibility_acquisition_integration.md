# PD Lite – PagerDuty Compatibility & Acquisition Integration (v1)

## Purpose

This document defines how PD Lite is architected to integrate with, coexist alongside, and ultimately enhance or replace portions of PagerDuty’s existing incident management experience in the event of an acquisition or deep partnership.

PD Lite is designed to be buyable, not just buildable.

---

## Design Principles

1. Clean architectural seams
2. Replace experience before replacing infrastructure
3. Preserve PagerDuty reliability guarantees
4. Enable phased adoption, not forced rewrites
5. Minimize customer-facing disruption

---

## Architectural Boundaries

### Experience Layer

PD Lite owns:
- Chat-native incident control plane
- Modern web and mobile incident UX
- AI-driven incident orchestration

PagerDuty may continue to own:
- Legacy web UI during transition
- Existing customer workflows

---

### Orchestration Layer

PD Lite provides:
- Incident state machine
- Role assignment logic
- Timeline construction
- AI decision engine

Integration requirements:
- Event-based APIs
- Idempotent operations
- Dual-write capability during transition

---

### Communications Layer

PD Lite:
- Abstracts telephony providers
- Owns delivery guarantees and failover

PagerDuty:
- May integrate existing notification infrastructure
- Can selectively route traffic via PD Lite

---

## Identity and Account Alignment

### Identity Mapping

- Users mapped by immutable external IDs
- Teams and schedules mapped explicitly
- Support for dual identity during migration

### RBAC Compatibility

- PD Lite roles map to PagerDuty permission models
- No permission loss during transition

---

## Data Compatibility

### Incident and Event Data

- Schema alignment with PagerDuty incident primitives
- Support for shared incident IDs
- Event enrichment without mutation

### Analytics

- Data export compatible with PagerDuty data lake
- Supports unified reporting post-acquisition

---

## Phased Integration Plan

### Phase 1: Standalone Product

- PD Lite operates independently
- Integration via APIs and webhooks

---

### Phase 2: Experience Overlay

- PD Lite replaces incident UX
- PagerDuty backends remain authoritative

---

### Phase 3: Deep Integration

- Shared services where appropriate
- Unified admin and configuration experience

---

## Customer Migration Post-Acquisition

- No forced migrations
- Preserve contracts and pricing
- Progressive enablement of PD Lite features

---

## Operational Alignment

- Shared SLOs and reliability targets
- Common incident response processes
- Integrated support escalation

---

## Exit Criteria

This document is complete when:

- PagerDuty engineering can review and understand integration seams
- Product leadership can articulate buy vs build rationale
- Customers can adopt PD Lite without disruption

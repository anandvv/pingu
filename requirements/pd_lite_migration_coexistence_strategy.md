# PD Lite – Migration & Coexistence Strategy (v1)

## Purpose

This document defines how customers migrate to PD Lite from existing incident management systems with minimal risk, zero downtime, and high confidence. Migration is a first-class product capability, not a services afterthought.

---

## Design Principles

1. No forced big-bang migrations
2. Incidents must not be disrupted
3. Customers control timing and scope
4. Parallel run is always supported
5. Rollback must be possible at every stage

---

## Supported Migration Sources (v1)

- PagerDuty
- Rootly
- incident.io

Future sources must follow the same import and coexistence model.

---

## Migration Modes

### 1. Progressive Migration (Recommended)

- Customers opt-in service by service
- New incidents handled in PD Lite
- Legacy system remains authoritative for non-migrated services

**Benefits**
- Low risk
- Minimal organizational disruption

---

### 2. Parallel Run

- Incidents mirrored into PD Lite
- PD Lite used for visibility and AI insights
- Paging remains in legacy system initially

**Benefits**
- Trust building
- Validation before cutover

---

### 3. Incident-by-Incident Opt-In

- Each incident declared with target system
- Teams learn PD Lite organically

**Benefits**
- Ideal for early adopters and pilots

---

### 4. Full Cutover

- All services move at once
- Requires validation and sign-off

**Benefits**
- Simplicity post-migration

---

## Migration Scope

### Importable Entities

- Services
- Teams and users
- On-call schedules
- Escalation policies
- Integrations
- Historical incidents (read-only)

### Non-Importable (v1)

- Custom scripts or extensions
- Deprecated integrations

---

## Migration Tooling Requirements

### Import Tools

- Read-only source validation
- Schema compatibility checks
- Preview before apply

### Diff and Validation

- Side-by-side comparison
- Configuration drift detection

### Rollback

- One-click rollback per service
- Incident-safe rollback boundaries

---

## Coexistence Model

- PD Lite and legacy systems can coexist indefinitely
- Webhooks and APIs synchronize incident state where required
- Clear source-of-truth per incident

---

## Risk Management

### Pre-Migration

- Readiness checklist
- Simulation runs
- Stakeholder sign-off

### During Migration

- Live monitoring
- Automatic rollback triggers

### Post-Migration

- Validation window
- Metrics comparison
- Support escalation path

---

## PagerDuty-Specific Considerations

- Preserve PagerDuty incident IDs for correlation
- Support dual-write during transition
- Enable PD Lite UI on top of PagerDuty backends (future)

---

## Exit Criteria

This document is complete when:

- Customers can migrate without downtime
- Rollback is demonstrably safe
- PagerDuty customers can adopt PD Lite incrementally

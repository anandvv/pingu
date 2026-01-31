# PD Lite – Data Ownership, Portability & Schema (v1)

## Purpose

This document defines data ownership, access, portability, and schema requirements for PD Lite. These requirements ensure customer trust, regulatory compliance, seamless migration, and future compatibility with PagerDuty’s data and analytics platforms.

Data is a first-class product surface.

---

## Design Principles

1. Customers own their incident data
2. Derived data must be explainable and exportable
3. No data lock-in
4. Schemas are explicit and versioned
5. AI learning must respect tenant boundaries

---

## Data Ownership Model

### Customer-Owned Data

The customer owns:

- Incident records
- Timeline events
- Notifications and delivery metadata
- Roles and assignments
- Post-incident reviews
- Configuration data (services, schedules, escalation policies)

### System-Owned Metadata

PD Lite owns:

- Internal system metrics
- Platform health telemetry
- Aggregated, anonymized usage analytics

System-owned data must never expose customer-identifiable content.

---

## AI-Derived Data

### Classification

AI-derived artifacts include:

- Severity classification suggestions
- Service attribution
- Timeline summaries
- Postmortem drafts
- Pattern and trend analysis

### Ownership and Control

- AI-derived data is owned by the customer
- Customers can:
  - View inputs and outputs
  - Edit or discard AI-generated artifacts
  - Disable learning from their data

---

## Data Isolation and Multi-Tenancy

- Strict tenant isolation at storage and processing layers
- No cross-tenant learning by default
- Optional opt-in anonymized learning

---

## Core Data Schemas (v1)

### Incident

- Incident ID
- Title
- Description
- Severity
- Status
- Affected services
- Declared timestamp
- Resolved timestamp
- Current IC

### Timeline Event

- Event ID
- Incident ID
- Timestamp
- Actor (human, AI, system)
- Event type
- Event payload

### Notification Event

- Notification ID
- Incident ID
- Channel
- Provider
- Delivery attempts
- Acknowledgement state

### Post-Incident Review

- Incident ID
- Summary
- Root cause
- Action items
- Owners
- Completion status
n
Schemas are versioned and backward compatible.

---

## Data Portability Requirements

### Export Capabilities

Customers must be able to export:

- All incident data
- Audit logs
- Configuration
- AI-generated artifacts

Exports must support:
- JSON
- CSV
- NDJSON for large datasets

---

## Deletion and Retention

### Retention Policies

- Configurable retention by data type
- Default retention aligned with enterprise expectations

### Deletion

- Secure deletion workflows
- Customer-initiated deletion
- Verification of completion

---

## APIs and Access

- Public, documented APIs for all core data objects
- Read and write access governed by RBAC
- Rate-limited and observable

---

## PagerDuty Compatibility

This data model is intentionally compatible with PagerDuty’s incident, event, and analytics primitives to support:

- Migration from PagerDuty to PD Lite
- Partial coexistence
- Post-acquisition integration

---

## Exit Criteria

This document is complete when:

- Customers can fully export and delete their data
- AI data usage boundaries are explicit
- PagerDuty can map schemas to its existing data lake and analytics pipelines


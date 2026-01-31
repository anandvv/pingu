# PD Lite – Integrations: CMDB & Service Catalog (v1)

## Purpose

This document defines requirements for integrating PD Lite with external CMDB and service catalog systems so PD Lite can reliably map incidents to services, ownership, dependencies, and business context.

This capability improves triage accuracy, responder routing, AI grounding, and post-incident insights.

---

## Design Principles

1. Service context must be accurate and up to date
2. Integrations must be safe by default and least-privileged
3. Sync must be incremental, observable, and resilient
4. PD Lite must work without a CMDB, but be better with one
5. The service catalog becomes the grounding layer for AI

---

## Supported Sources (v1)

### CMDB / ITSM

- ServiceNow CMDB

### Service Catalog

- Backstage (Spotify)

Future sources must be supported through the same connector framework.

---

## Core Data Model (Service Context)

PD Lite must maintain a canonical internal service model that can be populated from multiple sources:

### Service

- Service ID
- Service name
- Description
- Owning team
- On-call schedule reference
- Tier or criticality
- Runbooks and docs links
- Repositories and code owners

### Dependency Graph

- Upstream and downstream dependencies
- Critical path dependencies (optional)

### Business Context (optional)

- Customer impact profile
- Revenue or priority tags
- Compliance classification
n
---

## Connector Framework and Extensibility (Open Architecture)

PD Lite must support an open, declarative connector framework so new service catalog and CMDB sources can be added with minimal code changes.

### Extension Goals

- Add new sources via configuration whenever possible
- Standardize authentication, sync, webhooks, and rate limiting
- Allow custom logic through a narrow plugin interface when configuration is insufficient

### Connector Types

1. **Declarative Connectors (Preferred)**
   - Defined using config files
   - Use standard HTTP APIs and webhooks
   - Support field mapping and transforms

2. **Plugin Connectors (Fallback)**
   - Used only when a source requires complex protocols or custom auth
   - Implement a minimal interface for fetch, webhook verify, and normalize

### Declarative Connector Spec (Config-Driven)

A connector must be describable via a versioned configuration that includes:

- Base URL and endpoints
- Auth type and credential references
- Entity types supported (service, component, system, CI)
- Field mappings to PD Lite canonical model
- Relationship extraction rules
- Pagination, rate limits, retry policy
- Webhook subscriptions and signature verification

Config files must be:
- Versioned
- Validated before activation
- Audited on change

### Normalization Pipeline

All connectors must output events in a canonical form:

- `service.upsert`
- `service.delete`
- `dependency.upsert`
- `dependency.delete`
- `ownership.update`

These events feed the internal service catalog store and the incident context layer.

### API and Webhook Strategy

- Prefer webhooks for freshness when supported
- Fall back to incremental polling where webhooks are not available
- Always support a last-known-good snapshot

---

## Connector Requirements

### Authentication

- OAuth or token-based auth where supported
- Support for service accounts
- Scoped permissions

### Sync Modes

- Initial full sync
- Incremental sync
- Webhook-driven updates where supported

### Conflict Resolution

- Define source of truth per field
- Manual overrides supported with audit logging
- Drift detection

### Observability

- Sync health dashboard
n- Last successful sync time
n- Error reporting and retry status
n
---

## ServiceNow Integration Requirements

### Data Ingestion

- Import CI records relevant to services
- Map CI to PD Lite service entities
- Import ownership metadata where available

### Linking

- Link incidents to ServiceNow CIs
- Optional automatic ticket creation
- Two-way status synchronization (optional)

### Controls

- Admin-selectable CI scope
- Rate limiting and backoff
- Audit log for sync and ticket actions

---

## Backstage Integration Requirements

### Data Ingestion

- Import catalog entities (Component, System, API, Resource)
- Import ownership (team) and tags
- Import relationships and dependencies

### Linking

- Map Backstage entities to PD Lite services
- Show service metadata in incident context
- Deep links back to Backstage

### Controls

- Namespace and entity filtering
- Support for multiple Backstage instances

---

## Product Surface Requirements

### Admin Experience

- Guided connector setup
- Field mapping UI
- Preview before sync
- Ongoing sync status and alerts

### Incident Experience

- Service context panel (chat, web, mobile)
- Dependency view
- Ownership and escalation confidence

---

## AI Grounding Requirements

- AI context must include service metadata and dependencies
- AI should use catalog data to:
  - Suggest responders
  - Identify likely impacted downstream services
  - Suggest runbooks and relevant docs

AI must not hallucinate service ownership when catalog data exists.

---

## Failure Modes

- If connector fails, PD Lite continues operating using last-known-good snapshot
- Stale data indicators visible to admins and ICs
- Manual service selection always available

---

## Security and Compliance

- Store only necessary fields
- Encrypt at rest and in transit
- Tenant isolation preserved
- Audit logs for all connector actions

---

## Exit Criteria

This document is complete when:

- PD Lite can ingest and maintain an accurate service catalog
- Services can be linked to incidents with high confidence
- Admins can configure and monitor connector health
- AI uses service context for grounded recommendations


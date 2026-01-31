# PD Lite – Governance, Auditability & Controls (v1)

## Purpose

This document defines the governance, auditability, and control requirements required for PD Lite to be trusted by enterprise customers and to meet PagerDuty’s reliability, security, and compliance standards.

Governance is not an add-on. It is a core product capability.

---

## Design Principles

1. Every meaningful action is auditable
2. Incident history is append-only by default
3. Power exists, but is permissioned and observable
4. Emergency access is possible, but always traceable
5. Governance must not slow down incident response

---

## Audit Logging Requirements

### Scope of Audit Logs

The system must log the following actions at minimum:

- Incident lifecycle changes (declare, escalate, resolve, reopen)
- Role assignments and changes
- Manual and AI-initiated actions
- Notification delivery attempts and acknowledgements
- Configuration changes (services, schedules, escalation policies)
- Admin actions and permission changes
- Authentication and access events
- Break-glass access usage

### Audit Log Properties

Each audit log entry must include:

- Timestamp (UTC)
- Actor (human, AI agent, system)
- Actor identity and role
- Action type
- Target resource
- Before and after state (where applicable)
- Source (chat, web, API)
- Correlation ID

Audit logs are immutable and append-only.

---

## Incident Immutability Model

### Append-Only Timeline

- Timeline events cannot be deleted
- Corrections are added as new events
- Original entries remain visible

### Editable Fields

The following may be edited with permission:

- Incident title
- Incident description
- Severity classification

Edits must:
- Create an audit entry
- Preserve original values

---

## Role-Based Access Control (RBAC)

### Core Roles

- Responder
- Incident Commander
- Service Owner
- Admin
- Read-Only

### Permission Model

- Permissions are explicit and composable
- Defaults are conservative
- Temporary elevation is supported

---

## Break-Glass Access

### Purpose

Enable incident response when identity systems or permissions fail.

### Requirements

- Locally authenticated emergency accounts
- Time-bound access
- Mandatory justification
- Automatic alerting on usage
- Full audit trail

Break-glass access cannot be disabled.

---

## AI Governance

### AI Action Transparency

- All AI-suggested actions are labeled
- AI-executed actions require approval unless explicitly configured

### AI Auditability

- AI decisions are logged
- Input context and output summaries are retained
- Overrides are tracked for learning

---

## Compliance and Data Governance

### Baseline Compliance

- SOC 2 Type II readiness
- GDPR alignment
- Data residency support

### Retention Policies

- Configurable retention by data type
- Legal hold support
- Secure deletion workflows

---

## Admin Controls and Visibility

### Admin Dashboard Requirements

- Real-time view of incidents and actions
- Audit log search and export
- Permission review and drift detection
- Configuration change history

### Guardrails

- High-risk actions require confirmation
- Optional multi-approver workflows

---

## PagerDuty Alignment

This governance model is intentionally compatible with PagerDuty’s expectations:

- Supports enterprise compliance reviews
- Aligns with incident immutability standards
- Enables future integration into PagerDuty’s control plane

---

## Exit Criteria

This document is complete when:

- A security reviewer can approve the model
- An enterprise buyer sees no governance gaps
- PagerDuty can map these controls to its existing frameworks


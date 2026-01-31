# PD Lite – Killer Demo Moments & MVP Narrative (v1)

## Purpose

This document defines the core demo moments that communicate PD Lite’s value instantly and viscerally to AI startups and SMBs. These moments anchor product decisions, guide MVP scope, and serve as the canonical demo and sales narrative.

If these moments work flawlessly, the product wins. If they don’t, nothing else matters.

---

## Target Audience

- Founders and early engineers at AI startups
- Tech leads and first SREs
- Design partners evaluating incident tools
- Early-stage investors

---

## Demo Moment #1: Declare an Incident and AI Instantly Imposes Order

### The Moment

An engineer types a single command in Slack:

`/incident declare api latency spike`

Within seconds, PD Lite:

- Declares an incident
- Assigns a severity (with confidence level)
- Suggests an Incident Commander
- Creates a structured timeline
- Proposes next steps

Chaos turns into structure immediately.

### Why This Matters

During incidents, humans are overloaded. The first 5 minutes determine whether the incident stays manageable or spirals.

PD Lite’s AI acts as a calm, opinionated copilot that creates clarity without taking control away.

### MVP Requirements

- Slack-first incident declaration
- AI severity suggestion (no auto-enforcement)
- AI responder suggestion
- Timeline auto-construction

### Explicit Non-Goals (v1)

- No autonomous paging
- No AI-driven incident resolution

---

## Demo Moment #2: Service Context Appears Automatically

### The Moment

As soon as the incident is declared, PD Lite surfaces:

- Owning team
- On-call reference
- Service description
- Upstream and downstream dependencies
- Relevant runbooks and repos

All without manual configuration during the incident.

### Why This Matters

Responders waste precious time figuring out:
- Who owns this service?
- What does it depend on?
- Who should I wake up?

PD Lite answers these questions instantly by grounding incidents in real service data.

### MVP Requirements

- Backstage integration (config-driven)
- Canonical internal service model
- Service context panel in Slack and web UI

### Explicit Non-Goals (v1)

- No ServiceNow
- No multi-source reconciliation
- Manual service override always available

---

## Demo Moment #3: Slack Is Down, the Incident Continues

### The Moment

Slack becomes unavailable mid-incident.

PD Lite:

- Continues paging via SMS and voice
- Allows incident control via web and mobile
- Preserves incident state and timeline
- Clearly communicates degraded mode

The incident does not stop because a chat tool failed.

### Why This Matters

Slack-native tools collapse when Slack collapses.

PD Lite proves it is an incident management system, not a Slack workflow.

### MVP Requirements

- Web UI as secondary control plane
- SMS and voice paging
- Multi-provider telephony failover
- Clear degraded-mode messaging

### Explicit Non-Goals (v1)

- No full mobile app
n
---

## Demo Moment #4: AI Drafts the Update, Humans Stay in Control

### The Moment

Mid-incident, PD Lite posts a suggested status update:

> “We are investigating elevated API latency impacting a subset of customers. The team is actively mitigating and will provide an update in 15 minutes.”

The Incident Commander edits or approves it with one click.

### Why This Matters

Writing updates under pressure is error-prone and distracting.

PD Lite removes the toil without removing accountability.

### MVP Requirements

- AI-drafted internal and external updates
- Human approval required
- Update cadence reminders

---

## What Is Intentionally Missing in the MVP

To stay fast and focused, PD Lite v1 does NOT include:

- SCIM provisioning
- SAML-based SSO
- Complex RBAC models
- ServiceNow or ITSM workflows
- Custom approval chains
- Autonomous AI actions

These omissions are deliberate, not gaps.

---

## MVP Success Criteria

The MVP is successful if:

- A team can run a real incident within 15 minutes of signup
- Responders trust the AI suggestions
- Incidents remain manageable even when Slack fails
- Design partners ask to keep using the product

---

## How to Use This Document

- As the canonical demo script
- As the MVP scope guardrail
- As the emotional core of the product narrative

If a feature does not strengthen one of these demo moments, it does not belong in the MVP.


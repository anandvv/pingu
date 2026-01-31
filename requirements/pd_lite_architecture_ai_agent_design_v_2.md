# PD Lite – Architecture & AI Agent Design (v2)

## Purpose

This document defines the system architecture and AI agent behavior for PD Lite, explicitly scoped for the MVP targeting AI startups and SMBs. It clarifies responsibilities, boundaries, and guardrails so the system is powerful without being unsafe or over-automated.

This version adds an explicit **MVP AI behavior contract** to prevent autonomy creep and ensure trust under pressure.

---

## Architectural Principles

1. Event-driven and resilient by default
2. Clear separation between control, orchestration, and execution
3. AI operates as a proposal engine, never as an autonomous actor
4. No single dependency can block incident response
5. Architecture must scale without redesign as customers grow

---

## High-Level System Architecture

### Core Layers

1. **Experience Layer**
   - Slack integration (primary)
   - Web UI (secondary control plane)
   - Mobile web (fallback)

2. **Control Plane**
   - Incident lifecycle service
   - Role assignment and escalation service
   - Timeline and state management

3. **AI Orchestration Layer**
   - Context assembly and grounding
   - Agent coordination and proposal engine
   - Learning and feedback capture

4. **Execution Layer**
   - Notification and telephony abstraction
   - Integration adapters (monitoring, service catalog)

5. **Data Layer**
   - Incident and event stores (append-only)
   - Audit logs
   - Configuration and connector state

---

## Event-Driven Model

- All significant actions emit immutable events
- Events are the source of truth for:
  - Incident state
  - Timeline construction
  - AI context
- Enables replay, recovery, and post-incident learning

---

## AI Agent Model

### Core Agents (v1)

#### Incident Triage Agent

- Suggests incident severity with confidence
- Identifies likely owning service
- Suggests initial responders

#### Incident Commander Copilot

- Tracks roles and responsibilities
- Detects missing ownership or long silence
- Suggests next actions

#### Investigation Agent

- Correlates signals from monitoring tools
- Tracks hypotheses and findings
- Surfaces likely causes without asserting certainty

#### Communications Agent

- Drafts internal and external status updates
- Enforces update cadence
- Adapts tone by audience

#### Post-Incident Review Agent

- Builds structured timelines
- Drafts post-incident review outlines
- Identifies recurring patterns over time

---

## MVP AI Behavior Contract (AI Startups & SMBs)

This section is authoritative for MVP behavior.

### First 0–5 Minutes (Incident Declaration)

**AI WILL:**
- Suggest severity with a confidence indicator
- Suggest owning service (if available)
- Suggest Incident Commander and initial responders
- Initialize a structured, append-only timeline

**AI WILL NOT:**
- Auto-page responders
- Change severity autonomously
- Close, downgrade, or suppress incidents

---

### Minutes 5–30 (Active Investigation)

**AI WILL:**
- Continuously update the timeline
- Detect missing roles or prolonged silence
- Suggest investigative next steps
- Draft internal status updates

**AI WILL NOT:**
- Execute remediation actions
- Notify customers or external stakeholders without approval
- Escalate beyond configured policies

---

### Resolution Phase

**AI WILL:**
- Draft a resolution summary
- Prepare a post-incident review outline

**AI WILL NOT:**
- Mark incidents as resolved
- Assign blame or assert definitive root cause

---

## Context and Grounding

- AI operates on structured incident events and service catalog data
- Context windows are explicit and bounded
- No cross-tenant learning
- No training on customer data without opt-in

---

## Failure Handling

- AI failures degrade gracefully
- Manual controls are always available
- Incident state is independent of AI availability

---

## Extensibility

- New agents can be added behind feature flags
- Agent capabilities are versioned
- MVP contract remains the default until explicitly expanded

---

## Security and Privacy

- AI access governed by RBAC
- All AI proposals and approvals are audited
- Sensitive fields masked where required

---

## Exit Criteria

This document is complete when:

- Engineers can implement AI behavior without ambiguity
- No AI action can surprise a user
- MVP scope is protected from autonomy creep

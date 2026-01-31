# PD Lite – Architecture & AI Agent Design (v1)

## Purpose

This document defines the system architecture and AI agent design for PD Lite. The goal is to deliver an AI-first incident management platform where intelligent agents actively assist responders while preserving human control, reliability, and enterprise trust.

AI is a core system capability, not a feature layer.

---

## Architectural Principles

1. Event-driven and resilient by default
2. Clear separation between control, orchestration, and execution
3. AI operates as a proposal engine with explicit approvals
4. No single dependency can block incident response
5. Architecture must scale from SMB to enterprise without redesign

---

## High-Level System Architecture

### Core Layers

1. **Experience Layer**
   - Chat integrations (Slack, Teams)
   - Web UI
   - Mobile UI

2. **Control Plane**
   - Incident lifecycle service
   - Role and escalation service
   - Timeline and state management

3. **AI Orchestration Layer**
   - Agent coordination service
   - Context assembly and grounding
   - Decision proposal engine

4. **Execution Layer**
   - Notification and telephony abstraction
   - Integration adapters (monitoring, ticketing)
   - External action executors

5. **Data Layer**
   - Incident and event stores
   - Audit logs
   - Feature flags and configuration

---

## Event-Driven Model

- All significant actions emit immutable events
- Events are the source of truth for:
  - Incident state
  - Timeline construction
  - AI context
- Enables replay, recovery, and learning

---

## AI Agent Model

### Core Agents (v1)

#### Incident Triage Agent

- Classifies incident severity
- Identifies affected services
- Suggests initial responders

#### Incident Commander Copilot

- Tracks roles and responsibilities
- Detects missing ownership or silence
- Suggests next actions

#### Investigation Agent

- Correlates signals from monitoring tools
- Tracks hypotheses and findings
- Surfaces likely root causes

#### Communications Agent

- Drafts status updates
- Ensures update cadence
- Adapts messaging by audience

#### Post-Incident Review Agent

- Builds incident timelines
- Drafts postmortems
- Identifies recurring patterns

---

## Decision Loops and Guardrails

### Proposal Flow

1. Agent generates a proposal
2. Proposal is shown with rationale and confidence
3. Human approves, modifies, or rejects
4. Outcome is logged and learned from

### Guardrails

- No autonomous paging without explicit configuration
- No external communications without approval
- AI actions always auditable

---

## Context and Grounding

- AI operates on structured incident events
- Context windows are explicit and bounded
- No training on customer data without opt-in

---

## Failure Handling

- AI failures degrade gracefully
- Manual controls always available
- Incident state independent of AI availability

---

## Extensibility

- New agents can be added without changing core services
- Agent capabilities versioned and feature-flagged

---

## Security and Privacy

- AI access governed by RBAC
- Prompt inputs logged and traceable
- Sensitive data masked where required

---

## Exit Criteria

This document is complete when:

- AI responsibilities are clearly scoped
- Human control points are explicit
- Architecture supports reliability, scale, and learning


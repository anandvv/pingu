# PD Lite – Master Product Requirements Document (Executive Edition v2)

## Executive Summary

PD Lite is an AI-first, chat-native incident management platform built for AI startups and SMBs that need reliability under pressure without enterprise drag. It combines a Slack-first control plane, AI copilots that impose structure during chaos, and resilient multi-channel notifications that continue to function even when chat platforms fail.

PD Lite is designed to be trusted at 2am by small, fast-moving teams who cannot afford brittle workflows, heavy configuration, or per-seat pricing. The product is opinionated by design, optimized for speed to value, and priced on consumption rather than headcount.

---

## Product Vision

### Vision Statement

Make incident response calm, structured, and reliable for modern engineering teams, even when everything else is breaking.

### Long-Term Aspiration

PD Lite aims to become the default incident control plane for AI-first companies: a system that responders trust instinctively, that scales with company growth, and that embeds AI as a practical copilot rather than an autonomous risk.

---

## Target Customers

### Primary ICP

- AI startups and SMBs
- 10–150 engineers
- Slack-centric collaboration
- No dedicated NOC or operations teams
- High tolerance for opinionation, low tolerance for setup friction

### Buyers and Champions

- Founders
- Heads of Engineering
- Tech Leads and first SREs

---

## Problem Statement

Modern incident response is broken for small and fast-growing teams:

- Incidents unfold in chat, but tools fragment control across dashboards
- Slack-native tools fail when Slack fails
- Legacy incident platforms are reliable but slow, complex, and expensive
- AI features are bolted on as summaries instead of embedded into decision-making
- Per-seat pricing penalizes collaboration during incidents

Teams need a system that works when they are least capable of working carefully.

---

## Core Product Principles

1. Reliability beats elegance under pressure
2. Chat is the primary control plane, not the only one
3. AI assists humans but never replaces them
4. Configuration should feel optional
5. Pricing should scale with value, not headcount

---

## Killer Demo Moments (MVP Anchor)

PD Lite is intentionally built around a small number of high-impact demo moments that communicate value instantly and serve as guardrails for MVP scope.

1. **Declare an incident and AI imposes order**  
   A single Slack command creates structure, assigns severity, suggests responders, and builds a live timeline.

2. **Service context appears automatically**  
   Ownership, dependencies, runbooks, and repos surface immediately using service catalog data, without manual setup during the incident.

3. **Slack is down, the incident continues**  
   Paging, control, and state persist via web and SMS even when chat platforms fail.

4. **AI drafts updates, humans stay in control**  
   Status updates are suggested, reviewed, and sent without removing accountability.

These moments are defined in detail in **PD Lite – Killer Demo Moments & MVP Narrative** and represent the primary success criteria for the MVP.

---

## Core Capabilities (MVP)

### Incident Lifecycle Management

- Slack-first incident declaration
- Clear incident states and timeline
- Human-controlled escalation and resolution

### AI Copilot

- Severity and impact suggestions
- Responder and role suggestions
- Timeline construction and synthesis
- Drafted internal and external updates

AI actions are always advisory and require human approval.

### Service Context

- Backstage-backed service catalog (optional)
- Ownership and dependency visibility
- Runbook and repository links surfaced during incidents

### Resilient Communications

- SMS and voice paging
- Redundant telephony providers
- Automatic failover
- Delivery visibility

---

## Non-Goals (Explicit MVP Exclusions)

To maintain focus and speed, the MVP explicitly excludes:

- SCIM provisioning
- SAML-based SSO
- Complex RBAC models
- ServiceNow or ITSM workflows
- Autonomous AI actions
- Custom approval chains

These are deliberate omissions, not gaps.

---

## Pricing and Go-To-Market Summary

- Consumption-based pricing
- Free tier capable of handling real incidents
- Paid tiers scale on incidents and notifications, not seats
- Product-led growth with founder-friendly onboarding

---

## Success Metrics

- First incident run within 15 minutes of signup
- High trust in AI suggestions
- Zero incident state loss during failures
- Design partners convert into long-term users

---

## Supporting Requirements

This PRD is supported by detailed, standalone requirements documents covering:

- Failure Modes & Degraded Operations
- Governance, Auditability & Controls
- Data Ownership, Portability & Schema
- Migration & Coexistence Strategy
- Architecture & AI Agent Design
- Dogfooding & Operational Readiness
- Admin Experience & Onboarding
- Pricing, Packaging & Go-To-Market
- Service Catalog & CMDB Integrations
- Identity, SSO & OAuth

---

## Intended Audience

- Founders and early team members
- Design partners
- Seed and Series A investors
- Technical advisors

---

## Status

This document represents the authoritative, startup-ready Master PRD for PD Lite and is suitable for external circulation.

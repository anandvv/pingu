# PD Lite – Admin Experience & Onboarding (v1)

## Purpose

This document defines the admin and onboarding experience for PD Lite. The goal is to deliver a modern, intuitive, and low-friction experience that matches or exceeds the best SaaS admin products in the market while retaining enterprise-grade power.

Admin experience is a product feature, not a configuration tax.

---

## Design Principles

1. Intent-first, not settings-first
2. Progressive disclosure of complexity
3. Safe defaults that work out of the box
4. Every configuration has an explanation
5. Time-to-first-incident is the core success metric

---

## Target Benchmarks

PD Lite admin experience should feel comparable to:

- Stripe Dashboard
- Vercel
- Linear
- Notion

And explicitly avoid:

- Nested configuration trees
- Wall-of-checkbox experiences
- YAML-first workflows

---

## Onboarding Experience

### SMB Onboarding (Under 15 Minutes)

1. OAuth install from Slack or Teams
2. Connect one monitoring source
3. AI-generated default services and escalation
4. Test incident walkthrough
5. First live incident ready

### Enterprise Onboarding (Under 1 Day)

- SSO configuration
- RBAC review
- Environment separation
- Migration tooling access
- Admin readiness checklist

---

## Guided Setup Flows

### Services

- AI suggests service boundaries
- Admin confirms or edits
- Live preview of incident routing

### On-Call Schedules

- Drag-and-drop editor
- Visual coverage gaps
- Automatic sanity checks

### Escalation Policies

- Visual flow builder
- Simulation mode
- Clear explanation of escalation behavior

---

## Admin Console Capabilities

### Configuration Management

- Versioned configuration
- Change history
- Rollback support

### Visibility

- Real-time incident overview
- Notification health
- Provider status

### Safety Controls

- High-risk action confirmations
- Optional multi-approver workflows

---

## AI Assistance in Admin Experience

- AI explains what a setting does
- AI flags risky configurations
- AI suggests optimizations based on usage

AI never applies changes without approval.

---

## UX Requirements

- Fast load times
- Inline help and documentation
- Live previews for configuration changes

---

## PagerDuty Alignment

- Designed to replace PagerDuty admin UX incrementally
- Compatible with PagerDuty configuration primitives

---

## Exit Criteria

This document is complete when:

- A new customer can onboard without professional services
- Admins can explain their setup confidently
- Configuration errors are rare and recoverable

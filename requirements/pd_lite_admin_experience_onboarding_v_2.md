# PD Lite – Admin Experience & Onboarding (v2)

## Purpose

This document defines the admin and onboarding experience for PD Lite, explicitly scoped for AI startups and SMBs. The goal is to achieve **time-to-first-incident under 15 minutes** while preserving safety, clarity, and confidence.

Admin experience is not a control surface first; it is an enablement surface.

---

## Design Principles

1. Speed to value beats completeness
2. Defaults must work without configuration
3. Every setting must explain itself
4. Admin actions must be reversible
5. Complexity is earned, not exposed upfront

---

## Target Users

- Founders
- Heads of Engineering
- First SRE or Tech Lead

These users are time-poor, context-switching, and allergic to setup overhead.

---

## MVP Onboarding Path (AI Startups & SMBs)

### Success Definition

A team is considered onboarded when they can:
- Declare an incident in Slack
- Page a real human
- See AI-imposed structure
- Resolve the incident

All within 15 minutes of signup.

---

## Step-by-Step Onboarding Flow

### Step 1: Sign Up and Identity

- Email-based signup
- OIDC login via Google or GitHub
- No SAML
- No SCIM
- Local break-glass account auto-created

---

### Step 2: Slack Installation (Required)

- OAuth-based Slack app install
- Minimal permissions
- Immediate channel-level feedback confirming install

Slack is the primary control plane for the MVP.

---

### Step 3: Notification Setup (Guided)

- SMS and voice enabled by default
- One-click phone number verification
- Test page flow with confirmation

No escalation policies required in MVP.

---

### Step 4: Optional Service Context (Backstage)

- Clearly marked as optional
- Guided connector setup
- Preview of imported services and ownership

Skipping this step must not block incident response.

---

### Step 5: Test Incident Walkthrough

- Guided test incident
- AI explains what it is doing and why
- Clear success confirmation

This step builds trust before the first real incident.

---

## Admin Console Capabilities (MVP)

### Configuration

- View notification settings
- View connected integrations
- View AI feature toggles

No advanced workflow builders in MVP.

---

### Visibility

- Active and recent incidents
- Notification delivery health
- Connector sync status (if enabled)

---

## AI Assistance in Admin Experience

- AI explains configuration choices
- AI flags risky setups
- AI never auto-applies changes

---

## Explicit Non-Goals (MVP)

The MVP explicitly excludes:

- Complex RBAC
- Approval chains
- Multi-environment setup
- Compliance dashboards
- Custom workflows

These are future expansions.

---

## Failure and Safety Considerations

- Admin UI outages must not block incidents
- Slack failure triggers web-based fallback
- All admin changes are audited

---

## UX Requirements

- Sub-second page loads
- Inline explanations
- No dead ends
- Clear escape hatches

---

## Exit Criteria

This document is complete when:

- A new team can onboard without documentation
- No professional services are required
- Admins feel confident, not overwhelmed
- The onboarding flow reinforces the killer demo moments
